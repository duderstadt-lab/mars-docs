---
layout: image
title: Peak Tracker
permalink: /docs/image/PeakTracker/index.html
---
This command is used to find, fit and track peaks in images. Typically these are polystyrene beads or single fluorescent dyes or groups of dyes attached to biomolecules. This command is nearly the same as the [PeakFinder](../PeakFinder) command but with the addition of tracking and fewer output options. So a lot of the input documentation is the same between the two commands.

To determine the optimal setting for peak finder, first the [PeakFinder](../PeakFinder) command can be used with the Preview button. Once the best settings have been determined - that find all peaks with minimal background - the settings can be used here. This will generate the best tracks because the accuracy of tracking depends entirely on the quality of the peak fits in individual frames.

This command outputs a MoleculeArchive in which individual molecule records have DataTables with the x and y positions and slice. If verbose is checked, then all fit parameters are add to the DataTable. The MoleculeArchive will also contain metadata information from the image that can be used to convert from slice to Time and do a number of other useful operations provided by the other commands in the MoleculeArchive submenu.

The Color coded track overlay groovy script provides and excellent method for overlapping the tracking results with UIDs onto the image using the MoleculeArchive records. By running the movie forward frame by frame it is possible to see when tracking goes wrong and how to further optimise the tracking settings.

*Note* - Currently the status bar only runs during the peak finding and fitting steps. But tracking can also take some time. So be patient and your archive will appear. You can always check the console to see if a problem has occurred under Window->Console. Here you will also find the time it took for each step of the process and all the settings. The setting will also be placed in the log of the metadata record created and placed into the MoleculeArchive created.

#### Inputs

There are a lot of inputs, but don't panic, they are all pretty simple to understand and overlap with the [PeakFinder](../PeakFinder) options. A lot of inputs are included for completeness to give fine control but do not need to be changed in most cases.

<img align='center' src='{{site.baseurl}}/docs/image/img/Peak Tracker.png' width='550' />

* *Image* - The active image selected will be used by the Peak Tracker. So this is a required input but doesn't show up in the dialog.
* *use ROI* - If checked a subregion of the image will be used for processing. Otherwise, the entire image will be used. You can also add a selection with the box tool to add a rectangular ROI to the image. Upon running the command, this ROI will activate the checkbox and add the region settings.
* *ROI x0* - Upper left corner x0 position of ROI.
* *ROI y0* - Upper right corner y0 position of ROI.
* *ROI width* - width of the ROI.
* *ROI height* - height of the ROI.
* *Use DoG filter* - If checked the image will be processed with a Difference of Gaussian (DoG) filter before peak finding. Using an appropriately chosen radius this filter enhances real peaks with signal spread among several pixels and suppresses salt and pepper noise as demonstrated in [this systematic study](../DoGFilterProperties). If unchecked the raw image will be used for peak finding.
* *DoG filter radius* - The radius used for DoG filtering. The value chosen should reflect the size of the desired peaks. Decimal numbers are permitted.

<img align='center' src='{{site.baseurl}}/docs/image/img/DoGFilterRadiiExample.png' width='600' />

* *Detection threshold* - This is threshold in pixel value used for peak detection. Pixels with values above this threshold are considered peaks and pixels below this threshold are considered background. For everyday peak detection, the DoG filter should be used in which case this threshold using on the DoG filtered image. In this case, we have found that values between 40 and 60 provide [optimal detection](../DoGFilterProperties) for typical single-molecule observations. If the DoG Filter is turned off this threshold reflects raw pixel values in the input image.
* *Minimum distance between peaks* - This is the minimum allowed distance between peaks. This means only the pixel with the highest intensity within this radius will be accepted as a peak, even if there are other pixels above the threshold within this radius region. This is an important setting since most peaks have nearby pixels that are also above the detection threshold, but we only want to detect each peak once. See the image below for an example.

<img align='center' src='{{site.baseurl}}/docs/image/img/Minimum Distance.png' width='500' />

* *Find Negative Peaks* - If checked peak with negative pixel values are detected. This can be used in gradient images, where a negative gradient results in a negative peak. This is useful for detecting the edges of long DNA molecules and other structures.

