# Ambianic High Level Flow Diagram

![Screen Shot 2021-09-09 at 1 27 26 PM](https://user-images.githubusercontent.com/2234901/132743055-81d867b3-75e9-4433-a494-8e3a248a4f9c.png)



# Ambianic System Level Architecture

[![Ambianic-High-Level-Architecture](https://user-images.githubusercontent.com/2234901/112674881-73f7df00-8e34-11eb-9447-71a5a358bee5.png)](https://drive.google.com/file/d/13vtPDpW_wmE73YJ0lnIXF3xyPqGOYE22/view?usp=sharing)

# Ambianic User Personas

There are two major types of user personas we see in our community:
1. DYI folks and developers who love the ultimate control and flexibility of an Open Source privacy preserving surveillance solution.
2. End users who love the concept of a no nonsense privacy preserving solution to help care for their household and remote family members.

User group 1 (i.e. cohort) cares more about freedom to fork, hack and integrate the code any way they like with any other systems they want to. This group is about 2% of all people who have been involved in some capacity with Ambianic.ai.

User group 2, doesn't have the skills, time or plain interest to get messy with integrating an AI component with an existing camera system. They would rather buy a new turn-key intelligent camera up to $200. If they had an old camera installed (presumably a $50-100 investment), they would rather return it and replace with the a new one or simply donate it to a charity. If the old camera has been used more than 6 months, then its a depreciated asset that served its purpose and can be now written off. $100 is generally a tolerable pain threshold as compared to spending hours on integration or hiring an expert to do the work, which would cost much more than a new drop-in replacement.

Both user groups are important to the long term success of the project.

User Group 1 is vital, because these are the kinds of folks who can thoroughly dissect the software and offer valuable constructive criticism, contribute code, docs and in the future help grow the core team.

User Group 2, the remaining 98%, is obviously vital, because these are the majority of end users who we ultimately aim to provide value to. 

Just to be clear, Ambianic.ai is primarily focused on adding tangible value to real users today. The fact that its open source does not contradict the primary goal in any way. Open Source is meant to enforce its validity of true value by removing proprietary layers of obscurity and superficial flashy claims. 

As a high level roadmap decision, we are moving forward by acknowledging that both User Group 1 and 2 are important and the project should proceed in a way that doesn't lock out either one from participating.

To that extend, Luca Capra (one of our new team members) summarized the way forward better than I could:

> I am convinced that every niche expect and deserve a specific UX. I think the core could be flexible enough to support generic functionalities while delegating domain specific challenges to a vertical application. This could happen through different layers of integrations, eventually reusable. An example could be an object detection pipeline for training and execution, adapted to home care but also to security. Same functional perimeter but with different specializations based on user requirements

# Ambianic Edge Device

Here is a high level architecture diagram of an Ambianic Edge device.

[![Ambianic ai Camera Integration Architecture](https://user-images.githubusercontent.com/2234901/112674989-9984e880-8e34-11eb-8871-e65908f4f006.png)
)](https://drive.google.com/file/d/1taVlFu6r2jULWPpvdm1hO65WOxlNSw8R/view?usp=sharing)

And here is a composition diagram showing APIs and layers of each Ambianic Edge component.

![Screen Shot 2021-09-09 at 1 28 36 PM](https://user-images.githubusercontent.com/2234901/132742878-946677bc-990e-4eaa-af35-aeba091607c1.png)


# Ambianic Edge Components

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
