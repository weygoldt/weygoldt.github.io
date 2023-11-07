---
title: "Hardware and software development"
date: 2023-11-06T17:04:49+01:00
draft: false
heroStyle: "background"
layoutBackgroundBlur: true
---

To be able to record and analyze the data I need for my research, I contribute in the development of hardware and software projects.
The following list gives an overview of the projects I am currently working on.

## Submersible data loggers to record the activity of electric fish in their natural habitats

![Data logger](logger.jpg "A submersible data logger to record the electric organ discharge of weakly electric fish in the field.")

My main interest lies in recording natural behavior of weakly electric fish using electrode grids. But developing this system has led to multiple interesting side projects that I am currently contributing on.

- Submersible data logger that can be deployed in the field to record the electric organ discharge of weakly electric fish. Depending on the power supply, these loggers will able to record for multiple months and the housings are pressure rated to 100 meters depth.

- [Submersible data loggers for SCUBA divers](https://github.com/janscience/FishFinder/tree/main/R42-fishrecorder): Can be carried by scuba divers to record electric activity at depth. It is also equipped with an LED to be able to synchronize the electric activity with video recordings.

## [Gridtools](https://weygoldt.com/gridtools): A python package to analyze electrode grid data

In the current data pipeline, recordings fish frequencies are tracked using the [`wavetracker`](https://github.com/tillraab/wavetracker), which results in a quite complicated data structure. To simplify the analysis of the data, I developed a Pydantic data model that can be used to easily access the data. The package also includes functions to analyze, visualize, preprocess and export the data. I am also working on a simulation module that is capable of simulating a whole electrode grid recording, including multiple, moving fish that communicate with each other.

## [Chirpdetector](https://weygoldt.com/chirpdetector): A python package to detect and analyze brief communication signals of weakly electric fish

Chirps are one of the two main ways of communication in wave-type weakly electric fish. Chirps are modulations of the electric organ discharge frequency that last between 20 ms and 200 ms. This makes detection of chirps hard, when multiple fish are present in the same recording. The only way is using a spectrogram with a high temporal resolution. The `chirpdetector` tackles this challenge and uses a faster-RCNN to detect chirps in spectrograms. This will allow to analyze the communication behavior of freely behaving fish in their natural habitat quantitatively for the first time, which is the main goal of my master's thesis.

