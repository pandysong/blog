---
date: 2021-11-01
title: "AEC: Acoustic Echo Cancellation"
weight: 10
---


# Non-Linear AEC

The paper[1] describes that the echo takes effects when the echo exceeds
30-50ms. In modem communication system, the delay is far more than that.

In the paper, two method to handle non-linearity

- first an approach based on loudspeaker emulation and 
- the second on the linearisation of the loudspeaker.

## Echo Return Loss Enhancement (ERLE)

## Linear Algorithms performance

```

Frequency block-filtering approaches are shown to be the most disturbed in
non-linear environments. An Adaptive Projection Algorithm (APA) approach is
furthermore shown not to perform any bet- ter than a standard Normalized-LMS
(NLMS) algorithm


```

```

The new theoreti- cal analysis better accounts for observed experimental
results than any existing theory and shows why NLMS algorithms often perform
better than APA algorithms in the presence of non-linear distortion

```

## Loudspeaker Modelling

```

Loudspeaker modelling based on harmonic summation

Loudspeaker modelling based on polynomial expansion

```

## Loudspeaker pre-processing

```

This approach places no constraints on the use of any particular AEC algorithm
and avoids the introduction of distortion in the error signal which can occur
with alternative approaches to non-linear AEC.

```


# References

[1] Non-linear AEC with loudspeaker modelling and pre-processing, 2012
