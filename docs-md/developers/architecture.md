
# Ambianic High Level Architecture

1.  Pipelines
2.  Pipe Elements
3.  Sources
4.  Outputs
5.  Connecting Pipelines
6.  Integrations

Ambianic's main goal is to observe the environment and provide helpful
suggestions in the context of home and business automation. The user has
complete ownership and control of Ambianic's input sensors, AI inference flows
and processed data storage. All logic executes on user's own edge devices and
data is stored only on these devices with optional backup on
 a user designated server.

The main unit of work is the Ambianic pipeline which executes in the runtime of
Ambianic Edge devices.
The following diagram illustrates an example pipeline which takes as input
a video stream URI source such as a surveillance camera and
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

## Configuration

Here is the corresponding configuration section for the pipeline above
(normally located in `config.yaml`):

```yaml
pipelines:
  daytime_front_door_watch:
    - source: rtsp://user:pass@192.168.86.111:554/Streaming/Channels/101
    - detect_objects: # run ai inference on the input data
        model:
          tflite: ai_models/mobilenet_ssd_v2_coco_quant_postprocess.tflite
          edgetpu: ai_models/mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite
        labels: ai_models/coco_labels.txt
        confidence_threshold: 0.8
    - save_detections: # save samples from the inference results
        output_directory: ./data/detections/front-door/objects
        positive_interval: 2 # how often (in seconds) to save samples with ANY results above the confidence threshold
        idle_interval: 6000 # how often (in seconds) to save samples with NO results above the confidence threshold
    - detect_faces: # run ai inference on the samples from the previous element output
        model:
          tflite: ai_models/mobilenet_ssd_v2_coco_quant_postprocess.tflite
          edgetpu: ai_models/mobilenet_ssd_v2_face_quant_postprocess_edgetpu.tflite
        labels: ai_models/coco_labels.txt
        confidence_threshold: 0.8
    - save_detections: # save samples from the inference results
        output_directory: ./data/detections/front-door/faces
        positive_interval: 2
        idle_interval: 600
```

## Architectural Decomposition

The [following slides](https://docs.google.com/presentation/d/1VBDBpVR1ujHoi1g6NeFYiy7r9xflfYXNJ0jg6izEX7g/edit?usp=sharing) provide an alternative perspective on the Ambianic Edge architecture.
