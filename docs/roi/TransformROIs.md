---
layout: roi
title: Transform ROIs
permalink: /docs/roi/TransformROIs/index.html
---
This command is used to transform detected peaks from one channel of a dualview to another. The expected input is a table with Affine2D transformation parameters generated using the [Descriptor-based registration plugin](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/) and a set of PointROIs in the ROIManager generated using the [Peak Finder](../image/PeakFinder). The output is a second set of PointROIs added to the ROIManager. Usually the names are UID numbers and the command will output UID_SHORT and UID_LONG PointROIs in the ROIManager. These can then be used as input for the [Molecule Integrator](../image/MoleculeIntegrator).

<img align='center' src='{{site.baseurl}}/docs/roi/img/TransformROIs.png' width='800' />

Several additional inputs allow for filtering PointROIs in one channel based on the fluorescent intensity in the other channel. In the past this has been useful to filter for molecules bound to DNA by detecting fluorescence in the green channel (Short Wavelength Channel) and filtering the Original ROIs by this detected fluorescence.

#### Inputs

<img align='center' src='{{site.baseurl}}/docs/roi/img/img1.png' width='450' />

* *Image* - The selected image will by analyzed using the peaks in the ROIManger. This is a required input but doesn't show up in the dialog, it is just the open image you have have selected at the time you run the command. To see the transformation in progress and use the preview, check the Show All option in the ROIManager to see the starting PointROIs to be transformed.
* *Affine2D parameters* - The matrix values for the Affine2D transform generated using the Descriptor-based registration plugin.
* *Transformation Direction* - Whether detected peaks are being transformed from Long Wavelength to Short Wavelength or visa vera. This determines the names added to the ROIs. If Long Wavelength to Short Wavelength then the original Rois will get the _LONG ending and the new ones the _SHORT ending. If the opposite direction is selected, then the labels will be reversed.
* *Colocalize* - If checked the transformed peak positions will be checked for peaks and filtered to remove transformations to locations that do not have peaks in the second channel.
* *Use DoG filter* - If checked the image will be processed with a Difference of Gaussian (DoG) filter before peak finding. Using an appropriately chosen radius this filter enhances real peaks with signal spread among several pixels and suppresses salt and pepper noise as demonstrated in [this systematic study](../DoGFilterProperties). If unchecked the raw image will be used for peak finding.
* *DoG filter radius* - The radius used for DoG filtering. The value chosen should reflect the size of the desired peaks. Decimal numbers are permitted.

<img align='center' src='{{site.baseurl}}/docs/image/img/DoGFilterRadiiExample.png' width='600' />

* *Detection threshold* - This is threshold in pixel value used for peak detection. Pixels with values above this threshold are considered peaks and pixels below this threshold are considered background. For everyday peak detection, the DoG filter should be used in which case this threshold using on the DoG filtered image. In this case, we have found that values between 40 and 60 provide [optimal detection](../DoGFilterProperties) for typical single-molecule observations. If the DoG Filter is turned off this threshold reflects raw pixel values in the input image.
* *Minimum distance between peaks* - This is the minimum allowed distance between peaks. This means only the pixel with the highest intensity within this radius will be accepted as a peak, even if there are other pixels above the threshold within this radius region. This is an important setting since most peaks have nearby pixels that are also above the detection threshold, but we only want to detect each peak once. See the image below for an example.

<img align='center' src='{{site.baseurl}}/docs/image/img/Minimum Distance.png' width='500' />

* *Filter Original ROIs* - If checked the peaks detected at the transformed locations will be used to filter the original ROIs in the starting channel. This is helpful to remove all proteins that do not colocalize with DNA for example. The effect will not be shown in the preview, it will only be show in the resulting output ROIs.
* *Colocalize search radius* - The radius of pixels to include for peak detection in the transformed peak. If the alignment is not perfect or the signal is more distributed in the transformed position, this setting will ensure there are fewer false negatives.
* *Preview* - If checked the resulting transformed peaks will show up on the image based on the settings. *Note: The results of Filtering the original ROIs will not show up in preview but only in the final output.*  

#### Output

* *PointROIs - ROIManger* - This command outputs transformed PointROIs as well as the original PointROIs used as input but with _SHORT and _LONG names added to the ROIManager.

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
transformROIs.setUseDogFilter(false)
transformROIs.setDogFilterRadius(2)
transformROIs.setTreshold(50)
transformROIs.setMinimumDistance(4)
transformROIs.setFilterOriginalRois(false)
transformROIs.setColocalizeSearchRadius(1)

//Run the Command
transformROIs.run()

//The output is just renaming and new ROIs in the RoiManager
//I suppose future versions could have a
//table input and output if needed
```
