---
date: 2020-05-22
title: auditory masking of noise
weight: 10
---

# Introduction

This note records some interesting points about acoustic.

# Nature of Human hearing

An good explanation of human hearing nature in [the
paper](https://www.mp3-tech.org/programmer/docs/e95_1.pdf): 

> Give a pure tone with a certain frequency and intensity, for a normal
> listener there is a masking threshold function associated with this tone such
> that if noise is added to the tone and the power spectrum of the noise is
> strictly below the masking threshold at all frequencies, that noise will be
> inaudible, i.e., it will be completely masked by the tone. In General the
> masking threshold has a peak at the frequency of the tone, and monotonically
> decreases on both sides of the peak. This means the noise components near the
> tone frequency are allowed to have higher intensities than other noise
> components that are farther away from that frequency while remaining
> inaudible.

> A short segment of a speech signal can be considered as a superposition of
> many sine waves. If each of these sine waves were presented alone to a normal
> listener, there would be an associated masking threshold function with a peak
> at the frequency of that sine wave. When all such sine waves are
> superimposed, their associated threshold functions must also superimpose.
> Exactly how these functions interact with each other is unknown. However, no
> matter how complicated the interaction might be, there must exist an overall
> masking threshold function for the given segment of speech signal such that
> an added noise will be inaudible if its power spectrum is below the threshold
> at all frequencies. The overall masking threshold function follows to some
> extent the spectral peaks and valleys of the speech spectrum. (The
> supra threshold masking curves for limiting noise to a given level of
> subjective loudness will be similar in shape.) This characteristic behavior
> of the masking threshold function is more commonly associated with the
> spectral envelope of speech.
