---
layout: roi
title: Transform ROIs
permalink: /docs/roi/TransformROIs/index.html
---
This command is used to transform detected peaks from one channel of a dualview to another. The expected input is a table with Affine2D transformation parameters generated using the [Descriptor-based registration plugin](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/) and a set of PointROIs in the ROIManager generated using the [Peak Finder](../image/PeakFinder). The output is a second set of PointROIs added to the ROIManager. Usually the names are UID numbers and the command will output UID_SHORT and UID_LONG PointROIs in the ROIManager. These can then be used as input for the [Molecule Integrator](../image/MoleculeIntegrator).

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/roi/img/TransformROIs.png' width='800'/></div>

Several additional inputs allow for filtering PointROIs in one channel based on the fluorescent intensity in the other channel. In the past this has been useful to filter for molecules bound to DNA by detecting fluorescence in the green channel (Short Wavelength Channel) and filtering the Original ROIs by this detected fluorescence.

#### Input
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/roi/img/img46.png' width='350'/></div>

* *Image* - The selected image will by analyzed using the peaks in the ROIManger. The name of the image is displayed below the image stack in the dialog window.
* *Affine2D Transformation Matrix* - The matrix values for the Affine2D transform generated using the Descriptor-based registration plugin.
* *m00* - m00 matrix value.
* *m01* - m01 matrix value.
* *m02* - m02 matrix value.
* *m10* - m10 matrix value.
* *m11* - m11 matrix value.
* *m12* - m12 matrix value.

#### Colocalize
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/roi/img/img47.png' width='350'/></div>

* *Colocalize* - If checked the transformed peak positions will be checked for peaks and filtered to remove transformations to locations that do not have peaks in the second channel.
* *Channel* - Select the channel that needs to be transformed in the case of a multi-channel video.
* *DoG filter* - If checked the image will be processed with a Difference of Gaussian (DoG) filter before peak finding. Using an appropriately chosen radius this filter enhances real peaks with signal spread among several pixels and suppresses salt and pepper noise as demonstrated in [this systematic study](../DoGFilterProperties). If unchecked the raw image will be used for peak finding.
* *DoG radius* - The radius used for DoG filtering. The value chosen should reflect the size of the desired peaks. Decimal numbers are permitted.
* *Threshold* - This is threshold in pixel value used for peak detection. Pixels with values above this threshold are considered peaks and pixels below this threshold are considered background. For everyday peak detection, the DoG filter should be used in which case this threshold using on the DoG filtered image. In this case, we have found that values between 40 and 60 provide [optimal detection](../DoGFilterProperties) for typical single-molecule observations. If the DoG Filter is turned off this threshold reflects raw pixel values in the input image.
* *Search radius* - The radius of pixels to include for peak detection in the transformed peak. If the alignment is not perfect or the signal is more distributed in the transformed position, this setting will ensure there are fewer false negatives.
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/DoGFilterRadiiExample.png' width='600'/></div>
* *Filter Original ROIs* - If checked the peaks detected at the transformed locations will be used to filter the original ROIs in the starting channel. This is helpful to remove all proteins that do not colocalize with DNA for example. The effect will not be shown in the preview, it will only be show in the resulting output ROIs.
* *Remove colocalizing ROIs* - Remove ROIs in case the positions colocalize. This can be of use to filter out all peaks observed in both parts of the dualview resulting in a set of peaks that do not overlap and hence only have one of the two fluorophores. *Note:* This feature can only be enabled when 'Filter Original ROIs' is enabled.

#### Output
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/roi/img/img48.png' width='350'/></div>

* *Transformation Direction* - Whether detected peaks are being transformed from Long Wavelength to Short Wavelength or visa vera. This determines the names added to the ROIs. If Long Wavelength to Short Wavelength then the original Rois will get the _LONG ending and the new ones the _SHORT ending. If the opposite direction is selected, then the labels will be reversed.

#### Preview
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/roi/img/img49.png' width='350'/></div>

* *Preview* - If checked the resulting transformed peaks will show up on the image based on the settings. *Note: The results of Filtering the original ROIs will not show up in preview but only in the final output.*  
* *T* - Select the frame (T) in case of a multi-frame video.
* *Preview timeout (s)* - Maximum time that is allowed to pass before an (unsuccessful) preview attempt will be terminated.



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
transformROIs.setChannel(0)
transformROIs.setT(0)
transformROIs.setUseDogFilter(false)
transformROIs.setDogFilterRadius(2)
transformROIs.setTreshold(50)
transformROIs.setFilterOriginalRois(false)
transformROIs.setFilterColocalizingRois(true)
transformROIs.setColocalizeSearchRadius(1)

//Run the Command
transformROIs.run()

//The output is just renaming and new ROIs in the RoiManager
//I suppose future versions could have a
//table input and output if needed
```
