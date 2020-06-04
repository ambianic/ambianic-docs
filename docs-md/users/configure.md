
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

You can [use this starting config.yaml template](https://gist.github.com/ivelin/1d1c885a25ad45bf8a3262653944b82c)
and modify it to fit your environment. Read further about the parameters and format of the config file.

Let's take an example `config.yaml` file and walk through it:

```YAML
######################################
#  Ambianic main configuration file  #
######################################
version: '1.2.4'

# path to the data directory
data_dir: &data_dir ./data

# Set logging level to one of DEBUG, INFO, WARNING, ERROR
logging:
  file: ./data/ambianic-log.txt
  level: DEBUG

# Pipeline event timeline configuration
timeline:
  event_log: ./data/timeline-event-log.yaml

# Cameras and other input data sources
# Using Home Assistant conventions to ease upcoming integration
sources:
  front_door_camera: &src_front_door_cam
    uri: *secret_uri_front_door_camera
    # type: video, audio, or auto
    type: video
    live: true
  entry_area_camera: &src_entry_area_cam
    uri: *secret_uri_entry_area_camera
    type: video
    # live: is this a live source or a recording
    # when live is True, the AVSource element will try to reconnect
    # if the stream is interrupted due to network disruption or another reason.
    live: true
  # recorded front door cam feed for quick testing
  # recorded_cam_feed: &src_recorded_cam_feed
  #  uri: file:///workspace/tests/pipeline/avsource/test2-cam-person1.mkv
  #  type: video


  ai_models:
  #  image_classification:
  #    tf_graph:
  #    labels:
    image_detection: &tfm_image_detection
      model:
        tflite: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_coco_quant_postprocess.tflite
        edgetpu: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite
      labels: /opt/ambianic-edge/ai_models/coco_labels.txt
    face_detection: &tfm_face_detection
      model:
        tflite: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_coco_quant_postprocess.tflite
        edgetpu: /opt/ambianic-edge/ai_models/mobilenet_ssd_v2_face_quant_postprocess_edgetpu.tflite
      labels: /opt/ambianic-edge/ai_models/coco_labels.txt
      top_k: 2

  # A named pipeline defines an ordered sequence of operations
  # such as reading from a data source, AI model inference, saving samples and others.
  pipelines:
    # sequence of piped operations for use in daytime front door watch
    front_door_watch:
      - source: *src_front_door_cam
      - detect_objects: # run ai inference on the input data
         <<: *tfm_image_detection
         confidence_threshold: 0.8
      - save_detections: # save samples from the inference results
         positive_interval: 2 # how often (in seconds) to save samples with ANY results above the confidence threshold
         idle_interval: 6000 # how often (in seconds) to save samples with NO results above the confidence threshold
      - detect_faces: # run ai inference on the samples from the previous element output
         <<: *tfm_face_detection
         confidence_threshold: 0.8
      - save_detections: # save samples from the inference results
         positive_interval: 2
         idle_interval: 600

    # sequence of piped operations for use in daytime front door watch
    entry_area_watch:
      - source: *src_entry_area_cam
      # - mask: svg # apply a mask to the input data
      - detect_objects: # run ai inference on the input data
          <<: *tfm_image_detection
          confidence_threshold: 0.8
      - save_detections: # save samples from the inference results
          positive_interval: 2 # how often (in seconds) to save samples with ANY results above the confidence thresho$
          idle_interval: 6000 # how often (in seconds) to save samples with NO results above the confidence threshold
      - detect_faces: # run ai inference on the samples from the previous element output
          <<: *tfm_face_detection
          confidence_threshold: 0.8
      - save_detections: # save samples from the inference results
          positive_interval: 2
          idle_interval: 600

```

The only parameter you have to change in order to see a populated timeline in the UI is the source uri. In the section below:
```yaml
  front_door_camera: &src_front_door_cam
    uri: *secret_uri_front_door_camera
```
 
 Replace `*secret_uri_front_door_camera` with your camera still image snapshot URI (recommended). For example:
 
```yaml
  front_door_camera: &src_front_door_cam
    uri: http://192.168.86.29/cgi-bin/api.cgi?cmd=Snap&channel=0&rs=wuuPhkmUCeI9WG7C&user=admin&password=******
    type: image
    live: true
```

Or you can alternatively plug-in the camera video streaming RTSP URI. For example:

```yaml
  front_door_camera: &src_front_door_cam
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

## Cameras

Cameras are some of the most common sources of input data for Ambianic.ai pipelines.

Ambianic.ai is typically connected to IP Network cameras, but you can also connect a local embedded web cam or USB connected camera.

### Connecting a local Web USB camera

Ambianic.ai needs a valid URI to connect to. This requires running an app that captures video from your local camera and streams it over HTTP or RTSP. VLC is one of the most popular and easy to use apps for that. 
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

```
  front_door_camera: &src_front_door_cam
    uri: http://192.168.86.29/cgi-bin/api.cgi?cmd=Snap&channel=0&rs=wuuPhkmUCeI9WG7C&user=admin&password=******
    type: image
    live: true
```

In the example above the URI points to a still jpg sample from the camera. Ambianic Edge will continuously poll this URI approximately once per second (1fps).

## Sensitive config information

In the configuration example, there are a few references to variables
defined elsewhere in the YAML file. You can use standard [YAML anchors and aliases](https://yaml.org/refcard.html) for that.

Notice that `*src_front_door_cam` and
`*src_entry_area_cam` are the YAML aliases(references) that do not appear elsewhere in config.yaml. 
They are actually picked up from an optional file located in the same directory: `secrets.yaml`.

Since camera URIs often contain credentials, we recommend that you store such values
in `secrets.yaml`. Ambianic Edge will automatically look for and if available
prepend `secrets.yaml` to `config.yaml`. That way you can share
 `config.yaml` with others without compromising privacy sensitive parameters.

An example valid entry in `secretes.yaml` for a camera URI, would look like this:
```yaml
secret_uri_front_door_camera: &secret_uri_front_door_camera 'rtsp://user:pass@192.168.86.111:554/Streaming/Channels/101'
# add more secret entries as regular yaml mappings
```

### Other Ambianic Edge configuration settings

The rest of the configuration settings are for developers and contributors. If you feel ready to dive into log files and code, you can experiement with different AI models and pipeline elements. Otherwise leave them as they are.

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

## Local Deployment of Ambanic UI

Ambianic UI is available for FREE at [ui.ambianic.ai](https://ai.ambianic.ui).
Its designed to be easy to use with Plug-and-play connectivity to Ambianic Edge.
Secured with end-to-end encryption. Your data stays only on your own devices.

However if you would like to deploy Ambianic UI on your own machine, its a
straightforward process:

Ambianic UI is [hosted](https://github.com/ambianic/ambianic-ui) on github.
It is also [distributed](https://www.npmjs.com/package/ambianic-ui) as a Node JS npm package.

If you are familiar with Node JS, you can install and run ambianic-ui from its npm distribution.

Otherwise, to build from source you can follow these steps:

##### Clone source repository

```
git clone https://github.com/ambianic/ambianic-ui.git
cd ambianic-ui
```

##### Install dependencies
```
npm install
```

##### Compiles and hot-reloads for development
```
npm run serve
```
After a few moments you should see a message in your terminal window similar to this:

```sh
App running at:
- Local:   http://localhost:8080/
- Network: http://192.168.86.246:8080/
```
Now you can access the app from your browser.
