---
date: 2018-04-24
title: Onvif Device Type
weight: 10
---

# Introduction

Following are all copied from https://www.onvif.org/

# NVT (Network Video Transmitter) 


```

A Network Video Transmitter (NVT) is an ONVIF device that sends media data over
an IP network to a client. For example, an NVT may be an IP network camera or
an encoder device. 

```


An NVT implements the following services to provide its core functionality: 

- *Device service* enables an NVT to provide device management functionality
  such as device capabilities, system and network settings, security settings
and firmware upgrade. <xref to service specification>
- *Event service* enables an NVT to send events to clients. <xref to service
  specification> Media service enables an NVT to stream media data to clients.
Media data includes video, audio, video analytics and other metadata. <xref to
service specification>
- *Device IO service* enables an NVT to support physical inputs and outputs.
  <xref to service specification> An NVT can also implement the following
services to provide extended functionality:
- *PTZ service* enables an NVT to provide PTZ control if the device is a PTZ
  camera. <xref to service specification>
- *Imaging service* enables an NVT to provide configuration of image settings
  which affect the visual appearance of the video, for example, exposure time,
gain and white balance, focus control. <xref to service specification>
- *Video Analytics service* enables an NVT to provide video analytics
  functionality. <xref to service specification>

Beyond this, an NVT can also include additional ONVIF services, for example the
Recording service if support for local storage is required

# NVD (Network Video Display)


```
A Network Video Display (NVD) is an ONVIF device that receives media data over
an IP network and outputs it. For example, an NVD may be an IP network video
monitor which displays video and outputs audio. 
```

An NVD implements the following services to provide its core functionality:

- *Device service* enables an NVD to provide device management functionality
  such as device capabilities, system and network settings, security settings
and firmware upgrade.
- *Event service* enables an NVD to send events to clients.
- *Device IO service* enables an NVD to support physical inputs and outputs.
- *Display service* provides functions to enable a client to control and
  configure display devices.  The service introduces panes, each of which
occupies an area of the physical display.
- *Receiver service* enables an NVD to receive media streams from a media
  source e.g. a Network Video Transmitter (NVT). 


# NVS (Network Video Storage)

```
A Network Video Storage (NVS) is an ONVIF device that records media and
metadata received from a streaming device, such as an NVT, over an IP network
to a permanent storage medium. The NVS also enables clients to review the
stored data. For example, an NVS could be a network video recorder (NVR), a
digital video recorder (DVR) or a camera with embedded storage. 
```


- *Device service* enables an NVS to provide device management functionality
  such as device capabilities, system and network settings, security settings
and firmware upgrade.
- *Event service* enables an NVS to send events to clients.
- *Receiver service* enables an NVS to receive media streams from a media
  source e.g. a Network Video Transmitter (NVT), to be recorded.
- *Recording Control service* enables a client to manage recordings, and to
  configure the transfer of data from data sources to recordings. Managing
recordings includes creation and deletion of recordings and tracks.
- *Recording Search service* enables a client to find information about the
  recordings on the storage device, for example to construct a “timeline” view,
and to find data of interest within a set of recordings. The latter is achieved
by searching for events that are included in the metadata track recording.
- *Replay Control service* enables a client to play back recorded data,
  including video, audio and metadata. Functions are provided to start and stop
playback and to change speed and direction of the replayed stream. It also
enables a client to download data from the storage device so that export
functionality can be provided. 


# NVA (Network Video Analytics )

```

A Network Video Analytics (NVA) device is an ONVIF device that performs
analysis on media data and meta data received from from an IP streaming device.
For example, an NVA may be an analytics server device that receives and
analyses live data from a Network Video Transmitter to generate events, or
analyses recorded data from a Network Video Storage device, to perform forensic
searches. Evaluations may involve more than one media stream or metadata
enhanced media stream at a time. 

```


An NVA implements the following services to provide its core functionality:

- *Device service* enables an NVA to provide device management functionality such
  as device capabilities, system and network settings, security settings and
firmware upgrade.
- *Event service* enables an NVA to send events to clients.
- *Video Analytics service* enables a client to configure the video analytic
  algorithms of the NVA.
- *Video Analytics Device service* enables a client to configure NVA
  functionality, assign input streams and analytics to be performed as well as
control analytics processes and parameter.
- *Receiver service* enables an NVA to receive media streams from a media source
  e.g. a Network Video Transmitter (NVT). 


# detailed service definition

Refer to the [link](https://www.onvif.org/specs/DocMap-2.3.html)
