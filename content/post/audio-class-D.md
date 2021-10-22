---
date: 2020-09-22
title: Why Class-D has high efficiency
weight: 10
---

# Introduction

This notes try to explain why class-D amplifier has high efficiency.

# Other type of amplifier

An amplifier could be made from a voltage divider. When the current flows
through a resistor, heat would be generated. Not all current is running through
the speaker.

# Why Class-D is different

The Class-D amplifier basically generates a train of rectangular pulse. The
pulse is applied directly on a speaker, there is no resistor in play. That is
to say when the switch is "on", all current is flowing through the speaker,
when it is "off", there is no flow running through it. So theoretically, it has
efficiency of 100%.

# Class-D is not a digital amplifier

Although it could be done digitally using timer to generate modulated PWM.

# How to select a correct speaker?

Refer to https://statics.cirrus.com/pubs/appNote/WAN0200_1.pdf

# Class-D amplifier 80%

If the power supply of BTL is a, the output amplitude would be 2a. RMS of a sine wave would be a. So the output power would be `a^2/4ohm` in case of 4ohm speaker. 

# Tips to measure the dissipated energy


>> The suitability of the loudspeaker should also be verified by measuring the
>> power consumption of the amplifier, under quiescent signal conditions, with
>> the loudspeaker connected and again with the loudspeaker disconnected. The
>> difference between these two measurements can be interpreted to be
>> equivalent to the power dissipated by the loudspeaker in filtering the Class
>> D switching energy.

>> Having calculated the high frequency switching energy dissipation, the
>> suitability of the loudspeaker should then be considered in two ways:

>> Firstly, it should be verified that power rating of the loudspeaker is
>> sufficient to handle the maximum audio power level and the Class D switching
>> energy combined. (For example, if the application is required to deliver 1W
>> of audio, and the Class D switching dissipation is 200mW, then the
>> loudspeaker’s power rating must be at least 1200mW.)


>> Secondly, designers should consider whether the level of Class D switching
>> power dissipation is acceptable in terms of the overall system efficiency
>> and, where applicable, the anticipated battery endurance: if the power
>> efficiency is unacceptable, then it may be possible to select an alternative
>> loudspeaker which offers more efficient filter characteristics.

# References 

- How to Measure THD+N for Class-D Amplifier

https://e2echina.ti.com/cfs-file/__key/telligent-evolution-components-attachments/00-42-01-00-00-16-05-38/RC-Filter-Box-For-Class_2D00_D-Output-Power-and-THD_2B00_N-Measurement.pdf

- Design an RC Filter

https://www.ti.com/lit/an/slyt198/slyt198.pdf?ts=1600686394731&ref_url=https%253A%252F%252Fwww.google.com%252F


