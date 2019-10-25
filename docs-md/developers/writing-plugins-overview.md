
# How to Write and Publish an Ambianic Plugin

Ambianic can be extended via custom plugins.
Two types of plugins are supported:

-   Edge plugins, and
-   UI plugins

Edge plugins execute in an Ambianic Edge device environment, whereas
UI plugins execute in the Ambianic UI PWA environment.
It is common for a plugin to be built as a package with an Edge and an UI plugin
designed to work together for a smooth end-to-end user experience.

Let's take for example a `VideoSource` plugin that reads
data from video streams such as live security camera feeds and
feeds image frames to an Ambianic AI flow for inference such as person
detection.

The plugin will have two parts:

1.  An edge device plugin `VideoSourceEdge` which will reside in an
   Ambianic Edge device and execute in the core Ambianic runtime.
   It will be installed and configured on the edge device via
   secure shell access, its public REST APIs or GUI exposed by
   the `VideoSourceUI` plugin.
2.  An UI plugin `VideoSourceUI` which will implement GUI widgets for
   users to configure and control `VideoSource` connections. This plugin
   will enrich the visual user experience with the Ambianic UI PWA.

<div class="center plugin-overview-diagram">
st=>start: VideoSource Plugin
install=>parallel: Install Plugin
edge_plugin=>operation: VideoSourceEdge
edge_env=>operation: Ambianic Edge Device
src_connect=>inputoutput: Process video streams
edge_flow=>operation: AI flow
ui_plugin=>operation: VideoSourceUI
ui_env=>operation: Ambianic UI PWA
user_config=>inputoutput: GUI Configuration

st(bottom)->install
install(path1, right)->edge_plugin
edge_plugin(bottom)->edge_env
edge_env(bottom)->src_connect
src_connect(bottom)->edge_flow
install(path2, bottom)->ui_plugin
ui_plugin(bottom)->ui_env
ui_env(bottom)->user_config
</div>

<script>
$(".plugin-overview-diagram").flowchart();
</script>

<br/>

## Anatomy of an Ambianic Edge plugin

Ambianic Edge plugins are written in Python.
They are essentially pipeline elements. See the
[architecture](architecture.md) document for an overview of pipelines and
pipeline elements.

An edge plugin must provide at a minimum:

1.  Input:
    -   Declare input data types
    -   Provide implementation of input data consumer
2.  Output:
    -   Declare output data types
    -   Provide implementation of output data producer
3.  Configuration:
    -   Declare configuration parameters
4.  Lifecycle methods:
    -   Start
    -   Stop
    -   Heal

An edge plugin may also provide:

