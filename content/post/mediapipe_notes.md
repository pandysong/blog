---
date: 2021-04-19
title: Google MediaPipe
weight: 10
---

# MediaPipe 

MediaPipe streamlized the processing and make it flexible to handle the complex
scenario where it needs mutiple stage processing.

It support out of box GPU/CPU on desktop. It is also possible for media pipe to
run on NPU:


Refer to following disucssion:

https://github.com/google/mediapipe/issues/1425

# PalmDetection Graph

.pbtxt is the a text format descriing the graph of a data flow to performe a
specific detection, e.g.:

```
mediapipe/modules/palm_detection/palm_detection_cpu.pbtxt
```

For example the actual calculating is done by a calculator
"ImageToTensorCalculator".

```
node {
  calculator: "ImageToTensorCalculator"
  input_stream: "IMAGE:image"
  output_stream: "TENSORS:input_tensor"
  output_stream: "LETTERBOX_PADDING:letterbox_padding"
```

The input stream type is "IMAGE:image"

So that means the image is firstly converted to tensors, and then output to
"TENSORS:input_tensor", on which calculator "InferenceCalculator" is run.


```
node {
  calculator: "InferenceCalculator"
  input_stream: "TENSORS:input_tensor"
  output_stream: "TENSORS:detection_tensors"
  input_side_packet: "CUSTOM_OP_RESOLVER:opresolver"
  input_side_packet: "MODEL:model"
  options: {
    [mediapipe.InferenceCalculatorOptions.ext] {
      delegate { xnnpack {} }
    }
  }
}
```

It is tedious to understand from text format, fortunately mediapipe provides a
graph virtualizer:

```
https://google.github.io/mediapipe/tools/visualizer.html
```
