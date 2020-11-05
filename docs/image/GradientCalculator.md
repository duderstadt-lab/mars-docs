---
layout: image
title: Gradient Calculator
permalink: /docs/image/GradientCalculator/index.html
---
This command calculates the gradient (slope) of consecutive pixels in the y direction from top to bottom. The resulting gradient image or images are saved as an image sequence to the directory specified. This is very useful for detecting the edges of stained DNA molecules. If the DNA molecules are aligned with the y-axis then the gradient image will have a positive peak at the top and negative peak at the bottom. Usually images can't have negative values since pixels always have positive values, but in this case the output is a float image, so this is allowed.

At the boundaries, a reflected mirror image is used to calculate the slope. Sometimes this can cause edge effects, but not normally.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/Gradient Example.png' width='700'/></div>

#### Inputs

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/Gradient Dialog.png' width='600'/></div>

* *Image* - An image or sequence of images to calculate the gradient for. This doesn't show up in the dialog since the active image is taken, but this is a required input.
* *fitLength (in pixels, must be an odd number)* - An odd integer number of pixels to use for slope determination greater than or equal to 3. This number must be odd so that the calculated slope can be placed back as the value of the middle pixel with equal numbers on either side. There is no error checking for this at the moment, so you will get unpredictable results and errors with even numbers - do not use them. Usually 5-11 is a good range for this parameter.
* *Directory* - The folder where the corrected image sequence will be saved.

#### Output

* *Image* - A sequence of gradient tif images saved to the directory given. You can then open these as a new virtual stack and continue processing. Usually the next step is to use the [PeakFinder](../PeakFinder) and/or [PeakTracker](../PeakTracker) to find and track the DNA edges. The locations found by these tools should overlay perfectly with the original image prior to running the gradient tool. Meaning the slope is place at the same pixel position where it was calculated.

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@ File(label="Select an output directory", style="directory") directory
#@ ImageJ ij

import de.mpg.biochem.mars.ImageProcessing.*

//Make an instance of the Command you want to run...
final GradientCommand gradientCalculator = new GradientCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
gradientCalculator.setContext(ij.getContext())

gradientCalculator.setImage(image)
gradientCalculator.setFitLength(7)
gradientCalculator.setDirectory(directory);

//Run the Command
```
