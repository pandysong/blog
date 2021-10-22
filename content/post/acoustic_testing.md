---
date: 2020-12-04
title: Acoustic Testing and Issue Fixing
weight: 10
---

# SpeakerResponseTesting

https://www.diyaudioandvideo.com/Tutorial/SpeakerResponseTesting/


```
I don't see anything in any of these tests that would warrant any additional
modifications to the crossover. Given this is an off-the-shelf speaker that is
to be expected. Here are some things to look for in speaker testing:

A large drop (~20dB) at a crossover point would indicate that the drivers are
canceling each other out. Try reversing polarity (swap + & -) on one of the
drivers to see if the dip goes away. If the response is louder at the crossover
point with the polarity reversed then it should probably be reversed.  Any
other narrow spike (~10dB) (Ex: at the resonant frequency of a tweeter) might
need to be fixed using a Series Notch Filter.  A wide dB gain in the response
that corresponds to a single driver's output could be caused by that driver
having a higher sensitivity than the others. This can be resolved with a Driver
Attenuation Circuit / L-Pad.  Other dB gains over a wide area that are not
attributed to different driver sensitivities can be resolved using a Parallel
Notch Filter.  A subwoofer with a declining response curve (caused by rising
impedance of the large coil in the crossover at higher frequencies) can be
fixed with an Impedance Equalization Circuit.  A driver with a rising response
curve can be resolved with a Contour Network.
```

# What is Fs (free air resonance)

```
The Fs (free air resonance) of the tweeter is at 1000Hz. This is the frequency
at which the tweeter will resonate, and produce a large positive spike in the
frequency response. A series-notch filter will be used to remove this spike.
```


# 

```
A sealed space, or pressure chamber, lifts the low frequencies by 12 dB/octave
below a frequency related to the dimensions of the enclosed volume. With an ear
canal that is approximately 2 cm long, this magical frequency is located at
about 5 kHz. With more than five octaves separating the bass range from this
frequency, imagine the bass boost!
 ```

5Khz/32 = 156Hz
