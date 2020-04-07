---
layout: image
title: Peak Finder
permalink: /docs/image/PeakFinder/index.html
---
This command is used to find higher intensity spots or peaks in images. Typically these are single polystyrene beads or single fluorescent dyes or groups of dyes. After locating the peaks, this command can also perform 2D Gaussian fitting to determine the sub pixel position of the peaks. There are a lot of possible input options to allow for maximum customizability so the same tool can easily be used with multiple image and peak types.

The detection threshold determines the cutoff value in number of standard deviations above the mean for which peaks are not detected. So the initial processing is just to find the mean and standard deviation for all pixels and then make a list of those above the detection threshold. The preview button is very helpful in determining the correct threshold, since it directly shows the detected peaks in real-time before running the command. Obviously, for very bright peaks, there will be multiple nearby pixels above the threshold, so you have to specify the minimum allowed distance between peaks. This will ensure only the local maximum is selected as a peaks and you don't find the same peaks twice.

This usually works well, but sometimes you have very bright blobs in your image with lots of pixels above the threshold. Usually these are not the peaks you want to detect. The Discoidal Averaging Filter was designed to remove these in a pre-filter step prior to Peak Finding. With the correct settings, this tool can also increase the signal for weak peaks improving their detection. There is a separate command that will only apply the [[Discoidal Averaging Filter]] and generate the resulting image. Please refer to this page or below for further explanation. If you are unsure what settings to use, you can also apply the [[Discoidal Averaging Filter]] command and then run the Peak Finder on that image (with Use Discoidal Averaging Filter unchecked, since you already did it manually). This gets around the fact that you can't directly see the results of the filter in the peak finder. Instead in this command it is applied during processing and the resulting detect peaks are reported.

Once the pixel positions of the peaks have been determined you can directly output those locations or fit the peaks to get the sub pixel positions. There are numerous settings for fitting, from the initial guesses for each parameter, to which parameters to vary, and above which error margins to reject peaks. Using the Verbose table fit output you can get all the information from each fit. To save space leave this unchecked and only get x and y. At the moment, only symmetric Gaussian fitting is performed - only circular peaks are fit - in which the x and y sigma are the same. It is pretty easy to fit asymmetric peaks if needed with a groovy script, and this is implemented in the PeakFinder class code but commented out at the moment.

#### Inputs

<img align='center' src='{{site.baseurl}}/docs/image/img/PeakFinder.png' width='450' />

 * *Image* - The active image selected will be used by the Peak Finder. So this is a required input but doesn't show up in the dialog.
 * *use ROI* - If checked a subregion of the image will be used for processing. Otherwise, the entire image will be used. You can also add a selection with the box tool, so making a rectangular ROI to the image. This Roi will activate this box and add the settings below it.
 * *ROI x0* - Upper left corner x0 position of ROI.
 * *ROI y0* - Upper right corner y0 position of ROI.
 * *ROI width* - width of the ROI.
 * *ROI height* - height of the ROI.
 * *Use Discoidal Averaging Filter* - If checked the imaging will be processes with a discoidal averaging filter using the Inner radius and Outer radius prior to peak finding. If unchecked the raw image will be used the the Inner and Outer radius settings will be ignored.
 * *Inner Radius* - Inner Radius beyond a single pixel used for discoidal processing. This should be enough to include your peak.
 * *Outer Radius* - Outer Radius that determine the radius of the ring used for calculating the background for discoidal averaging. This need to be large enough so peak signal is excluded.
 <img align='center' src='{{site.baseurl}}/docs/image/img/DS filter.png' width='600' />
 * *Detection threshold* - This is how many standard deviations above the mean the peak detection threshold should we set as. So the input here is N in the expression. mean is the mean value of pixels in the image and STD is the standard deviation in the pixel values. Usually, between 2 and 4 is good for low signals and 5-8 is good for higher signals. This should be determined using the Preview button that will show the detected peaks for the given settings.
 * *Minimum distance between peaks (in pixels)* - This is the minimum allowed distance between peaks, This means only the pixel with the highest intensity within this radius will be accepted as a peak, even if there are other peaks above the threshold within this radius region. This is an important setting since most peaks have nearby pixels that are also above the detection threshold, but we only want to detect each peak once. See the image below for an example.
 <img align='center' src='{{site.baseurl}}/docs/image/img/Minimum Distance.png' width='500' />
 * *Find Negative Peaks* - If checked this inverts the image for the purposes of peak detection, so that only peaks with negative values are detected. This is used in gradient images, where a negative gradient can results in a negative peak. This is useful for detecting the edges of long DNA molecules.
 * *Generate peak count table* - If checked a table will appear in which each row has the number of detected peaks for each slice. This is useful to look at bead loss as a function of time in the bead assays and bleaching for a given intensity in smTIRF data.
 * *Generate peak table* - If checked a table will be generated listing the positions of all detected peaks.
 * *Add to RoiManager* - If checked detected peaks will be added to the RoiManager. This is useful for further processing such as transforming peaks between dual view regions and integrating peaks with the Molecule Integrator.
 * *Process all slices* - If checked all frames in the video will be analyzed. If unchecked only the current frame is analyzed.
 * *Preview* - When checked the crosshairs will appear for all detected peaks given the current settings. This is very useful for finding the correct settings that detect just enough peaks without too much background. This will live update as parameters are changed, such as the detection threshold. This is used before running the command to confirm you have the correct settings. This only detects peaks at the pixel level and doesn't do any fitting, So all settings below the Preview checkbox are ignored for the purposes of preview. Fitting can result in the rejection of some peaks see in the preview depending on the error settings.

