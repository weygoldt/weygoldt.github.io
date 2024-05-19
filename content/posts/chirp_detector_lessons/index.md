---
title: "Lessons learned detecting communication of electric fish"
summary: " "
categories: ["Post", "Blog"]
tags:
  [
    "signalprocessing",
    "python",
    "machine-learning",
    "deep-learning",
    "neuroethology",
    "biology",
    "deeplearning",
    "machinelearning",
    "ai",
  ]
date: 2024-01-02T10:44:24+01:00
draft: true
---

Title: "Decoding the Symphony of Weakly Electric Fish: A Journey into Automated
Chirp Detection and Deep Learning"

In the past year, my journey in neuroethology has taken an exciting turn as I
delved into the intricate world of the communication of weakly electric fish.
What started as a lab practical on chirp detection evolved into my masters
thesis, culminating in the development of an algorithm that automates the
detection of these elusive communication signals. As I started this journey
almost exactly one year ago, now it is time to reflect on this project. In this
blog post, I'll take you through the key milestones, sharing the challenges,
triumphs, and valuable lessons learned along the way.

## The Challenge: Chirps and the Need for Automation

Weakly electric fish communicate through brief signals known as chirps.
Detecting these chirps manually in recordings with multiple fish is a daunting
task, prompting the need for automated solutions. Until now, chirp detection in
complex recordings has been largely done by hand, limiting the quantitative
analysis of this intriguing behavior. The datasets we are working on often
consist of hundrets of hours of continuous recording. It is ablolutely
infeasible, to manually search for chirps by hand, a signal that can be as
short 20 ms.

## Detector 1: A Hand-Tuned Approach

My journey began with a manual approach – a combination of filters, rectifiers,
and hand-tuned parameters. I was introduced to this projects in a lab practical
for my masters program in Neurobiology. While effective for simpler datasets,
the complexity of the recordings revealed edge cases and diverse chirp types.

[Check out the initial project
here.](https://github.com/weygoldt/gp-chirpdetector)

The absence of a ground truth in hours-long recordings led me to simulate an
initial ground truth, creating artificial fish interactions and combining
background noise from real recordings: This required developinig random walkers
with continuously adjustable velocities and trajectories to mimic the natural
movement of fish in a virtual space. I also simulated the electric organ
discharge of fish, including communication signals. The last step was, to bring
all this together: I created a virtual grid of `n` recording electrodes and
modulated the amplitude of each fish based on the distance of a fish to every
electrode on the grid at a given point in time. The result: Ground truth
simulations that closely resemble electrode grid recordings of electric fish in
their natural habitat, including their communication repertoire.

The resulting simulation pipeline now resides in my
[`gridtools`](https://github.com/weygoldt/gridtools) package. This laid the
foundation for the next step:

## Detector 2: First Steps with Neural Networks

Realizing the limitations of the hand-tuned approach, I ventured into deep
learning. I built a custom convolutional neural network (CNN) that classified
spectrogram snippets as either 'chirp' or 'no-chirp.' This involved
GPU-accelerated processing of days of recordings, with post-processing steps
enhancing the detection accuracy. Despite being trained mostly on artificial
data, the model achieved an impressive F1 score of 0.91.

[Explore the second detector project
here.](https://github.com/weygoldt/cnn-chirpdetector)

But I quickly learned, that in the world of deep learning and computer vision,
there are mainly two groups of people: The ones that almost exclusively spend
their time developing new models and the ones that use theses models. As my
field of research lies in Neuroethology and not Deep Learning, I am the latter.
So instead of reinventing the wheel, I wanted something tried and trusted: Why
build a detection algorithm when there are already many other ones on which people
spend much more time investing in the architecture?

## Detector 3: Transitioning to Object Detection Algorithms

While successful, the second detector faced challenges with varying chirp
durations and frequencies. See, the sliding window was always the same size:
Some chirps were too large to fit into the box and some where so small that
they were misclassified. To address this, I dived deeper into deep learning and
computer vision, adopting object detection algorithms. These algorithms predict
any size of bounding box, which would resolve this limitation oft the first
detector. The transition involved simulating large chirps, extracting
parameters from real datasets, and generating even more artificial datasets. I
opted for a faster R-CNN model, implementing a fine-tuning pipeline with
k-folds cross-validation. Challenges persisted, leading to manual labeling of
600 images to refine the model's performance on real recordings.

The current detector is hosted
[here](https://github.com/weygoldt/chirpdetector). It will be further refined
and will come to great use in my upcoming PhD project.

## Lessons Learned

This journey into automated chirp detection taught me invaluable lessons. I
gained insights into deep learning methods, realizing that the majority of the
work lies in setting up code infrastructure and data pipelines. Managing
large-scale projects enhanced my Python coding skills, emphasizing the
importance of code cleanliness, type hinting, and documentation. Commit hooks
became essential in maintaining code quality. Additionally, this project
sparked a newfound interest in applying machine learning to research, prompting
further education in both machine learning and deep learning.

In conclusion, what may seem like a "simple" problem turned into a fulfilling
and educational experience. The joy of unraveling the mysteries of weakly
electric fish communication through the lens of data science and machine
learning has opened new horizons. As I continue to refine the algorithm and
face challenges ahead, the journey remains an exhilarating and rewarding ride.

Stay tuned for more updates on the intersection of neuroethology and
cutting-edge technology!
