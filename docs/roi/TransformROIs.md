---
layout: roi
title: Transform ROIs
permalink: /docs/roi/TransformROIs/index.html
---
This command is used to transform detected peaks from one channel of a dualview to another. The expected input is a table with Affine2D transformation parameters generated using the Descriptor-based registration plugin and a set of PointRois in the RoiManager generated using the [Peak Finder](../image/PeakFinder). The output is a second set of PointRois added to the RoiManager. Usually the names are UID numbers and the command will output UID_SHORT and UID_LONG PointRois in the RoiManager. These can then be used as input for the [Molecule Integrator](../image/MoleculeIntegrator).

<img align='center' src='{{site.baseurl}}/docs/roi/img/TransformROIs.png' width='800' />

Several additional inputs allow for filtering PointRois in one channel based on the fluorescent intensity in the other channel. In the past this has been useful to filter for molecules bound to DNA by detecting fluorescence in the green channel (Short Wavelength Channel) and filtering the Original Rois by this detected fluorescence.

#### Inputs

* *Image* - The selected image will by analyzed using the peaks in the RoiManger. This is a required input but doesn't show up in the dialog, it is just the open image you have have selected at the time you run the command. To see the transformation in progress and use the preview, check the Show All option in the RoiManager to see the starting PointRois to be transformed.
* *Affine2D parameters* - The matrix values for the Affine2D transform generated using the Descriptor-based registration plugin.
* *Transformation Direction* - Whether detected peaks are being transformed from Long Wavelength to Short Wavelength or visa vera. This determines the names added to the ROIs. If Long Wavelength to Short Wavelength then the original Rois will get the _LONG ending and the new ones the _SHORT ending. If the opposite direction is selected, then the labels will be reversed.
* *Colocalize* - If checked the transformed peak positions will be checked for peaks and filtered to remove transformations to locations that do not have peaks in the second channel.
* *Use Discoidal Averaging Filter* -  If checked the imaging will be processes with a discoidal averaging filter using the Inner radius and Outer radius prior to peak finding. If unchecked the raw image will be used the the Inner and Outer radius settings will be ignored. See [[DiscoidalAveragingFilter]] for further information.
* *Inner Radius* - The inner radius for the Discoidal averaging filter.
* *Outer Radius* - The outer radius for the Discoidal averaging filter.
* *Detection threshold (mean + N * STD)* - This is how many standard deviations above the mean the peak detection threshold should we set at. So the input here is N in the expression. mean is the mean value of pixels in the image and STD is the standard deviation in the pixel values. Usually, between 2 and 4 is good for low signals and 5-8 is good for higher signals. This should be determined using the Preview button that will show the detected peaks for the given settings.
* *Filter Original ROIs* - If checked the peaks detected at the transformed locations will be used to filter the original ROIs in the starting channel. This is helpful to remove all proteins that do not colocalize with DNA for example. The effect will not be shown in the preview, it will only be show in the resulting output ROIs.
* *Colocalize search radius* - The radius of pixels to include for peak detection in the transformed peak. If the alignment is not perfect or the signal is more distributed in the transformed position, this setting will ensure there are fewer false negatives.
* *Preview* - If checked the resulting transformed peaks will show up on the image based on the settings. *Note: The results of Filtering the original ROIs will not show up in preview but only in the final output.*  

#### Output

* *PointROIs - RoiManger* - This command outputs transformed PointRois as well as the original PointRois used as input but with _SHORT and _LONG names added to the RoiManager.

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@ ImageJ ij
#@ RoiManager roiManager

import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.RoiTools.*

//Make an instance of the Command you want to run...
final TransformROIsCommand transformROIs = new TransformROIsCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
transformROIs.setContext(ij.getContext())

transformROIs.setROIManager(roiManager)
transformROIs.setImage(image)
transformROIs.setM00(0)
transformROIs.setM01(0)
transformROIs.setM02(0)
transformROIs.setM10(0)
transformROIs.setM11(0)
transformROIs.setM12(0)
transformROIs.setTransformationDirection("Long Wavelength to Short Wavelength")
transformROIs.setColocalize(false)
transformROIs.setUseDiscoidalAveragingFilter(false)
transformROIs.setInnerRadius(1)
transformROIs.setOuterRadius(5)
transformROIs.setDetectionThreshold(5)
transformROIs.setFilterOriginalRois(false)
transformROIs.setColocalizeSearchRadius(1)

//Run the Command
transformROIs.run()

//The output is just renaming and new ROIs in the RoiManager
//I suppose future versions could have a
//table input and output if needed
```
