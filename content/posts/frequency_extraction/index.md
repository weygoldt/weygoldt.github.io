---
title: "Forward and backward Fourier transformations"
summary: "One of the most flexible tools to de-noise signals based on spectral components"
#showSummary: true
categories: ["Post", "Blog"]
tags:
  [
    "signalprocessing",
    "preprocessing",
    "Fourier",
    "spectrogram",
    "datascience",
    "python",
  ]
date: 2023-03-12T14:29:15+01:00
draft: true
---

Signals we record often contain noise. This becomes problematic in cases where this noise infers with
the analysis of said signals. In cases, where the signal of interest is limited to a certain frequency
band and the noise to another, we can apply simple filters (lowpass, highpass, bandpass) to denoise the
signal. In some cases however, the frequency component of the signal changes. This is the case when working
with communication signals of weakly electric fish. Each fish has its own baseline frequency but to
communicate, they can increase their frequency. If we now want to extract the signal created by a single
fish in a recording that includes multiple individuals, we face the problem that a bandpass filter
would need to have a changing center frequency of its passband to achieve this. Instead, we can perform
a **short time fourier transformation (stft)** to generate a spectrogram, then mask all frequency
components of this spectrogram that are not needed and use this spectrogram for an inverse stft to
get an extracted signal of just a single fish.

This post is a documentation of how this process can be implemented in Python.

## Fourier transformation and spectrograms

## The inverse Fourier transform

## The problem of ridge detection

## Implementation in Python

## Conclusion and application
