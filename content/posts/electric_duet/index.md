---
title: "Electric duet"
summary: "Some electric fish synchronously modulate their frequencies in duets. I detected these events using a custom covariance-based event detector on frequency estimates from spectrograms."
heroStyle: "background"
showSummary: true
showAuthor: false
layoutBackgroundHeaderSpace: false
showTableOfContents: false
categories: ["Projects"]
tags: ["efish", "covariance", "research", "timeseries", "spectrogram"]
date: 2022-11-07T12:43:20+01:00
draft: false
---

While sifting through a two-week continuous recording of electric fish in their
natural habitat, I observed synchronous frequency modulations on a scale of
seconds to two ten minutes between two fish. To detect these modulations, I
developed a covariance-based detector, which I then used to detect these
events on recordings. The following plot shows some examples of detected modulations:
![modulations](modulations.png "Synchronous rises of the EODf betweeen two
individuals recorded in freely interacting fish in their natural habitat.")

To detect spatio-temporal interactions, particularly when fish chased each
other, I build a cost function that peaks when (1) fish swim in the same
trajectories relative to each other, (2) velocities of both fish increase, and
(3) fish accelerate rapidly. By analyzing the estimated positions of fish over
time, I demonstrated that those involved in frequency duets approach one
another following the initiation of their "choir." I also rendered videos
depicting some of these interactions: Fish moving as data points on an
electrode grid and their frequencies changing on the right-hand side.

{{< youtube id="ihDTMcn7LWM" title="Synchronous modulations" >}}

The resulting output from this effort was displayed as a poster at the 2022
International Conference of Neuroethology (ICN). This poster can be accessed
through the link provided in the GitHub repository below.

{{< github repo="weygoldt/synchronous-modulations" >}}
