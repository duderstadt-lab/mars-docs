---
layout: Image
title: Peak Tracker
permalink: /docs/image/PeakTracker/index.html
---
This command is used to find, fit and track peaks in images. Typically these are polystyrene beads or single fluorescent dyes or groups of dyes attached to biomolecules. This command is nearly the same as the [[PeakFinder]] command but with the addition of tracking and fewer output options. So a lot of the input documentation is the same between the two commands.

To determine the optimal setting for peak finder, first the [[PeakFinder]] command can be used with the Preview button. Once the best settings have been determined - that find all peaks with minimal background - the settings can be used here. This will generate the best tracks because the accuracy of tracking depends entirely on the quality of the peak fits in individual frames.

This command outputs a MoleculeArchive in which individual molecule records have DataTables with the x and y positions and slice. If verbose is checked, then all fit parameters are add to the DataTable. The MoleculeArchive will also contain metadata information from the image that can be used to convert from slice to Time and do a number of other useful operations provided by the other commands in the MoleculeArchive submenu.

The [[ColorCodedTrackOverlay]] groovy script provides and excellent method for overlapping the tracking results with UIDs onto the image using the MoleculeArchive records. By running the movie forward frame by frame it is possible to see when tracking goes wrong and how to further optimise the tracking settings.

*Note* - Currently the status bar only runs during the peak finding and fitting steps. But tracking can also take some time. So be patient and your archive will appear. You can always check the console to see if a problem has occurred under Window->Console. Here you will also find the time it took for each step of the process and all the settings. The setting will also be placed in the log of the ImageMetaData record created and placed into the outputted MoleculeArchive.

#### Inputs

OMG that is a lot of inputs, but don't panic, they are all pretty simple to understand and overlap with the [[PeakFinder]] options. A lot of inputs are included for completeness to give fine control but are not needed in most normal cases.

<img align='center' src='{{site.baseurl}}/docs/ImageProcessing/img/Peak Tracker.png' width='450' />

* *Image* - The active image selected will be used by the Peak Tracker. So this is a required input but doesn't show up in the dialog.
* *use ROI* - If checked a subregion of the image will be used for processing. Otherwise, the entire image will be used. You can also add a selection with the box tool to add a rectangular ROI to the image. Upon running the command, this ROI will activate the checkbox and add the region settings.
* *ROI x0* - Upper left corner x0 position of ROI.
* *ROI y0* - Upper right corner y0 position of ROI.
* *ROI width* - width of the ROI.
* *ROI height* - height of the ROI.
* *Use Discoidal Averaging Filter* - If checked the image will be processed with a discoidal averaging filter using the Inner radius and Outer radius prior to peak finding. If unchecked the raw image will be used and the Inner and Outer radius settings will be ignored.
* *Inner Radius* - Inner Radius beyond a single pixel used for discoidal processing. This should be enough to include your peak.
* *Outer Radius* - Outer Radius determines the size of the ring used for calculating the background for discoidal averaging. This needs to be large enough so peak signal is exlcuded.

<img align='center' src='{{site.baseurl}}/docs/ImageProcessing/img/DS filter.png' width='600' />

* *Detection threshold (mean + N * STD)* - This is how many standard deviations above the mean the peak detection threshold should be set at. So the input here is N in the expression. mean is the mean value of pixels in the image and STD is the standard deviation in the pixel values. Usually, between 2 and 4 is good for low signals and 5-8 is good for higher signals.
* *Minimum distance between peaks (in pixels)* - This is the minimum allowed distance between peaks, This means only the pixel with the highest intensity within this radius will be accepted as a peak, even if there are other peaks above the threshold within this radius region. This is an important setting since most peaks have nearby pixels that are also above the detection threshold, but we only want to detect each peak once. See the image below for an example.

<img align='center' src='{{site.baseurl}}/docs/ImageProcessing/img/Minimum Distance.png' width='500' />

* *Find Negative Peaks* - If checked this inverts the image for the purposes of peak detection, so that only peaks with negative values are detected. This is used in gradient images, where a negative gradient can results in a negative peak. This is useful for detecting the edges of long DNA molecules.
* *Fit peaks* - If checked all peaks will be fit with 2D Gaussians to determine the sub pixel position. If left unchecked, all the remaining settings will be ignored and the peaks will be reported with their integer pixel positions. Fitting is done with the following 2D Gaussian equation:

