
# Quick Start

Ambianic's main goal is to provide helpful suggestions in the context of home
and business automation. The main unit of work is the Ambianic pipeline.
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

## Ambianic Deployment

Ambianic has two major deployment components: Ambianic Edge and Ambianic UI.

### Ambanic Edge Deployment
The easiest way to deploy Ambianic Edge is via its
[Docker distribution](https://hub.docker.com/r/ambianic/ambianic-edge).

The recommended deployment platform for Ambianic Edge is
[Raspberry Pi 4 with 4GB RAM and 32GB SD card](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/).
The docker image is availbale for ARM and x86 architectures so it can be deployed
on most modern machines and operating systems.

To deploy on a Raspberry Pi 4, you will need a recent
[Raspbian install](https://www.raspberrypi.org/documentation/setup/) with
[Docker on it](https://www.freecodecamp.org/news/the-easy-way-to-set-up-docker-on-a-raspberry-pi-7d24ced073ef/).

You can install and run the image in the default pi user space on Raspbian.

#### Configuration

Before starting the image, you will need to designate a workspace directory with an initial `config.yaml` file in it.
For example, the directory could be named `/opt/ambianid-edge.workspace`.

Here is an example `config.yaml` file

```YAML
######################################
#  Ambianic main configuration file  #
######################################
version: '1.0.10'

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

Here is the definition of this variable reference that you will find in config.yaml:

Since camera URIs often contain credentials, we recommended that you store these values
in `secrets.yaml` which needs to be located in the same directory as
`config.yaml`. Ambianid Edge will automatically look for and if available
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

#### Starting Ambianic Edge

Here is an example startup line for Ambianic Edge. We will go over each parameter below:

```sh
# test if coral usb stick is available
USB_DIR=/dev/bus/usb


if [ -d "$USB_DIR" ]; then
  USB_ARG="--device $USB_DIR"
else
  USB_ARG=""
fi

# check if there is an image update
docker pull ambianic/ambianic-edge:latest
# run latest image
docker run -it --rm \
  --name ambianic-edge \
  --mount type=bind,source="/opt/workspace",target=/workspace \
  --publish 8778:8778 \
  $USB_ARG \
  ambianic/ambianic:latest
```

Let's review the start script above line by line:
```sh
# test if coral usb stick is available
USB_DIR=/dev/bus/usb


if [ -d "$USB_DIR" ]; then
  USB_ARG="--device $USB_DIR"
else
  USB_ARG=""
fi

```

Ambianic Edge is able to automatically detect and use the
[Google Coral EdgeTPU](https://coral.withgoogle.com/) for AI inference.
Coral is a powerful USB stick that can speed up inference 5-10 times. However AI inference is only part of
all the functions that execute in an Ambianic Edge pipeline. Video decoding and formatting
also takes substantial amount of processing time. Overall we've seen Coral to improve
performance 30-50% on a realistic Raspberry Pi 4 deployment with multiple simultaneous pipelines
pulling images from multiple cameras and processing these images with multiple inference models.

If you don't have a Coral device available, no need to worry for now. Raspberry Pi 4 is powerful enough
to handle multiple simultaneous camera sourced pipelines with approximately 1-2 frames per second (fps).
An objects or person of interest would normally show up in one of your cameras for at leastd one second,
which is enough time to be registered and processed by Ambianic Edge on a plain RPI4.

Let's look at the other docker image start parameters:

```sh
docker pull ambianic/ambianic-edge:latest
```

This line ensures that we fetch the latest available Ambianic Edge image. There are weekly semanctic releases
with fixes and new updates. Generally this should work for most deployments. Occasionally there may be
new releases with breaking changes. Its important to keep an eye on the release notes and make any configuration
adjustments if necessary.

```sh
--name ambianic-edge \
```

This parameter just gives a local friendly name of the container that will show up in a docker container listing instead of a cryptic hash tag.

```sh
--mount type=bind,source="/opt/workspace",target=/workspace \
```

This parameter tells Docker to bind your prepared workspace with `config.yaml` to the image internal `/workspace` directory. In addition to finding `config.yaml`, the image will use the workspace directory to write log and data files such as images with object detections.

```sh
$USB_ARG \
```

This is the dynamic parameter which is empty if Coral is not found on the USB bus. Otherwise it tells Docker
to let Ambianic Edge access Coral on the usb bus.

```sh
  --publish 8778:8778 \
```

This parameter exposes port 8778 where the Ambianic Edge REST API is available.

```sh
ambianic/ambianic:latest
```
The final parameter points to the latest Ambianic Edge image that Docker will execute.

If you use Docker Compose for a more convenient management of multiple docker images, here is a configuration section
that you can place in your `docker-compose.yaml`

```YAML
ambianic:
    container_name: ambianic
    restart: unless-stopped
    privileged: true
    image: ambianic/ambianic-edge:latest
    volumes:
      # - /dev/bus/usb:/dev/bus/usb
      - /opt/ambianic-edge.prod:/workspace
    ports:
      - 8778:8778
    restart: on-failure
    healthcheck:
      test: ["CMD", "curl", "-sI", "http://127.0.0.1:8778/"]
      interval: 300s
      timeout: 1s
      retries: 10
```

Notice the line with the usb parameter. It would be commented out if there is no Coral attached to your RPI.

### Ambanic UI Deployment

[Ambianic UI](https://github.com/ambianic/ambianic-ui) is a prorgressive web application (PWA) that provides you with a friendly access to Ambianic Edge device data. For example you can see a timeline view with
notable events around your home organized chronologically or by importance. Below is an example
timeline screenshot.

![Timeline](../assets/images/timeline-screen.png)

#### Public Ambanic UI app

You can access the public app at [https://ui.ambianic.ai/](https://ui.ambianic.ai/)

```Notice
Due to PWA security requirements it is currently not possible to connect directly from the public app to your local Ambianic Edge device.
```

One solution is to setup a secure connection to your Ambinic Edge device via Dynamic DNS and Open SSH tunnel. However this is an
[involved process](https://tinkerman.cat/post/secure-remote-access-to-your-iot-devices/) that could take a few hours of your time.

There is an alternative solution via WebRTC that only takes a few moments to setup. We are working on it for the next Ambianic UI release.

In the meanwhile, you can install Ambianic UI on a local machine - either the same one where Ambianic Edge runs or any other machine that runs on the same LAN. Here is how:

#### Local deployment of Ambianic UI

Ambianic UI is distributed as a Node JS npm package.
https://www.npmjs.com/package/ambianic-ui

If you are familiar with Node JS, you can install and run ambianic-ui from its npm distribution.

Otherwise, you can follow these steps:

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

The home screen looks like this:

![Home screen](../assets/images/home-screen.png)

Go to the Settings page to point the UI to the host name of your Ambianic Edge device. You can test the connection to be sure that all is fine. Then navigate to the Timeline page to see detections from your cameras.

![Home screen](../assets/images/settings-screen.png)

If you experience problems with your initial setup, feel free to [open an issue](https://github.com/ambianic) on github.
