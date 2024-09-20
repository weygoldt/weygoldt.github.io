---
title: "Colorblind direction cells"
summary: "Some neurons in thet zebrafish brain respond to movement direction. We recorded and analyzed the activity of these neurons and showed, that these cells probably rely on luminance alone to perceive motion."
heroStyle: "background"
showSummary: true
showAuthor: false
layoutBackgroundHeaderSpace: false
showTableOfContents: false
categories: ["Projects"]
tags: ["zebrafish", "caimg", "neuroscience", "datascience"]
date: 2022-12-08
draft: false
---

Experimental research has shown that zebrafish are less likely to perceive
motion when the only moving element is color. Instead, it is the differences in
brightness between moving stimuli that trigger a response (Orger and Baier,
2005). As part of a lab rotation project, we investigated how this behavior
manifests in the optic tectum, the primary visual processing center in fish.

To achieve this, we employed two-photon calcium imaging to record the activity
of direction selective neurons in the optic tectum of zebrafish larvae.
Additionally, we simultaneously measured the optokinetic response, a behavioral
indicator of motion perception.

The resulting dataset consisted of a z-stack of fluorescence microscopy images
and the eye movements of the zebrafish embryo. After segmenting the cells with
[suite2p](https://github.com/mouseland/suite2p) we analyzed the calcium
activity (i.e., the luminance of each cell) and the eye velocities with respect
to the stimulus that was shown to the fish.

The ensuing graphs illustrate a comparable pattern between the calcium
activities (neural signal) and eye velocities (behavioral output) for the same
stimulation conditions. ![calcium activity](caneuro.png "Calcium acitivty in
the Zebrafish optic tectum when stimulated with different levels of chromatic
and achromatic contrasts.") ![behavioral output](cabehav.png "Behavioral output
measured in the eye velocities during the optokinetic response shows a similar
pattern compared to the neural activity.")

Our analysis suggested that the direction selective neurons in the optic tectum
are likely colorblind, as they primarily respond to variations in brightness
rather than color distinctions. As a result of this experiment, we have
produced not only a poster but also a more comprehensive report, both of which
can be found in the GitHub repository provided below.

{{< github repo="weygoldt/colorblind-directioncells" >}}

Image by [Zeiss](https://www.zeiss.com/microscopy/en/applications/life-sciences/cell-imaging.html)