<img align='center' src='{{site.baseurl}}/docs/ImageProcessing/img/2D Gaussian.png' width='500' />

Where x0 and y0 are the subpixel positions of the peak, Height, Baseline and Sigma match the settings below and f(x,y) gives the intensity as a function of pixel position for a given set of parameters.
* *Fit Radius* - The radius of pixels used for fitting. 0 is one pixel, 1 is 9 pixels, 2 is 25 pixels. Usually 2 is a pretty good estimate depending on the peak size. There needs to be some pixels at the edges close to background for an ideal fit.
* *Initial Baseline* - The initial guess for the mean background pixel value.
* *Initial Height* - The initial guess for the difference between the mean background pixel at the value of the pixel at the top of the peak.
* *Initial Sigma* - The initial guess for the sigma of the 2D Gaussian. Approx. 68% of the peak intensity should lie within one sigma (within one standard deviation from the center).
* *Vary Baseline* - If checked the baseline will be refined to obtain the best fit. If left unchecked the initial baseline will be used as a constant during fitting.
* *Vary Height* - If checked the height will be refined to obtain the best fit. If left unchecked the initial height will be used as a constant during fitting.
* *Vary Sigma* - If checked the sigma will be refined to obtain the best fit. If left unchecked the initial sigma will be used as a constant during fitting.

The following are the max allowed error in each parameter. If the error from fitting is larger than these estimates, then the fit will be rejected and the peak will be thrown-out. For optimal filtering these parameters should be empirically optimized by looking at the Verbose table fit output with Filter by Max Error checked. Then check Filter by Max Error with acceptable settings so only good fits are included.
* *Filter by Max Error* - If checked all peaks fits will be checked again the Max Error thresholds below. If the error is beyond the threshold then the peak will be rejected and not included in the output. If unchecked all peak fit will be reported even very bad fits that might contain NaN values. If you have to use a low threshold to detect your peaks and you pick up some background (you should be able to see this using the preview button)
* *Max Error Baseline* - The maximum allowed error in the baseline during fitting.
* *Max Error Height* - The maximum allowed error in the height during fitting.
* *Max Error X* - The maximum allowed error in the x coordinate during fitting.
* *Max Error Y* - The maximum allowed error in the y coordinate during fitting.
* *Max Error Sigma* - The maximum allowed error in Sigma during fitting.
* *Verbose table fit output* - If checked all columns from fitting will be included in the table generated (baseline, error_baseline, height, error_height, x, error_x, y, error_y, sigma, error_sigma). If unchecked only the position columns will be included in the table generated (x, y). Very useful for optimising the parameters, or for other applications using various peak properties.
The following are the setting for tracking absent in the [[PeakFinder]] command. Once all peaks have been found and fit, the program still doesn't know which peaks correspond to which molecules between slices. To connect all the peaks, the Peak Tracker does a nearest-neighbor search between frames. So the closest peak in the next frame to the current peak position is considered the same molecule.

<img align='center' src='{{site.baseurl}}/docs/ImageProcessing/img/Tracking Peak Stack.png' width='400' />

