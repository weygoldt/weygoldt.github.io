---
title: "A collection of data visualizations"
summary: "A gallery of recent data visualizations, from simple plots to conference posters and even a video."
heroStyle: "background"
showSummary: true
showAuthor: false
layoutBackgroundHeaderSpace: false
showTableOfContents: false
categories: ["Projects"]
tags: ["python", "datascience", "matplotlib", "dataviz", "design"]
date: 2024-05-19
draft: false
---

## Standalone plots

Here you see some data visualizations exclusively rendered in Matplotlib without
additional edits from graphical editors.

{{< gallery >}}
<img src="gallery/plot_assignment_feature_extraction.png" class="grid-w100 md:grid-w100 xl:grid-w50" />
<img src="gallery/chirp_detections_simultaneous.png" class="grid-w100 md:grid-w100 xl:grid-w50" />
<img src="gallery/competition_trial_overview.png" class="grid-w100 md:grid-w50 xl:grid-w50" />

<!-- <img src="gallery/detected_bbox_overview.png" class="grid-w100 md:grid-w50 xl:grid-w50" /> -->
<img src="gallery/plot_assignment_decision_boundary_autoencoder.png" class="grid-w100 md:grid-w50 xl:grid-w50" />
<img src="gallery/plot_behavioral_correlations_of_chirps.png" class="grid-w100 md:grid-w50 xl:grid-w50" />
<img src="gallery/plot_what_is_a_chirp_and_why_use_spectrograms.png" class="grid-w100 md:grid-w50 xl:grid-w50" />
{{< /gallery >}}

## Conference posters

The following list includes some posters from University projects as well as
conference presentations. All posters are exclusively rendered in LaTex.

<table>
    <thead>
        <tr>
            <th>Title</th>
            <th>Presented</th>
            <th>Link</th>
        </tr>
    </thead>
    <tbody>
         <tr>
            <td>Complex frequency modulations in freely interacting electric fish recorded in their natural habitat</td>
            <td>International Conference of Neuroethology, Lisbon 2022</td>
            <td><a target="_blank" href="https://drive.proton.me/urls/7SYFEYJM08#A2LugREOIEkR">pdf</a></br><a target="_blank" href="https://github.com/weygoldt/synchronous-modulations">github</a></td>
        </tr>
         <tr>
            <td>Automated detection of chirps in electric fish: Deep learning applied to ethology</td>
            <td>Annual Meeting of the Ethological Society, Muenster 2024</td>
            <td><a target="_blank" href="https://drive.proton.me/urls/83Z66SEED8#k3f6q9ieFiWZ">pdf</a></td>
        </tr>
    </tbody>
</table>

## Data videos

These were created by producing series of individual frames with Matplotlib
and combining them into videos with ffmpeg.

{{< youtube id="ihDTMcn7LWM" title="Synchronous modulations" >}}
