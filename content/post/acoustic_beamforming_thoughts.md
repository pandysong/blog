---
date: 2021-10-29
title: "Acoustic Beamforming Algorithm"
weight: 10
---

# Question Mark

When reading the paper of acoustic beamforming of mask based algorithm, I am a
bit lost on why do we need to split the signal to frequency bin.

```
https://arxiv.org/pdf/2005.09587v2.pdf
```

Above paper descirbes how the mask the calculated with a lot of terms

- TDOA (Time Difference Of Arrival)
- Stearing Vector (Used to cancel the phase differences)

But the logic behind that is not explained, why do we bother to split the
spectrum to a set of frequency bin?


# Answer

The reason is that the stearing vector depends on the frequency. As we could
tell from the euqation 2. 

Stearing Vector is used to cancel teh phase differences on the input signals to
two microphones (or multiple microphones in other paper). The vector depends on
the frequency. If frequency changes, the vector changes as well. 

# Basic Concept

Why do we choose cross-spectrum to estimate the masks?

Because cross spectrum contains the information of the delay and colleration of
two time series (two signals from two microphones).

## Features from Cross-Spectrum

Again, Cross spectral analysis allows one to determine the relationship between
two time series as a function of frequency.  So this is the basic of the
features.

In the mentioned paper, two features is extracted from the cross-spectrum

- log absolute value of cross spectrum
- and the phase from cross spectrum

Two features are concatenated as indicatied in equation 11.

Why do we need `log` absolute value? We want to make it as linear as possible,
since deep learning framework is a linear system.

## BLSTM

The deep learning allows to mapping the features to the masks.
