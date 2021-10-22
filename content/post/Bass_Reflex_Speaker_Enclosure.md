---
date: 2021-03-14
title: Bass Reflex Speaker Enclosure
weight: 10
---

# Introduction

This is the notes on reading LPRNet.


# Quoting

```

This is probably the most common enclosure in use today.  It was used in very
early speaker systems, but it was basically a 'trial-and-error' design until
the loudspeaker parameters were properly quantified by Neville Thiele and
Richard Small.  This allowed mathematical calculation of the enclosure and port
sizes, and it was then possible to design a system, build it, and have it
perform as expected.  Many of the early 'tuned' boxes were what's now commonly
referred to as 'boom boxes', because they had excessive and often 'one note'
bass.  Countless programs have been written to allow users to design an
enclosure, based on the Thiele-Small parameters.  This has removed much of the
guesswork, but by themselves, the programs are (mostly) unable to provide a
complete design.  Most provide the necessary internal volume and port (vent)
diameter and length, but further 'tweaking' is nearly always needed.

In these enclosures, the rear radiation is utilised to boost the bass response
below the loudspeaker driver's resonant frequency.  The combination of the
enclosure volume and the vent length and diameter form a Helmholtz resonator,
which (when done properly) reinforces the low frequency response without
creating excessive bass and/or poor transient response.  It's important to
understand that the Thiele-Small parameters are 'small signal', meaning that
the performance is not necessarily the same at high power levels.  Only the
bass region is affected by a bass reflex enclosure, and mid to high frequencies
still need to be absorbed within the enclosure.

```

# Key Points

- It could be calcuated by parameters (thanks to Neville Thiele and Richard Small)
- Have many programs/application could be used to design it using the
  Thiele-Small parameters.
- Futher tweaking is needed always.
- Combination of vent length and diameter form a Helmholtz resonator. It
  reinforces the low frequency response without creating excessive bass and/or
  poor transient response.

# Quoting


```

These parameters are published in specification sheets by driver manufacturers
giving designers a guide for selecting drivers for loudspeaker systems.

```

Key Points:

- Parameters are in datasheet of a driver.

# Parameters could be tested and confirmed


Refer to : https://courses.physics.illinois.edu/phys406/sp2017/Student_Projects/Spring01/PPoongbunkor/Piya_Poongbunkor_TS.pdf

## Most Important Parameters

```
There are many Thiele-Small parameters that can be measured from loudspeakers,
but the three most important ones are: free air resonance (F(s)), Q(ts), and volume of
suspension (V(as)). The free air resonance is the resonant frequency of the speaker found
in free air. Below, a typical impedance frequency response of a loudspeaker is shown on
a log scale.
```

```

The resonant frequency is considered the threshold at which a speaker can be
used. 

```


```

The Q(ts) or total Q of the speaker, also known as total quality factor, is
basically describing the resonance curve of the speaker. The higher and thinner
of a peak the curve has, the lower Q it will have. The rounder a peak the curve
has, the higher Q it will have

```

```

The Q(ts) value of the speaker is an indicator of the “resonant magnification”
of the speaker. This value is important in deciding what type of enclosure a
speaker should go into. Also, a high value for Q(ts) will have a “warmer” tone,
while lower values have a more clear and “shallower” sound, according to The
Loudspeaker Design Cookbook by Vance Dickason. A very high Q(ts) or a very low
value for Q(ts) is not good. The speakers towards the middle of the table
should be of higher quality and should sound better than those at the edges,
since the table is sorted with respect to Q(ts).

```

```

One of the biggest things that you can see is that the resonant frequency is
actually shifted down when the speaker is in the cabinet as opposed to in free air. In the
graphs, above, the F(s) of the speaker in free air is about 58 Hz, while the F(s) of the
speaker in the cabinet is about 53 Hz. From these observations, we can draw the
conclusion that speakers work better in a cabinet or infinite baffle rather than in free air.
This is because when a speaker is in free air, the low frequency sound waves emanating
from behind and in front of the speaker cone may interfere with each other and cancel
out. If the speaker is mounted on an infinite baffle, the waves behind the cone will not
interfere with the audio signals produced by the speaker in front of the baffle. The other
obvious difference we can see from a speaker in free air and a speaker in the cabinet is
that the in the free air situation, the curve is much more smooth than in the cabinet. The
true characteristic of the impedance curve is amplified in the cabinet.

```

Key Points:

- F(s) is lower (which is better) in enclosure.
- The reason is that low frequency emanating from behind, which is not
  conflicting with the low frequency sound from the front.
- curve is more smooth in free air than in enclosure.

# Practising

## How to measure the parameters

https://makersportal.com/blog/2019/1/21/loudspeaker-analysis-and-experiments-part-i#thiele_small
https://github.com/makerportal/thiele_small_parameters


## Equivalent Compliance Volume, Var


Refer to : https://makersportal.com/blog/2019/2/20/loudspeaker-analysis-and-experiments-part-ii

```
The compliance value above allows us to calculate another important parameter
called the equivalent compliance volume, Var , which is a measure of the amount
of air that the suspension system is capable of moving. It is measured in
liters or m3, and generally is a measure of what size enclosure the loudspeaker
should be housed in


```

# YuTube Channel

https://www.youtube.com/channel/UCWOhWAOydPUqillkpt5UlaA

# Tools

# VituixCAD Loudspeaker

https://kimmosaunisto.net/

## audiotester

http://www.audiotester.de/

## 

http://www.interdomain.net.au/~bodzio

## WinSpeakerz

https://trueaudio.com/win_abt1.htm

## Could be measured using Audio Precision

# References

[1] https://sound-au.com/articles/enclosures.htm
[2] https://en.wikipedia.org/wiki/Neville_Thiele
[3] https://en.wikipedia.org/wiki/Richard_H._Small
[3] https://www.youtube.com/watch?v=T3BbyYBrCJ8
[4] https://makersportal.com/blog/2019/1/21/loudspeaker-analysis-and-experiments-part-i
[5] https://courses.physics.illinois.edu/phys406/sp2017/Student_Projects/Spring01/PPoongbunkor/Piya_Poongbunkor_TS.pdf
[6] https://www.ap.com/electro-acoustic-test/
[7] http://www.hornlautsprecher.net/Dokumente/Grundlagen/Tiele-Smal-LSPRMTRS604.htm
