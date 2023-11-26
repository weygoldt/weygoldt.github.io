---
title: "Hardware and software development"
date: 2023-11-06T17:04:49+01:00
draft: false
heroStyle: "background"
layoutBackgroundBlur: true
---
In order to collect and evaluate the data required for my research, I participate in the creation of both hardware and software initiatives. Please find below a summary of the current projects I am engaged with.

## Submersible data loggers to record the activity of electric fish in their natural habitats

![Data logger](logger.jpg "A submersible data logger to record the electric organ discharge of weakly electric fish in the field.")
My primary interest involves documenting the natural behaviors of weakly electric fish using electrode grids. However, developing this system has given rise to several intriguing side projects that I am presently working on.

- An underwater data logger designed for field deployment to record the electric organ discharge of weakly electric fish. These loggers are capable of recording for extended periods, with housings rated for pressures up to 100 meters depth.

- A submersible data logger suitable for SCUBA divers to capture electric activity at great depths. This device is also equipped with an LED to facilitate synchronization with video recordings. You can find an overview [here](https://github.com/janscience/FishFinder/tree/main/R42-fishrecorder).

## [Gridtools](https://weygoldt.com/gridtools): A python package to analyze electrode grid data

In the existing data pipeline, fish frequencies are recorded using the [`wavetracker`](https://github.com/tillraab/wavetracker), resulting in a rather intricate data structure. To streamline the analysis of the data, I have created a Pydantic data model that enables effortless access to the information. The package also includes functions for analyzing, visualizing, preprocessing, and exporting the data. Additionally, I am currently developing a simulation module capable of simulating an entire electrode grid recording, including moving, mobile fish that communicate with one another.

## [Chirpdetector](https://weygoldt.com/chirpdetector): A python package to detect and analyze brief communication signals of weakly electric fish

Chirps are one of the two primary methods of communication utilized by wave-type weakly electric fish. These chirps involve modulations of the electric organ discharge frequency that range from 20 ms to 200 ms, making detection challenging when multiple fish are present within a single recording. The only viable approach is through the utilization of a spectrogram with high temporal resolution. The `chirpdetector` addresses this challenge by employing a *faster-RCNN*, a object detection algorithm for images, to detect chirps within spectrograms. This enables the quantitative analysis of communication behavior among freely behaving fish in their natural environment, which is the primary objective of my master's thesis.