* *Fit Radius* - The radius of pixels used for peak fitting. 0 is one pixel, 1 is 9 pixels, 2 is 25 pixels. Usually 3 or 4 is a good estimate depending for typical single-molecule observations. Optimal fitting is done using a region with background pixels at the edges.
* *Minimum R-squared* - This is the minimum allowed value for peak fits. R-squared is defined as 1 - SSres / SStot to match the definition used in [Prism](
https://www.graphpad.com/guides/prism/7/curve-fitting/reg_intepretingnonlinr2.htm) as defined [here](https://en.wikipedia.org/wiki/Coefficient_of_determination) with 1.0 for perfect fits and 0.0 for fits no better than a straight line. Typical single-molecule images are noisy leading to large variation in R-squared values so this threshold should only be used as an additional step if further peak rejection is required. If set to 0, the R-square value will not be calculated. Values of 0.05 to 0.15 are reasonable for noisy images.
* *Verbose output* - If checked all fit parameters are included in the output (baseline, height, x, y, sigma, R2). If unchecked only the position, slice, intensity columns will be included in the output. Verbose output can be used to optimise the R-squared minimum, or for calculations based on peak properties.
The following are the settings for tracking absent in the [PeakFinder](../PeakFinder) command. Once all peaks have been found and fit, which peaks correspond to which molecules between slices is still unknown. To connect all the peaks, the Peak Tracker does a nearest-neighbor search between frames. The closest peak in the next frame to the current peak position is considered the same molecule.

<img align='center' src='{{site.baseurl}}/docs/image/img/Tracking Peak Stack.png' width='400' />

* *Max Difference X* - The threshold difference between the X position of two peaks below which they are still allowed to be linked.
* *Max Difference Y* - The threshold difference between the Y position of two peaks below which they are still allowed to be linked.
* *Max Difference Slice* - The number of frames into the future to search for a possible link. This parameter is very important to ensure tracking is robust against single frames where peaks are not detected. Blinking events could cause a molecule to go dark for a couple frames. This setting ensures the track is still linked once the molecule is bright again. Alternatively, a molecule could go out of focus, again causing it to go below the detection threshold. If this value is too large, it will take a long time to track and bad links will start to be formed. Usually 3 to 5 is a good range.
* *Minimum track length* - All tracks with fewer than this number of slices will be rejected and removed from the MoleculeArchive. Frequently single-molecule experiments have many short tracks resulting from molecules appearing for only a couple frames then going away. This removes all these events.
* *Microscope* - The name of the microscope used for data collection. This name is included in the metadata record.
* *Format* - This is the software used to collect the Image data. Based on this setting, the command will attempt to parse all the metadata included in each tif frame header. This information will then be included in the Data Table and properties of the metadata. This information is very useful to automatically convert slice to time using the raw frame timestamp information and detecting the lasers and filters used.

### Output

* *MoleculeArchive* - A MoleculeArchive with individual records for all tracked molecules as well as metadata information.

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*
import de.mpg.biochem.mars.ImageProcessing.commands.*

//Make an instance of the Command you want to run...
final PeakTrackerCommand peakTracker = new PeakTrackerCommand()

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
peakTracker.setContext(ij.getContext())

peakTracker.setImage(image)
peakTracker.setUseROI(false)
peakTracker.setX0(0)
peakTracker.setY0(0)
peakTracker.setWidth(100)
peakTracker.setHeight(100)
peakTracker.setUseDogFiler(true)
peakTracker.setDogFilterRadius(1.8d)
peakTracker.setThreshold(50)
peakTracker.setMinimumDistance(5)
peakTracker.setFindNegativePeaks(false)
peakTracker.setFitRadius(4)
peakTracker.setMinimumRsquared(0.0d)
peakTracker.setVerboseOutput(false)
peakTracker.setMaxDifferenceX(5)
peakTracker.setMaxDifferenceY(5)
peakTracker.setMaxDifferenceSlice(5)
peakTracker.setMinimumTrackLength(10)
peakTracker.setIntegrate(true)
peakTracker.setIntegrationInnerRadius(1)
peakTracker.setIntegrationOuterRadius(3)
peakTracker.setMicroscope("Microscope")
peakTracker.setImageFormat("MicroManager")

//Run the Command
peakTracker.run();

//Retrieve output from the command
MoleculeArchive archive = peakTracker.getArchive()

//If run in Fiji the archive will appear,
//but you can continue to use it for further processing
//Using the retrieval command above.
```