The remaining settings are only important if you want to fit the peaks to determine their subpixel positions.
 * *Fit peaks* - If checked all peaks will be fit with 2D Gaussians to determine the sub pixel position. If left unchecked, all the remaining settings will be ignored and the peaks will be reported with their integer pixel positions. Fitting is done with the following 2D Gaussian equation:
 <img align='center' src='{{site.baseurl}}/docs/image/img/2D Gaussian.png' width='500' />
Where x0 and y0 are the subpixel positions of the peak, Height, Baseline and Sigma match the settings below and f(x,y) gives the intensity as a function of pixel position for a given set of parameters.
 * *Fit Radius* - The radius of pixels used for fitting. 0 is one pixel, 1 is 9 pixels, 2 is 25 pixels. Usually 2 is a pretty good estimate depending on the peak size. There needs to be some pixels at the edges close to background for an ideal fit.

The following are the max allowed error in each parameter. If the error from fitting is larger than these estimates, then the fit will be rejected and the peak will be thrown-out. For optimal filtering these parameters should be empirically optimized by looking at the Verbose table fit output with Filter by Max Error checked. Then check Filter by Max Error with acceptable settings so only good fits are included.
 * *Filter by Max Error* - If checked all peaks fits will be checked again the Max Error thresholds below. If the error is beyond the threshold then the peak will be rejected and not included in the output. If unchecked all peak fit will be reported even very bad fits that might contain NaN values. If you have to use a low threshold to detect your peaks and you pick up some background (you should be able to see this using the preview button)

#### Output

Several different kinds of output are possible depending on the settings used.
   * *Generate peak count table* - If checked a table will appear in which each row has the number of detected peaks for each slice.
   * *Generate peak table* - If checked a table will be generated listing the positions of all detected peaks.
   * *Add to RoiManager* - If checked detected peaks will be added to the RoiManager.

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@OUTPUT MarsTable peakCountTable
#@ ImageJ ij

import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.ImageProcessing.commands.*

//Make an instance of the Command you want to run...
final PeakFinderCommand peakFinder = new PeakFinderCommand()

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
peakFinder.setContext(ij.getContext())

peakFinder.setImage(image)
peakFinder.setUseROI(false)
peakFinder.setX0(0)
peakFinder.setY0(0)
peakFinder.setWidth(1024)
peakFinder.setHeight(1024)
peakFinder.setUseDogFiler(true)
peakFinder.setDogFilterRadius(1.8d)
peakFinder.setThreshold(50)
peakFinder.setMinimumDistance(4)
peakFinder.setFindNegativePeaks(false)
peakFinder.setGeneratePeakCountTable(true)
peakFinder.setGeneratePeakTable(false)
peakFinder.setAddToRoiManager(false)
peakFinder.setProcessAllSlices(true)
peakFinder.setFitPeaks(true)
peakFinder.setFitRadius(4)
peakFinder.setMinimumRsquared(0.01d)
peakFinder.setVerboseFitOutput(true)

//Run the Command
peakFinder.run();

//Retrieve output from the command
peakCountTable = peakFinder.getPeakCountTable()

//peakFinder.getPeakTable()

//MarsTable peakCountTable = peakFinder.getPeakCountTable()

//The other possible output is adding the peaks to the RoiManager
//in which the RoiManager can then be use for further steps.
//However, keep in mind the RoiManager will probably not work
//in headless mode.
```
