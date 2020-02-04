
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

You can use as a starting point this
[template](https://github.com/ambianic/ambianic-edge/blob/master/config.yaml)
and modify to fit your needs.

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

In the configuration excerpt above, there are a few references to variables
defined elsewhere in the YAML file. `*src_front_door_cam` and
`*src_entry_area_cam` are the only
references that you have to understand and configure in order
to get Ambianic Edge working with your own cameras (or other source of video feed).

Since camera URIs often contain credentials, we recommended that you store these values
in `secrets.yaml` which needs to be located in the same directory as
`config.yaml`. Ambianic Edge will automatically look for and if available
prepend `secrets.yaml` to `config.yaml`. The idea here is that you can share
 `config.yaml` with others without compromising privacy sensitive parameters.

An example valid entry in `secretes.yaml` for a camera URI, would look like this:
```yaml
secret_uri_front_door_camera: &secret_uri_front_door_camera 'rtsp://user:pass@192.168.86.111:554/Streaming/Channels/101'
# add more secret entries as regular yaml mappings
```

The rest of the configuration settings can be left with their default values for now.

Notice that each pipeline definition starts with a media source. You can add and remove
pipelines to match your home setup.

Once you specify the URI of your camera(s), you can start the Docker image.
You can reference the [Quick Start Guide](quickstart.md) for information on starting and
stopping the Ambianic Edge docker image.

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
services:
  ambianic-edge:
    container_name: ambianic-edge
    restart: unless-stopped
    privileged: true
    image: ambianic/ambianic-edge:latest
    network_mode: "host"
    volumes:
      # - /dev/bus/usb:/dev/bus/usb
      - /opt/ambianic-edge.prod:/workspace
    restart: on-failure
    healthcheck:
      test: ["CMD", "curl", "-sI", "http://127.0.0.1:8778/"]
      interval: 300s
      timeout: 1s
      retries: 10
```

Notice the line with the usb parameter. It would be uncommented if there is a Coral attached to your RPI.

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
