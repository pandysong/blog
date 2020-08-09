---
date: 2018-04-24
title: ONVIF NVA
weight: 10
---

# Introduction

NVA defines following interfaces:

- Analytics config: config how the analytics could be done on the device
- Scene description (Mandatory): describe the analytic output, e.g. where the object is and
  when, either static or dynamic.
- Rule config (optional): Rules Analytics Engine needs the rule config.



# Analytics Config

A video analytics configuration can be attached to a Media Profile if the ONVIF
Media Service is present. In that case the video analytics configuration
becomes connected to a specific video source.


# Scene description

xml based description streamed as metadata to client via RTP.


```

The temporal and spatial relation of scene elements with respect to the
selected video source is discussed in sections 5.1.2.1 and 5.1.2.2. The
appearance and behaviour of tracked objects is discussed in section 5.1.3.1.
Interactions between objects like splits and merges are described in section
5.1.3.2. 

```
