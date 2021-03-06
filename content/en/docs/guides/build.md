---
title: "Build an edge AI app in about 30 minutes"
linkTitle: "Build an edge AI app"
weight: 200
---

![face-detection-app](/images/guide-ai-app-hero.jpg)

## What you will accomplish

In this step-by-step guide you’ll learn how to build your first Darcy AI app. This [app](../more/terminology.md#application) utilizes all of the core Darcy AI concepts, such as [pipelines](../more/terminology.md#pipeline), [input streams](../more/terminology.md#input-stream), [perceptors](../more/terminology.md#perceptor) and [output streams](../more/terminology.md#output-stream) to detect and count people and then change some basic configurations. The concepts in this guide are the base foundations for any Darcy AI application you might want to build in the future.

{{< alert >}}
Darcy AI can run in CPU mode (if you don't have a Coral accelerator attached), but requires a relatively modern system with a good CPU (ideally with multiple cores), and a decent amount of memory. If you run it on a low power platform, then you will get a very low frames per second video output.
{{< /alert >}}

### Requirements

- macOS (Linux & Windows coming soon)
- 5GB or more of free disk space
- Video source (like a built-in web cam or attached USB cam)
- Basic command line knowledge
- Basic Python knowledge

---

## Set up your build environment

![face-detection-app](/images/guides/environment@2x.jpg)

### Install dependencies

{{< rawhtml >}}

<ul class="nav nav-pills darcy-tabs" id="osTabs" role="tablist">
  <li class="nav-item" role="presentation">
    <button class="nav-link active" id="macosx-tab" data-bs-toggle="tab" data-bs-target="#macosx" type="button" role="tab" aria-controls="macosx" aria-selected="true">macOS</button>
  </li>
  <li class="nav-item" role="presentation">
    <button class="nav-link" id="linux-tab" data-bs-toggle="tab" data-bs-target="#linux" type="button" role="tab" aria-controls="linux" aria-selected="false">Linux</button>
  </li>
  <li class="nav-item" role="presentation">
    <button class="nav-link" id="windows-tab" data-bs-toggle="tab" data-bs-target="#windows" type="button" role="tab" aria-controls="windows" aria-selected="false">Windows</button>
  </li>
</ul>
<div class="tab-content darcy-tab-content">
    <div class="tab-pane active" id="macosx" role="tabpanel" aria-labelledby="macosx-tab">
{{</ rawhtml>}}

- Install Python 3.6.9 or greater by [downloading](https://www.python.org/downloads/) or installing via your favorite package manager, e.g.

```bash
brew install python
```

{{< rawhtml >}}

<div class="alert-container">
{{< /rawhtml >}}
{{< alert icon="" >}}
If you have older Python versions on your computer, you may need to use `python3` and `pip3` commands.
You can also change this behavior for [macOS](https://osxdaily.com/2022/02/15/make-python-3-default-macos/)
or [Windows (coming soon)](https://stackoverflow.com/questions/5087831/how-should-i-set-default-python-version-in-windows)
{{< /alert >}}
{{< rawhtml >}}
</div>
{{< /rawhtml >}}

**X86 Platforms**:

```bash
# Install OpenCV
pip install opencv-python>=4.5.5.64

# Install the Pillow library
pip install Pillow>=8.3.2

# Install the Numpy library
pip install numpy>=1.22.4

# Install the Imutils library
pip install imutils>=0.5.4

# Install the DarcyAI Engine
pip install darcyai

# Install TensorFlow
pip install tensorflow
```

**Apple M1 Platforms**:

{{< rawhtml >}}

<div class="alert-container">
{{< /rawhtml >}}
{{< alert style="warning" >}}
If you are on Apple M1 silicon, these instructions will not work for you. M1 Mac solution coming soon.
{{< /alert >}}
{{< rawhtml >}}
</div>
{{< /rawhtml >}}

{{< rawhtml >}}

</div>
<div class="tab-pane" id="linux" role="tabpanel" aria-labelledby="linux-tab">
{{</ rawhtml>}}

Linux coming soon...

{{< rawhtml >}}

</div>
<div class="tab-pane" id="windows" role="tabpanel" aria-labelledby="windows-tab">
{{</ rawhtml>}}

Windows coming soon...

{{< rawhtml >}}

</div>

</div>
{{< /rawhtml >}}

## Build your Darcy AI app

### Create your application Python file and import libraries

You only need a single Python file to build a Darcy AI application. Open a new .py file in your
favorite IDE and name it whatever you want. Then add the following statements at the top to include
the Darcy AI libraries and some additional helpful libraries:

```python
import cv2
import os
import pathlib

from darcyai.perceptor.people_perceptor import PeoplePerceptor
from darcyai.input.camera_stream import CameraStream
from darcyai.output.live_feed_stream import LiveFeedStream
from darcyai.pipeline import Pipeline
from darcyai.config import RGB
```

### Add the Pipeline, Input Stream, and Output Stream objects

```python
# Instantiate an Camera Stream input stream object
camera = CameraStream(video_device=0, fps=20)

# Instantiate the Pipeline object and pass it the Camera Stream object as its input stream source
pipeline = Pipeline(input_stream=camera)

# Create a Live Feed output stream object and specify some URL parameters
live_feed = LiveFeedStream(path="/", port=3456, host="0.0.0.0")
```

### Set up a callback and add the Output Stream to the Pipeline

Before we add the LiveFeed Output Stream to the Pipeline, we need to set up a [callback](../more/terminology.md#callback) function that
we are going to use to process the data before displaying the video. This is where all of the business logic is taking place. After the callback function definition, there is a line for adding the LiveFeed Output Stream to the Pipeline. That command needs to have the callback
function already defined before it can execute successfully.

```python
# Create a callback function for handling the Live Feed output stream data before it gets presented
def live_feed_callback(pom, input_data):
    # Start wth the annotated video frame available from the People Perceptor
    frame = pom.peeps.annotatedFrame().copy()

    # Add some text telling how many people are in the scene
    label = "{} peeps".format(pom.peeps.peopleCount())
    color = (0, 255, 0)
    cv2.putText(frame, str(label), (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, color, 1, cv2.LINE_AA)

    # If we have anyone, demonstrate looking up that person in the POM by getting their face size
    # And then put it on the frame as some text
    # NOTE: this will just take the face size from the last person in the array
    if pom.peeps.peopleCount() > 0:
        for person_id in pom.peeps.people():
            face_size = pom.peeps.faceSize(person_id)
            face_height = face_size[1]
            label2 = "{} face height".format(face_height)
            color = (0, 255, 255)
            cv2.putText(frame, str(label2), (10, 80), cv2.FONT_HERSHEY_SIMPLEX, 1, color, 1, cv2.LINE_AA)

    # Pass the finished frame out of this callback so the Live Feed output stream can display it
    return frame

# Add the Live Feed output stream to the Pipeline and use the callback from above as the handler
pipeline.add_output_stream("output", live_feed_callback, live_feed)
```

### Define event Output Stream, input the Output Stream and instantiate the People Perceptor

Just like the LiveFeed Output Stream, the People [Perceptor](/docs/more/terminology#perceptor)
must have the callback already defined before it can work with those callbacks. The input callback
simply takes the [input stream](/docs/more/terminology#input-stream) data and sends it onward to
the People [Perceptor](/docs/more/terminology#perceptor). The “New Person” event callback simply
prints the unique person identifier string to the console output when a new person has been detected
by Darcy AI.

```python
# Create a callback function for handling the input that is about to pass to the People Perceptor
def people_input_callback(input_data, pom, config):
    # Just take the frame from the incoming input stream and send it onward - no need to modify the frame
    frame = input_data.data.copy()
    return frame

# Create a callback function for handling the "New Person" event from the People Perceptor
# Just print the person ID to the console
def new_person_callback(person_id):
    print("New person: {}".format(person_id))

# Instantiate a People Perceptor
people_ai = PeoplePerceptor()

# Subscribe to the "New Person" event from the People Perceptor and use our callback from above as the handler
people_ai.on("new_person_entered_scene", new_person_callback)
```

### Add the People Perceptor to the Pipeline

```python
# Add the People Perceptor instance to the Pipeline and use the input callback from above as the input preparation handler
pipeline.add_perceptor("peeps", people_ai, input_callback=people_input_callback)
```

### Change some configuration items in the People Perceptor

```python
# Update the configuration of the People Perceptor to show the pose landmark dots on the annotated video frame
pipeline.set_perceptor_config("peeps", "show_pose_landmark_dots", True)
pipeline.set_perceptor_config("peeps", "pose_landmark_dot_size", 2)
pipeline.set_perceptor_config("peeps", "pose_landmark_dot_color", RGB(0, 255, 0))
```

### Start the Pipeline

```python
# Start the Pipeline
pipeline.run()
```

### Check your completed code

Your finished Python file should look similar to this. Save your Python file. Next we will run your code!

```python
import cv2
import os
import pathlib

from darcyai.perceptor.people_perceptor import PeoplePerceptor
from darcyai.input.camera_stream import CameraStream
from darcyai.output.live_feed_stream import LiveFeedStream
from darcyai.pipeline import Pipeline
from darcyai.config import RGB

# Instantiate an Camera Stream input stream object
camera = CameraStream(video_device=0, fps=20)

# Instantiate the Pipeline object and pass it the Camera Stream object as its input stream source
pipeline = Pipeline(input_stream=camera)

# Create a Live Feed output stream object and specify some URL parameters
live_feed = LiveFeedStream(path="/", port=3456, host="0.0.0.0")

# Create a callback function for handling the Live Feed output stream data before it gets presented
def live_feed_callback(pom, input_data):
    # Start wth the annotated video frame available from the People Perceptor
    frame = pom.peeps.annotatedFrame().copy()

    # Add some text telling how many people are in the scene
    label = "{} peeps".format(pom.peeps.peopleCount())
    color = (0, 255, 0)
    cv2.putText(frame, str(label), (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, color, 1, cv2.LINE_AA)

    # If we have anyone, demonstrate looking up that person in the POM by getting their face size
    # And then put it on the frame as some text
    # NOTE: this will just take the face size from the last person in the array
    if pom.peeps.peopleCount() > 0:
        for person_id in pom.peeps.people():
            face_size = pom.peeps.faceSize(person_id)
            face_height = face_size[1]
            label2 = "{} face height".format(face_height)
            color = (0, 255, 255)
            cv2.putText(frame, str(label2), (10, 80), cv2.FONT_HERSHEY_SIMPLEX, 1, color, 1, cv2.LINE_AA)

    # Pass the finished frame out of this callback so the Live Feed output stream can display it
    return frame

# Add the Live Feed output stream to the Pipeline and use the callback from above as the handler
pipeline.add_output_stream("output", live_feed_callback, live_feed)

# Create a callback function for handling the input that is about to pass to the People Perceptor
def people_input_callback(input_data, pom, config):
    # Just take the frame from the incoming input stream and send it onward - no need to modify the frame
    frame = input_data.data.copy()
    return frame

# Create a callback function for handling the "New Person" event from the People Perceptor
# Just print the person ID to the console
def new_person_callback(person_id):
    print("New person: {}".format(person_id))

# Instantiate a People Perceptor
people_ai = PeoplePerceptor()

# Subscribe to the "New Person" event from the People Perceptor and use our callback from above as the handler
people_ai.on("new_person_entered_scene", new_person_callback)

# Add the People Perceptor instance to the Pipeline and use the input callback from above as the input preparation handler
pipeline.add_perceptor("peeps", people_ai, input_callback=people_input_callback)

# Update the configuration of the People Perceptor to show the pose landmark dots on the annotated video frame
pipeline.set_perceptor_config("peeps", "show_pose_landmark_dots", True)
pipeline.set_perceptor_config("peeps", "pose_landmark_dot_size", 2)
pipeline.set_perceptor_config("peeps", "pose_landmark_dot_color", RGB(0, 255, 0))

# Start the Pipeline
pipeline.run()
```

## Run your app

Using your IDE, run your Python code. Don't set any breakpoints at first because that will prevent
you from seeing the video stream. If you get an error initializing the camera, ensure that your IDE
has permissions to access the camera on your device. If you followed the code reference above
directly and you have all the required Python libraries installed, your Darcy AI application should
run successfully and stay running until you stop the program execution.

{{< alert icon="" >}}
Best performance is achieved with a modern, multi-core CPU and plenty of memory. In the next guide, you'll deploy to an edge device (like a Raspberry Pi + AI accelerator) which should provide great performance.
{{< /alert >}}

### View your real-time Darcy AI application video output

Once your application is running, you can view the live video feed by visiting `http://localhost:3456/` in
any browser. The port number 3456 has been specified in the Python code. Feel free to change that port
if desired.

### What you should see

![face-detection-app](/images/guide-ai-app-hero.jpg)

You should see a live video feed coming from your camera. When a person is detected in the field of
view, some information should be displayed on the video and some dots should be drawn on top of key
face locations. The dots should move with the person's face. This is a demonstration of using Darcy
AI to detect the presence of people, assign an anonymous stable identifier to persons as they move
around the field of view, and annotate the video frames with text and graphics.

Next, you'll [deploy the app to the edge](/docs/guides/deploy).