* *Check Max Difference Baseline* - If checked the Max Difference Baseline setting will be used as a threshold to determine if the baseline difference for two peaks is too large for them to be linked. If left unchecked this setting is ignored. (Maybe this shouldn't set an included setting since usually the baseline is always the same and is just the image background level...)
* *Max Difference Baseline* - The threshold baseline difference between peaks below which they are still allowed to be linked.
* *Check Max Difference Height* - If checked the Max Difference Height setting will be used as a threshold to determine if the height difference for two peaks is too large for them to be linked. If left unchecked this setting is ignored. This can exclude linking of a very bright and very dim peak. For example, a single bead and a clump, or a single dye vs a cluster.
* *Max Difference Height* - The threshold height difference between peaks below which they are still allowed to be linked. This is probably best determine using the [[PeakFinder]] in verbose mode and then looking at the differences between dim and bright peaks.
The following settings define the boundaries of nearest neighbors that are allowed to be linked. These are very useful at ensuring only real links are made. Also, if you know your molecule only moves along once dimension you can restrict the other to further prevent incorrect links from occurring.
* *Max Difference X* - The threshold difference between the X position of two peaks below which they are still allowed to be linked.
* *Max Difference Y* - The threshold difference between the Y position of two peaks below which they are still allowed to be linked.
* *Check Max Difference Sigma* - If checked the Max Difference Sigma setting will be used as a threshold to determine if the Sigma difference for two peaks is too large for them to be linked. If left unchecked this setting is ignored. Again this can be useful in preventing links between bright and dim molecules, since the bright molecules typically have a larger sigma or width.
* *Max Difference Sigma* - The threshold difference between the Sigma values of two peaks below which they are still allowed to be linked.

<img align='center' src='{{site.baseurl}}/docs/ImageProcessing/img/Peak Difference Settings.png' width='400' />

* *Max Difference Slice* - The number of frames into the future to search for a possible link. This parameter is very important to ensure tracking is robust against single frames where peaks are not detected. Blinking events could cause a molecule to go dark for a couple frames. This setting ensures the track is still linked once the molecule is bright again. Alternatively, a molecule could go out of focus, again causing it to go below the detection threshold. If this setting is larger than the number of frames during which the peak wasn't detected, then the track will be picked up when the molecule is detected again. If this value is too large, it will take a long time to track and bad links will start to be formed. Usually 3-5 is a good range depending on your dataset.
* *Min trajectory length* - All tracks with fewer than this number of slices will be rejected and removed from the MoleculeArchive. There are always a lot of very show tracks resulting from a molecule appearing for only a couple frames then going away, or if the peak detection threshold is very low, nearby false detections in consecutive frames can be linked to make short tracks. This removes all these events. For bead data this is almost always in the 50-200 or more range. However, it really depends on your experiment how this should be set, so you don't exclude good events.
* *Microscope* - The name of the microscope used to collect this data. This name is included in the ImageMetaData record.
* *Format* - This is the software used to collect the Image data. Based on this setting, the command will attempt to parse all the ImageMetaData information included in each tif frame header. This information will then be included in the DataTable and properties of the ImageMetaData. This information is very useful to automatically convert slice to time using the raw frame timestamp information and detecting the lasers and filters used on the fluorescence microscopes.

### Output

* *MoleculeArchive* - A MoleculeArchive with individual records for all tracked molecules as well as ImageMetaData information.

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*
import de.mpg.biochem.mars.ImageProcessing.*

//Make an instance of the Command you want to run...
final FinderFitterTrackerCommand peakTracker = new FinderFitterTrackerCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
peakTracker.setContext(ij.getContext())

peakTracker.setImage(image)
peakTracker.setUseROI(false)
peakTracker.setX0(0)
peakTracker.setY0(0)
peakTracker.setWidth(100)
peakTracker.setHeight(100)
peakTracker.setUseDiscoidalAveragingFilter(true)
peakTracker.setInnerRadius(1)
peakTracker.setOuterRadius(3)
peakTracker.setThreshold(5)
peakTracker.setMinimumDistance(4)
peakTracker.setFindNegativePeaks(false)
peakTracker.setFitRadius(2)
peakTracker.setInitialBaseline(40)
peakTracker.setInitialHeight(150)
peakTracker.setInitialSigma(1)
peakTracker.setVaryBaseline(true)
peakTracker.setVaryHeight(true)
peakTracker.setVarySigma(true)
peakTracker.setMaxErrorFilter(true)
peakTracker.setMaxErrorBaseline(20)
peakTracker.setMaxErrorHeight(30)
peakTracker.setMaxErrorX(0.2)
peakTracker.setMaxErrorY(0.2)
peakTracker.setMaxErrorSigma(0.2)
peakTracker.setVerboseFitOutput(false)
peakTracker.setCheckMaxDifferenceBaseline(false)    
peakTracker.setMaxDifferenceBaseline(500)
peakTracker.setCheckMaxDifferenceHeight(false)
peakTracker.setMaxDifferenceHeight(500)
peakTracker.setMaxDifferenceX(5)
peakTracker.setMaxDifferenceY(5)
peakTracker.setCheckMaxDifferenceSigma(false)
peakTracker.setMaxDifferenceSigma(1)
peakTracker.setMaxDifferenceSlice(5)
peakTracker.setMinTrajectoryLength(50)
peakTracker.setMicroscope("Odin")
peakTracker.setImageFormat("NorPix")

//Run the Command
peakTracker.run();

//Retrieve output from the command
MoleculeArchive archive = peakTracker.getArchive()

//If run in Fiji the archive will appear,
//but you can continue to use it for further processing
//Using the retrieval command above.
```