1.  REST API:
    -   Declare public REST APIs in [OpenAPI](https://www.openapis.org/) format.
    -   Provide implementation for REST APIs

## Anatomy of an Ambianic UI plugin

An UI plugin must provide at a minimum:

1.  Install/Uninstall visual UI flow
1.  Configuration visual UI flow
1.  Runtime data views:
    -   Event Timeline: inputs and outputs
    -   Alerts: Critical, Warning, Info
    -   Stats: such as performance and error rates
1.  Event curation:
    *   Prioritization:
        -     Alert
        -     Warning
        -     Info
    *   Avatar:
        -     Image
        -     Name
        -     Color
    *   Follow up Actions:
        -     Checkbox
        -     Button
        -     Link
        -     Text
        -     Speech
        -     Gesture
        -     Custom
    *   Data curation for plugins with AI inference:
        -   Infenrece label correction
        -   Adding new labels
        -   Removing labels



## Plugin bundles

A plugin bundle consists of:

1.  Edge plugin
2.  UI plugin

### A basic edge plugin template

The following abstract Python class implements the minimum sceleton of an
edge plugin. Plugin implementations should extend this class and add custom
logic as we will see further in this document with a more advanced example.

```Python

class PipeElement(ManagedService):
    """The basic building block of an Ambianic pipeline."""

    def __init__(self):
        super().__init__()
        self._state = PIPE_STATE_STOPPED
        self._next_element = None
        self._latest_heartbeat = time.monotonic()

    @property
    def state(self):
        """Lifecycle state of the pipe element."""
        return self._state

    def start(self):
        """Only sourcing elements (first in a pipeline) need to override.

        It is invoked once when the enclosing pipeline is started. It should
        continue to run until the corresponding stop() method is invoked on the
        same object from a separate pipeline lifecycle manager thread.

        It is recommended for overriding methods to invoke this base method
        via super().start() before proceeding with custom logic.

        """
        self._state = PIPE_STATE_RUNNING

    @abc.abstractmethod
    def heal(self):  # pragma: no cover
        """Override with adequate implementation of a healing procedure.

        heal() is invoked by a lifecycle manager when its determined that
        the element does not respond within reasonable timeframe.
        This can happen for example if the element depends on external IO
        resources, which become unavailable for an extended period of time.

        The healing procedure should be considered a chance to recover or find
        an alternative way to proceed.

        If heal does not reset the pipe element back to a responsive state,
        it is likely that the lifecycle manager will stop the
        element and its ecnlosing pipeline.

        """
        pass

    def healthcheck(self):
        """Check the health of this element.

        :returns: (timestamp, status) tuple with most recent heartbeat
        timestamp and health status code ('OK' normally).
        """
        status = 'OK'  # At some point status may carry richer information
        return self._latest_heartbeat, status

    def heartbeat(self):
        """Set the heartbeat timestamp to time.monotonic().

        Keeping the heartbeat timestamp current informs
        the lifecycle manager that this element is functioning
        well.

        """
        now = time.monotonic()
        self._latest_heartbeat = now

    def stop(self):
        """Receive stop signal and act accordingly.

        Subclasses implementing sourcing elements should override this method
        by first invoking their super class implementation and then running
        through steps specific to stopping their ongoing sample processing.

        """
        self._state = PIPE_STATE_STOPPED

    def connect_to_next_element(self, next_element=None):
        """Connect this element to the next element in the pipe.

        Subclasses should not override this method.

        """
        assert next_element
        assert isinstance(next_element, PipeElement)
        self._next_element = next_element

    def receive_next_sample(self, **sample):
        """Receive next sample from a connected previous element if applicable.

        All pipeline elements except for the first (sourcing) element
        in the pipeline will depend on this method to feed them with new
        samples to process.

        Subclasses should not override this method.

        :Parameters:
        ----------
        **sample : dict
            A dict of (key, value) pairs that represent the sample.
            It is left to specialized implementations of PipeElement to specify
            their in/out sample formats and enforce compatibility with
            adjacent connected pipe elements.

        """
        self.heartbeat()
        for processed_sample in self.process_sample(**sample):
            if self._next_element:
                if (processed_sample):
                    self._next_element.receive_next_sample(**processed_sample)
                else:
                    self._next_element.receive_next_sample()
                self.heartbeat()

    @abc.abstractmethod  # pragma: no cover
    def process_sample(self, **sample) -> Iterable[dict]:
        """Override and implement as generator.

        Invoked by receive_next_sample() when the previous element
        (or pipeline source) feeds another data input sample.

        Implementing subclasses should process input samples and yield
        output samples for the next element in the pipeline.

        :Parameters:
        ----------
        **sample : dict
            A dict of (key, value) pairs that represent the sample.
            It is left to specialized implementations of PipeElement to specify
            their in/out sample formats and enforce compatibility with
            adjacent connected pipe elements.

        :Returns:
        ----------
        processed_sample: Iterable[dict]
            Generates processed samples to be passed on
            to the next pipeline element.

        """
        yield sample

```

The following sequence diagram illustrates an example flow of data from
an external security camera source through `VideoSourceEdge` and the rest of
an object detection pipeline.

<div class="mermaid">
  sequenceDiagram
      participant Cam as Security Camera
      participant Source as VideoSourceEdge
      participant Detect as ObjectDetection
      participant Store as StoreDetection
      participant File as File System
      loop Until pipeline.stop()
          Cam->>Source: rtsp://ip/video/channel0
          Source->>Detect: process_sample(image)
          Detect->>Store: process_sample(det_boxes)
          Store->>File: file.write(image, det_json)
      end
</div>

&nbsp;

### A basic UI plugin template



### A basic plugin bundle template

## A more advanced plugin example

## Required components of a plugin

## Optional components of a plugin

## Writing a custom plugins: step by step

## Testing plugins

## Packaging plugins

### Packaging and Edge plugin

Use a standard python wheel and publish on Pypi

### Packaging and UI plugin

Use a standard npm package format and publish on npm

### Packaging a bundle of plugins

In the UI plugin, provide reference to the edge plugin package:

-   PyPi wheel name
-   package version

## Documenting plugins

## Publishing plugins

More about [Publishing your AI app](publish-bazaar.md)
