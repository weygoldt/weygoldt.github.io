---
title: "Chirp detector"
summary: "Electric fish produce fast frequency sweeps to communicate, called chirps. Here, I detected these chirps with a CNN-classifier on spectrograms that was trained on data I simulated."
heroStyle: "background"
showSummary: true
showAuthor: false
layoutBackgroundHeaderSpace: false
showTableOfContents: false
categories: ["Projects"]
tags:
  [
    "cnn",
    "efish",
    "neuralnetwork",
    "python",
    "timeseries",
    "spectrogram",
    "dsp",
  ]
date: 2023-11-18
draft: false
---

Understanding the significance of specific communication cues necessitates our
ability to detect them, particularly with transient frequency modulation
signals like chirps produced by an electric organ discharge in electric fish.
Previous research has mainly focused on immobilizing or artificially
stimulating fish or physically separating them, conditions unfavorable for
natural communication.

To address this challenge, I designed a convolutional neural network-based
detector capable of detecting chirps in freely behaving fish. Despite initially
training the model on simulated data, it surprisingly performed well on
real-world recordings after some fine-tuning. Using this version, I
successfully detected approximately 50,000 chirps, marking the largest dataset
at that time.

The following image illustrates a short segment of a recording featuring two
fish. Chirps are visible as frequency fluctuations from the baseline of one of
the two fish on a spectrogram. The dots indicate where the detector identified
a chirp.

![A spectrogram of a recording of two fish.](chirps.png "A spectrogram of a recording of two fish. The dots indicate where the detector detected a chirp.")

Performing preliminary analyses utilizing this detector, we discovered that
chirps could potentially be utilized by the losing fish to indicate submission
during competition for a shelter among two fish.

{{< github repo="weygoldt/cnn-chirpdetector" >}}
