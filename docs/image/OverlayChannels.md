---
layout: image
title: Overlay Channels
permalink: /docs/image/OverlayChannels/index.html
---
This simple command can be used to combine several individual videos into one. It creates a combined video in which each of the original videos is categorized with a different value for 'Channel' (C). The output is a combined video in which the channel can be selected by using the slider at the bottom. Make sure to adjust the contrast for each channel separately to obtain a proper visualization.

#### Input

<img align='center' src='{{site.baseurl}}/docs/image/img/img6.png' width='550' />

* *Add To Me* - Select the video to which the other video should be added.
* *Transform Me* - Select the video to be added to the other video.
* *Keep originals* - Tick box in case the original video documents should be saved (preffered option).

Affine2D Transformation Matrix: coordinates found with the Affine2D calculation (see [tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/))  
* *m00*
* *m01*
* *m02*
* *m10*
* *m11*
* *m12*

<img align='center' src='{{site.baseurl}}/docs/image/img/img5.png' width='350' />

#### Output

* A single video with individual channels representing the input videos.

<img align='center' src='{{site.baseurl}}/docs/image/img/img7.png' width='450' />
