
# Ambianic Configuration

Ambianic Edge executes pipelines with input sources, AI inference models and
output targets. Ambianic UI visualizes observations in a chronological timeline.

Pipelines are configured in a `config.yaml` file which resides in the
runtime directory of the Ambianic Edge docker image.

The following diagram illustrates an example pipeline that takes as input a video stream
 URI source such as a surveillance camera and
outputs object detections to a local directory.

<div class="diagram">
st=>start: Video Source
op_obj=>operation: Object Detection AI
op_sav1=>parallel: Storage Element
io1=>inputoutput: save object detections to file
op_face=>operation: Face Detection AI
op_sav2=>parallel: Storage Element
io2=>inputoutput: save face detections to file
e=>end: Output to other pipelines

st->op_obj
op_obj(bottom)->op_sav1
op_sav1(path1, bottom)->op_face
op_sav1(path2, right)->io1
op_face(bottom)->op_sav2
op_sav2(path1, bottom)->e
op_sav2(path2, right)->io2
</div>

<script>
$(".diagram").flowchart();
</script>

<br/>

## Pipeline Configuration

Create a `config.yaml` file in the workspace directory of the
Ambianic Edge docker image.
For example, the directory could be named `/opt/ambianid-edge.workspace`.

You can [use this starting config.yaml template](https://raw.githubusercontent.com/ambianic/ambianic-quickstart/master/config.yaml)
and modify it to fit your environment. Read further about the parameters and format of the config file.

Let's take an example `config.yaml` file and walk through it:

```YAML
######################################
#  Ambianic main configuration file  #
######################################
version: '2020.12.11'

# path to the data directory
# ./data is relative to the container runtime. Do not change it if you are unsure.
data_dir: ./data

# Set logging level to one of DEBUG, INFO, WARNING, ERROR
logging:
  file: ./data/ambianic-log.txt
  level: INFO

Notifications provider configuration
# see https://github.com/caronc/apprise#popular-notification-services for syntax examples
notifications:
  catch_all_email:
    include_attachments: true
    providers:
      - mailto://userid:pass@domain.com
      - hassios://{host}/{access_token}
  alert_fall:
    providers:
      - mailto://userid:pass@domain.com
      - json://hostname/a/path/to/post/to


# Pipeline event timeline configuration
timeline:
  event_log: ./data/timeline-event-log.yaml

# Cameras and other input data sources
# Using Home Assistant conventions to ease upcoming integration
sources:

  # direct support for raspberry picamera
  picamera:
    uri: picamera
    type: video
    live: true
    
  front_door_camera: 
    uri: <your camera>
    type: video
    live: true

  # local video device integration example
  webcam:
    uri: /dev/video0
    type: video
    live: true
    
  # recorded front door cam feed for quick testing
  recorded_cam_feed:
    uri: file:///workspace/tests/pipeline/avsource/test2-cam-person1.mkv
    type: video
    
ai_models:
  image_detection:
    model:
      tflite: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_coco_quant_postprocess.tflite
      edgetpu: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite
    labels: /opt/ambianic-edge/ai_models/coco_labels.txt
  face_detection:
    model:
      tflite: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_face_quant_postprocess.tflite
      edgetpu: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_face_quant_postprocess_edgetpu.tflite
    labels: /opt/ambianic-edge/ai_models/coco_labels.txt
    top_k: 2
  fall_detection:
    model:
      tflite: /opt/ambianic-edge/ai_models/posenet_mobilenet_v1_100_257x257_multi_kpt_stripped.tflite
      edgetpu: /opt/ambianic-edge/ai_models/posenet_mobilenet_v1_075_721_1281_quant_decoder_edgetpu.tflite
    labels: /opt/ambianic-edge/ai_models/pose_labels.txt

# A named pipeline defines an ordered sequence of operations
# such as reading from a data source, AI model inference, saving samples and others.
pipelines:
   # Pipeline names could be descriptive, e.g. front_door_watch or entry_room_watch.
   area_watch:
     - source: picamera
     - detect_objects: # run ai inference on the input data
        ai_model: image_detection
        confidence_threshold: 0.6
        # Watch for any of the labels listed below. The labels must be from the model trained label set.
        # If no labels are listed, then watch for all model trained labels.
        label_filter:
          - person
          - car
     - save_detections: # save samples from the inference results
        positive_interval: 300 # how often (in seconds) to save samples with ANY results above the confidence threshold
        idle_interval: 6000 # how often (in seconds) to save samples with NO results above the confidence threshold
     - detect_falls: # look for falls
        ai_model: fall_detection
        confidence_threshold: 0.6
     - save_detections: # save samples from the inference results
        positive_interval: 10
        idle_interval: 600000
        notify: # notify a thirdy party service
          providers:
            - alert_fall        

```

The only parameter you have to change in order to see a populated timeline in the UI is the source uri. In the section below:
```yaml
  front_door_camera:
    uri: <your camera>
```
 
 Replace `<your camera>` with your camera still image snapshot URI (recommended). For example:
 
```yaml
  front_door_camera:
    uri: http://192.168.86.29/cgi-bin/api.cgi?cmd=Snap&channel=0&rs=wuuPhkmUCeI9WG7C&user=admin&password=******
    type: image
    live: true
```

Or you can alternatively plug-in the camera video streaming RTSP URI. For example:

```yaml
  front_door_camera:
    uri: rtsp://admin:password@192.168.1.99/media/video1 
    type: video
    live: true
```

Only use RTSP if you run high end hardware and use AI inference in your pipelines that needs video and/or audio data instead of still images.

Make sure that the HTTP URI points to a valid image. You can test by simply putting the URI in your web browser. You should see a current snapshot of live camera capture.

If you choose to use an RTSP URI, then you can [test that](https://www.unifore.net/ip-video-surveillance/how-to-play-rtsp-video-stream-of-ip-cameras-on-vlc-player-quicktime-player.html) with a tool like [VLC](https://www.videolan.org/).

The parameter `live: true` indicates that this is a continuous stream without a predetermined end. Therefore Ambianic Edge should do whatever it can to continuously pull media and recover from interruptions caused by network or other glitches.

Now save the file and restart the docker image. Within a few moments you should be able to see a populated timeline in the UI with fresh images from your camera and some object, people and face detections if there are any to detect.

You can reference the [Quick Start Guide](quickstart.md) for instructions on starting and stopping the Ambianic Edge docker image.

## Camera Configuration

Cameras are some of the most common sources of input data for Ambianic.ai pipelines.

Ambianic.ai is typically connected to IP Network cameras, but you can also connect a local embedded web cam or USB connected camera.

You can set `uri` to a local path such as `/dev/video0` to use a working usb webcam or `picamera` in case you have a picamera connected to Raspbery Pi connector.

### Connecting a local Web USB camera

A laptop camera or USB camera on linux or mac can be used just providing a working reference to the video device, eg. `/dev/video0`

### Connecting a Picamera

To use a Picamera connected to the camera connector on the Raspberry Pi (usually available in the middle of the board)

Ensure also your setup have the camera enabled and provides enough GPU memory for processing frames. The following script will take care of it for you:

```sh

# Enable camera
sudo raspi-config nonint do_camera 1

# Allocate GPU to camera:
if ! grep -Fq "gpu_mem=" /boot/config.txt; then
  echo "gpu_mem=256" | sudo tee -a /boot/config.txt
fi
```

In a tipical setup your `/boot/config.txt` should contain something like this at the end

```ini
[all]
start_x=1
gpu_mem=256
```

Then update your configuration to point to the right source

```yaml
  picamera: 
    uri: picamera
    type: video
    live: true
```

### Connecting over HTTP or RTSP

Another option is to stream your local camera over HTTP or RTSP. VLC is one of the most popular and easy to use apps for that. 
[Here is how](https://espressolive.com/blog/how-to-webcast-in-vlc-media-player/) you can turn your local webcam into an IP streaming camera. The streaming URL shared by VLC is the input source URI for the Ambianic.ai configuration file.

### How to find the URI for your camera

If you don't have experience configuring surveillance cameras, it can be tricky to find the URI. We are working on a camera Plug-and-Play feature in Ambianic to make it easier to discover and connect to your cameras. Keep an eye for release news. 

In the meanwhile, you have several options:

First, check your camera manufacturer's documentation whether it supports still image URL over HTTP or video streaming over RTSP. Most IP cameras do, but not all. If your camera is ONVIF standard conformant, then you will have access to HTTP camera images and RTSP video stream. You can use the [ONVIF Search Tool](https://www.onvif.org/conformant-products/) to check if your camera is listed as conformant.

You can then use a tool such as ONVIF Device Manager to auto discover your IP camera and show you its RTSP and HTTP URI. Here is a [quick How-To](http://help.angelcam.com/en/articles/372646-how-to-find-a-rtsp-address-for-an-onvif-compatible-camera-nvr-dvr). A more detailed post on this tool is also [available here](https://learncctv.com/onvif-device-manager/).

Your camera manufacturer will likely have an online resource describing how to determine its RTSP address. It would look something like [this one](https://reolink.com/wp-content/uploads/2017/01/Reolink-CGI-command-v1.61.pdf).

There is also an [online directory](https://security.world/rtsp/) where you can search for the RTSP URI of many camera brands.

### Using still image source instead of a video stream for your camera URI

In many cases processing 1 frame per second is sufficient frequency to detect interesting events in your environment. It is usually more CPU and network resource efficient to use a still image source instead of live RTSP stream for 1fps. This approach allows Ambianic.ai Edge to pull from the camera another image snapshot when it is ready as opposed to streaming which pushes constantly media updates that Edge may or may not be ready to process.

All you have to do to use a still image source from your camera is to locate the specific image URI as described above and plug it in the corresponding `source` section of the `config.yaml` file. Here is what it would look like:

```yaml
  front_door_camera: 
    uri: http://192.168.86.29/cgi-bin/api.cgi?cmd=Snap&channel=0&rs=wuuPhkmUCeI9WG7C&user=admin&password=******
    type: image
    live: true
```

In the example above the URI points to a still jpg sample from the camera. Ambianic Edge will continuously poll this URI approximately once per second (1fps).

## Sensitive config information

Since camera URIs often contain credentials, we recommend that you store such values
in `secrets.yaml`. Ambianic Edge will automatically look for and if available
prepend `secrets.yaml` to `config.yaml`. That way you can share
 `config.yaml` with others without compromising privacy sensitive parameters.

You can use standard [YAML anchors and aliases](https://yaml.org/refcard.html) in `config.yaml` 
to reference YAML variables defined in `secrets.yaml`. They will resolve after the files are merged into one.

An example valid entry in `secrets.yaml` for a camera URI, would look like this:
```yaml
secret_uri_front_door_camera: &secret_uri_front_door_camera 'rtsp://user:pass@192.168.86.111:554/Streaming/Channels/101'
# add more secret entries as regular yaml mappings
```

It can be then referenced in `config.yaml` as follows:
```yaml
  front_door_camera: 
    uri: *secret_uri_front_door_camera
```

## Notification settings

Ambianic edge can be configured to instantly alerts users when a detection occurs. Notifications are a feature of the `save_detections` element. 
Every time a timeline event is saved along with its contextual data, a notification can be fired to a number of supported channels such as email, sms, or a local 
smart home hub such as [Home Assistant](https://github.com/caronc/apprise/wiki/Notify_homeassistant).

Notification providers are first configured at a system level using the following syntax:

```
notifications:
  catch_all_email:
    include_attachments: true
    providers:
      - mailto://userid:pass@domain.com
      - hassios://{host}/{access_token}
  alert_fall:
    providers:
      - mailto://userid:pass@domain.com
      - json://hostname/a/path/to/post/to
```

Notice that the `catch_all_email` provider template uses the `include_attachments` attribute. Email notifications to this provider will include timeline event attachments such as captured images. On the other hand the `alert_fall` does not use the `include_attachments` attribute. Notifications sent through this provider will include the essential event information, but no attachments. In order to include attachments via `alert_fall` , just add the `include_attachments: true` element.

For a full list of supported providers and config syntax, take a look at the [official apprise documentation](https://github.com/caronc/apprise#popular-notification-services). Apprise is a great notifications library that ambianic edge uses and sponsors.

Once the system level notification providers are configured, they can be referenced within `save_detection` elements as in the following example:

```
pipelines:
   # Pipeline names could be descriptive, e.g. front_door_watch or entry_room_watch.
   area_watch:
     ...
     - detect_falls: # look for falls
     ...
     - save_detections: # save samples from the inference results
        positive_interval: 10
        idle_interval: 600000
        notify: # notify a thirdy party service
          providers:
            - alert_fall        
```

Each time a detection event is saved, a corresponding notification will be instantly fired.


### Other configuration settings

The rest of the configuration settings are for advanced developers. 
If you feel ready to dive into log files and code, you can experiement with different AI models and pipeline elements. 
Otherwise leave them as they are.

## Using AI Accelerator

Ambianic Edge will use the [Google Coral TPU](https://coral.ai/) accelerator if one is available on the system.

_Note: The default docker compose configuration for Ambianic Edge checks whether a USB attached Coral is running on `/dev/bus/usb`. You may need to adjust that if the TPU is attached to a different file system directory._

Coral is a powerful TPU that can speed up inference 5-10 times. However AI inference is only part of
all the functions that execute in an Ambianic Edge pipeline. Video decoding and formatting
also takes substantial amount of processing time. Overall we've seen Coral to improve
performance 30-50% on a realistic Raspberry Pi 4 deployment with multiple simultaneous pipelines
pulling images from multiple cameras and processing these images with multiple inference models.

If you don't have a Coral device available, no need to worry for now. Raspberry Pi 4 is quite powerful.
It comfortably handles 3 simultaneous HD camera sourced pipelines with approximately 1-2 frames per second (fps).
An objects or person of interest would normally show up in one of your cameras for at least one second,
which is enough time to be registered and processed by Ambianic Edge on a plain RPI4.

## Configuration Questions

If you are having difficulties with configuration settings, feel free to ask your question in the [community discussion forum](https://github.com/ambianic/ambianic-edge/discussions) or our [public slack channel](https://ambianicai.slack.com/join/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ#/).
