---
date: 2020-09-22
title: Why decimation but not only downsampling?
weight: 10
---

# Introduction

The statement in https://dspguru.com/dsp/faqs/multirate/decimation/ well
explains why we need decimations:

```
In most cases, though, you’ll end up lowpass-filtering your signal prior to
downsampling, in order to enforce the Nyquist criteria at the post-decimation
rate. For example, suppose you have a signal sampled at a rate of 30 kHz, whose
highest frequency component is 10 kHz (which is less than the Nyquist frequency
of 15 kHz). If you wish to reduce the sampling rate by a factor of three to 10
kHz, you must ensure that you have no components greater than 5 kHz, which is
the Nyquist frequency for the reduced rate. However, since the original signal
has components up to 10 kHz, you must lowpass-filter the signal prior to
downsampling to remove all components above 5 kHz so that no aliasing will
occur when downsampling.

This combined operation of filtering and downsampling is called decimation.


What happens if I violate the Nyquist criteria in downsampling or decimating?

You get aliasing–just as with other cases of violating the Nyquist criteria.
(Aliasing is a type of distortion which cannot be corrected once it occurs.)
```

The link also explains why we bother to use multi-stage decimation and why we
use FIR filter instead if IIR filter to filter before downsampling.
