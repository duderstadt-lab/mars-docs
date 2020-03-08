---
layout: image
title: Discoidal Averaging Filter
permalink: /docs/image/DiscoidalAveragingFilter/index.html
---
This command generates a Discoidal Averaged image from a copy of the current slice of the selected image. It generates a single filtered image and does not work on videos. As a consequence, this command is intended primarily for diagnostic purposes to directly see the result of filtering. This discoidal filtering can then be used in the Peak Finder that Tracker as an integrated step with the optimized settings.

#### Inputs

<img align='center' src='{{site.baseurl}}/docs/image/img/Discoidal Averaging Dialog.png' width='300' />

* *Image* - The current slice of the selected active image will be used. This is a required input but doesn't show up in the dialog.
* *use ROI* - If checked a subregion of the image will be used for processing. Otherwise, the entire image will be used. You can also add a selection with the box tool, so making a rectangular ROI to the image. This Roi will activate this box and add the settings below it.
* *ROI x0* - Upper left corner x0 position of ROI.
* *ROI y0* - Upper right corner y0 position of ROI.
* *ROI width* - width of the ROI.
* *ROI height* - height of the ROI.
* *Inner Radius* - Inner Radius beyond a single pixel used for discoidal processing. This should be enough to include your peaks.
* *Outer Radius* - Outer Radius that determines the radius of the ring used for calculating the background for discoidal averaging. This needs to be large enough so peak signal is exlcuded.

<img align='center' src='{{site.baseurl}}/docs/image/img/DS filter.png' width='600' />

#### Output

* *Image* - Discoidal averaged filtered image.
