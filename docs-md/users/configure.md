
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
```
  front_door_camera: &src_front_door_cam
    uri: *secret_uri_front_door_camera
```
 
 Replace `*secret_uri_front_door_camera` with your camera RTSP URI. For example:

```
  front_door_camera: &src_front_door_cam
    uri: rtsp://admin:password@192.168.1.99/media/video1 
    live: true    
```

Make sure that this exact camera RTSP URI produces a video feed. You can [test that](https://www.unifore.net/ip-video-surveillance/how-to-play-rtsp-video-stream-of-ip-cameras-on-vlc-player-quicktime-player.html) with a tool like [VLC](https://www.videolan.org/).

The parameter `live: true` indicates that this is a continuous stream without a predetermined end. Therefore Ambianic Edge should do whatever it can to continuously pull media and recover from interruptions caused by network or other glitches.

Now save the file and restart the docker image. Within a few moments you should be able to see a populated timeline in the UI with fresh images from your camera and some object, people and face detections if there are any to detect.

You can reference the [Quick Start Guide](quickstart.md) for instructions on starting and stopping the Ambianic Edge docker image.

### How to find the RTSP URI for your camera

If you don't have experience configuring surveillance cameras, it can be tricky to find the RTSP URI. We are working on a camera Plug-and-Play feature in Ambianic to make it easier to discover and connect to your cameras. Keep an eye for release news. 

In the meanwhile, you have several options:

First, check your camera manufacturer's documentation whether it support RTSP. Most IP cameras do, but not all.

You can use a tool such as ONVIF Device Manager to auto discover your IP camera and show you its RTSP URI. Here is a [quick How-To](http://help.angelcam.com/en/articles/372646-how-to-find-a-rtsp-address-for-an-onvif-compatible-camera-nvr-dvr). A more detailed post on this tool is also [available here](https://learncctv.com/onvif-device-manager/).

Your camera manufacturer will likely have an online resource describing how to determine its RTSP address. It would look something like [this one](https://reolink.com/wp-content/uploads/2017/01/Reolink-CGI-command-v1.61.pdf).

There is also an [online directory](https://security.world/rtsp/) where you can search for the RTSP URI of many camera brands.

### Using still image source instead of RTSP stream for your camera URI

In many cases processing 1 frame per second is sufficient frequency to detect interesting events in your environment. It is usually more CPU and network resource efficient to use a still image source instead of live RTSP stream for 1fps.

All you have to do to use a still image source from your camera is to locate the specific image URI using techniques similar to the ones you use to find the RTSP URI. Then replace the value in the corresponding `source` section of the `config.yaml` file. Here is what it would look like:

```
  front_door_camera: &src_front_door_cam
    uri: http://192.168.86.29/cgi-bin/api.cgi?cmd=Snap&channel=0&rs=wuuPhkmUCeI9WG7C&user=admin&password=******
    live: true
```

In the example above the URI points to a still jpg sample from the camera. Ambianic Edge will continuously poll this URI approximately once per second (1fps).

### Storing sensitive config information in secrets.yaml

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

Let's assume that you have a USB attached Coral running on `/dev/bus/usb`.
In order for Ambianic Edge to see it, add the following parameter
to the docker image start line:
`--device /dev/bus/usb`

Coral is a powerful TPU that can speed up inference 5-10 times. However AI inference is only part of
all the functions that execute in an Ambianic Edge pipeline. Video decoding and formatting
also takes substantial amount of processing time. Overall we've seen Coral to improve
performance 30-50% on a realistic Raspberry Pi 4 deployment with multiple simultaneous pipelines
pulling images from multiple cameras and processing these images with multiple inference models.

If you don't have a Coral device available, no need to worry for now. Raspberry Pi 4 is quite powerful.
It comfortably handles 3 simultaneous HD camera sourced pipelines with approximately 1-2 frames per second (fps).
An objects or person of interest would normally show up in one of your cameras for at least one second,
which is enough time to be registered and processed by Ambianic Edge on a plain RPI4.

## Using Docker Compose

If you use Docker Compose for a more convenient management of multiple docker images,
here is a configuration section
that you can place in your `docker-compose.yaml`

```YAML
version: "3.7"
services:
  ambianic-edge:
    container_name: ambianic-edge
    restart: unless-stopped
    privileged: true
    image: ambianic/ambianic-edge:latest
    # command: /workspace/ambianic-run2.sh
    network_mode: "host"
    volumes:
    #  - /dev/bus/usb:/dev/bus/usb
      - /opt/ambianic-edge.prod:/workspace
    # ports:
    #  - 8778:8778
    restart: on-failure
    healthcheck:
      test: ["CMD", "curl", "-sI", "http://127.0.0.1:8778/"]
      interval: 300s
      timeout: 3s
      retries: 10
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "10"      
      
```

Notice the line with the usb parameter. Uncomment it if there is a Google Coral TPU Accelerator attached to your RPI.

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
