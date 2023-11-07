---
title: "Projects"
date: 2023-02-25T22:40:17+01:00
draft: false
heroStyle: "background"
layoutBackgroundBlur: true
---

This is a small collection of university and research projects I particularly enjoyed working on. I hope you find them interesting!

<br/>

## Detecting communication signals of weakly electric fish

To understand the meaning of certain communication signals, we need to be able to detect them. With chirps, a transient frequency modulation of the electric organ discharge, this is particularly challenging. In most of the research to date, fish where immobilized and stimulated artifically or kept physically seperated, conditions not particularly advantageous for natural communication. 

To solve this, I developed a detector based on a convolutional neural network that can detect chirps in freely behaving fish. The detector was initially trained on large datasets of simulated chirps, which suprisingly, after some fine-tuning, transferred well to real data.  With this version I was able to detect approximately 50 000 chirps, the largest dataset of chirps at that time. 

The following image shows a short snippet of a recording of two fish. Chirps are visible on a spectrogram as excursions in frequency from the baseline of one of the two fish. The dots indicate where the detector detected a chirp. 

![A spectrogram of a recording of two fish.](chirps.png "A spectrogram of a recording of two fish. The dots indicate where the detector detected a chirp.")

Using this detector, we performed first, preliminary analyses of the communication behavior of two fish competing for a shelter, which suggested that chirps might be used by the loosing fish to signal submission.

I am currently working on using the resulting dataset, manually annotating and correcting it to train a new detector based on a deep neural network that is taylored to detection tasks, that should perform even better on more complex recordings.

{{< github repo="weygoldt/cnn-chirpdetector" >}}

<br/>

## Singing in the dark: Synchronous frequency modulations of weakly electric fish

While looking through a two-week continuous recording of electric fish in their natural habitat, I noticed that some frequency modulations on the scale of seconds up two ten minutes happened in synchrony between two fish. I developed a covariance-based detector that can detect these modulations and used it to analyze the recordings. 

Using estimated positions of the fish over time, I was able to show that fish that participate in these interactions approach each other after they initated their "choir". This work resulted in a poster presented at the 2022 international conference of Neuroethology (ICN), that you can find in the github repository below. 

{{< github repo="weygoldt/synchronous-modulations" >}}

Additionally, I rendered some of these diadic interactions as videos, where you can see the fish moving as data points on the electrode grid and their frequencies evolving on the right.

{{< youtube id="ihDTMcn7LWM" title="Synchronous modulations" >}}

<br/>

## Colorblind direction cells in the zebrafish optic tectum

Behvioral experiments already showed that zebrafish are likely to not perceive motion if the sole feature that is moving is color. Instead, brightness differences between moving stimuli where needed to elicit a response (Orger and Baier, 2005). For a project during a lab rotation, we investigated how this behavior is reflected in the optic tectum, the main visual processing center in fish. 

To do this, we used two-photon calcium imaging to record the activity of direction selective neurons in the optic tectum of zebrafish larvae. In addition, we simultaneously recorded the optokinetic response, a behavioral proxy of motion perception. 

Our analysis indicated that the direction selective neurons in the optic tectum are most likely colorblind, meaning that they respond to brightness differences but not to color differences. This experiment also resulted in a poster as well as a more detailed report, both of which you can find in the github repository below.

{{< github repo="weygoldt/colorblind-directioncells" >}}

<br/>