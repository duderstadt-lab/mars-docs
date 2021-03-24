---
layout: image
title: Overlay Channels
permalink: /docs/image/OverlayChannels/index.html
---
This simple command can be used to combine several individual videos into one. It creates a combined video in which each of the original videos is categorized with a different value for 'Channel' (C). The output is a combined video in which the channel can be selected by using the slider at the bottom. Make sure to adjust the contrast for each channel separately to obtain a proper visualization.

#### Input

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img6.png' width='550'/></div>

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

* *Thread count* - Determines how much computing power of your computer will be devoted to this calculation. A higher thread count decreases computing time.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img5.png' width='350'/></div>

#### Output

* A single video with individual channels representing the input videos.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img7.png' width='450'/></div>

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@ ImageJ ij

import de.mpg.biochem.mars.ImageProcessing.*

//Make an instance of the Command you want to run...
final OverlayChannelsCommand overlayChannels = new OverlayChannelsCommand();

//Set the parameters required for the command
overlayChannels.setAddToMe(img1.tif);
overlayChannels.setKeepOriginals(True);
overlayChannels.setTransformMe(img2.tif);
overlayChannels.setM00(1);
overlayChannels.setM01(0);
overlayChannels.setM02(0);
overlayChannels.setM10(0);
overlayChannels.setM20(1);
overlayChannels.setM12(0);

//Run the Command
overlayChannels.run();
```
