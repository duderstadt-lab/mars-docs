---
layout: affine2D
title: How to calculate an Affine2D matrix
permalink: /tutorials/affine2D/HowToCalculateAffine2D/index.html
---

The Descriptor-based registration (2d/3d) in Fiji provide robust tools for determination of coordinate transformations. Transformation coordinates are needed to align the two channels of a dualview when analyzing single-molecule TIRF data. These coordinates are used for analysis of FRET datasets and also for direct viewing using the BigData Viewer.

Use the Fiji search bar to locate the plugin. Put in registration and select the plugin from the list:

<img height='363' src='{{site.baseurl}}/tutorials/img/search.png' width='708' />

Then go through the dialogs as they come up and use the setting you see below. For our data we have been doing an 2D affine transform and you definitely need a guassian mask for subpixel alignment.

<img height='373' src='{{site.baseurl}}/tutorials/img/dialog1.png' width='705' />

<img height='434' src='{{site.baseurl}}/tutorials/img/dialog2.png' width='702' />

<img height='565' src='{{site.baseurl}}/tutorials/img/dialog3.png' width='702' />

Here you can only use Sigma 1 and Threshold. Sigma 2 is for 3D data.

<img height='395' src='{{site.baseurl}}/tutorials/img/dialog4.png' width='702' />

<img height='766' src='{{site.baseurl}}/tutorials/img/dialog5.png' width='946' />

This is the magic! It does interpolation to achieve a really nice looking subpixel alignment!

The console outputs the coordinates that you will need to input in the Molecule Explorer when using the BigData Viewer or when using the [Transform ROIs](https://duderstadt-lab.github.io/mars-docs/docs/roi/TransformROIs/) command.
