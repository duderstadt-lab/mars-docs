---
layout: image
title: DNA finder
permalink: /docs/image/DNA_finder/index.html
---

This command is used to find vertically aligned DNA molecules in images. Typically these are 20-50 kb DNAs that are surface immobilized and fluorescently stained. After locating the ends of the DNA molecules, this command can also perform 2D Gaussian fitting to determine the location of each end with subpixel accuracy. There are a lot of possible input/output options to allow for customizability so the same tool can be used with multiple image and DNA types.

<img align='center' src='{{site.baseurl}}/docs/image/img/DNA Finder Example 1.png' width='450' />

#### Inputs

<img align='center' src='{{site.baseurl}}/docs/image/img/DNA Finder Dialog.png' width='550' />

* *Image* - The active image selected will be used by the DNA Finder. This is a required input but doesn't show up in the dialog.
* *use ROI* - If checked a subregion of the image will be used for processing. Otherwise, the entire image will be used. You can also add a selection with the box tool, by making a rectangular ROI to the image. This Roi will activate this box and add the settings below it.
* *ROI x0* - Upper left corner x0 position of ROI.
* *ROI y0* - Upper right corner y0 position of ROI.
* *ROI width* - width of the ROI.
* *ROI height* - height of the ROI.
* *Gaussian Filter Sigma* - The sigma used for gaussian smoothing. The DNA Finder uses the ImageJ op derivativeGauss which filters the image with a combined gaussian smoothing and takes the derivative. Usually a sigma of 1 is usually sufficient. Higher sigma values might lead to too much smoothing and obscure important features.

<img align='center' src='{{site.baseurl}}/docs/image/img/derivativeSeriesSmall.png' width='800' />

* *End detection threshold (mean + N * STD)* - This is how many standard deviations above the mean the end peak detection threshold should we set as. So the input here is N in the expression. mean is the mean value of pixels in the image and STD is the standard deviation of the pixel values. Usually, between 2 and 4 is good for low signals and 5-8 is good for higher signals. End detection is performed using the derivativeGauss filtered image (lower row above) and the threshold given is use for detecting both positive and negative ends.
* *Minimum distance between edges (in pixels)* - This is the minimum allowed distance between peaks (edges here), This means only the pixel with the highest (or lowest) intensity within this radius will be accepted as a peak, even if there are other peaks above the threshold within this radius region. This is an important setting since most peaks have nearby pixels that are also above the detection threshold, but we only want to detect each peak once. If you see a lot of overlapping DNAs increasing this setting will help.
* *Optimal DNA length (in pixels)* - The mean length in pixels of the DNA you want to find. This value is used when attempting to link positive peaks to their negative counterparts to find DNAs.  
* *DNA end search y (in pixels)* - The y radius used during the DNA end search. Larger values will ensure DNAs of different lengths are found.
* *DNA end search x (in pixels)* - The x radius used during the DNA end search. If the DNAs are vertically aligned this value should be kept low. As this value is increase cross linking between adjacent DNAs might occur.
* *Filter by median intensity* - If checked DNAs will be removed if their median intensity is below the "Median DNA intensity lower bound" value.
* *Median DNA intensity lower bound* - A lower bound threshold for the median intensity of the DNA molecule. Sometimes two small DNA fragments are considered a single DNA. In this case the median DNA intensity is low. This filter can be used to remove these.

<img align='center' src='{{site.baseurl}}/docs/image/img/MedianFilter.png' width='500' />

* *Filter by intensity MSD* - If checked DNAs will be removed if the mean squared deviation of their intensity is above the "DNA intensity MSD upper bound" value.
* *DNA intensity MSD upper bound* - An upper bound threshold for the mean squared deviation of the DNA intensity. Sometimes multiple DNAs clump together or form arches. These structures are sometimes detected as single DNA molecules, but have a large mean squared intensity deviation. Setting an upper bound threshold for the MSD will remove these.

<img align='center' src='{{site.baseurl}}/docs/image/img/MSDFilter.png' width='500' />

<img align='center' src='{{site.baseurl}}/docs/image/img/MSDFilter2.png' width='500' />

* *Fit ends (subpixel localization)* - If checked the DNA ends will be fit with 2D Gaussians to determine their sub pixel position. If left unchecked, all the remaining settings will be ignored and the peaks will be reported with their integer pixel positions. See the [[Peak Finder]] for further information about fitting.
* *Fit Radius* - The radius of pixels used for fitting. 0 is one pixel, 1 is 9 pixels, 2 is 25 pixels. Usually 2 is a pretty good estimate depending on the peak size. There needs to be some pixels at the edges close to background for an ideal fit.
* *Initial Baseline* - The initial guess for the mean background pixel value.
* *Initial Height* - The initial guess for the difference between the mean background pixel at the value of the pixel at the top of the peak.
* *Initial Sigma* - The initial guess for the sigma of the 2D Gaussian. Approx. 68% of the peak intensity should lie within one sigma (within one standard deviation from the center).
* The Baseline, Height, and Sigma are all varied and no max error thresholds are applied. No max error thresholds are applied. If fitting results in NaN values, the pixel values will be used.
* *Preview label type* - To help with determination of median intensity and mean squared deviation thresholds, the values of these parameters can be displayed in preview mode. This radio button determines which of the two values to display. The labels may include k for x1,000 or m for x1,000,000 for clarity.
* *Preview* - Check this box to turn on preview mode. This will display an overlay of the DNAs that were located on the image. Sometime if you have a very large image this might be really slow, be patient. Otherwise, just make an ROI for preview testing, which will run faster. Once you have found the right settings you can cancel, remove the ROI, and run the command on the whole image with the correct settings.

#### Outputs

Several different kinds of output are possible depending on the settings used.

* *Generate DNA count table* - If checked a table will appear in which each row has the number of detected peaks for each slice.
* *Generate DNA table* - If checked a table will be generated listing the positions of all detected peaks. x1, y1 for the top edges and x2, y2 for the bottom edges. Also, the length calculated from the coordinates as well as the median intensity and mean squared deviation of the intensity.
* *Add to RoiManager* - If checked, lines peaks will be added to the RoiManager. By default there will be UID names.
* *Molecule Names in Manager* - If checked the lines in the RoiManager will be names molecule0, molecule1, etc...
* *Process all slices* - If checked the command will run on all slices in the video. For tables the slice for each DNA is given in a separate column. For the RoiManager the position is set based on the slice.

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@ ImageJ ij

import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.image.*
import de.mpg.biochem.mars.image.command.*

//Make an instance of the Command you want to run...
final DNAFinderCommand dnaFinder = new DNAFinderCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
dnaFinder.setContext(ij.getContext())

dnaFinder.setImage(image)
dnaFinder.setUseROI(false)
dnaFinder.setX0(0)
dnaFinder.setY0(0)
dnaFinder.setWidth(1024)
dnaFinder.setHeight(1024)
dnaFinder.setGaussianSigma(1)
dnaFinder.setThreshold(2)
dnaFinder.setMinimumDistance(4)
dnaFinder.setOptimalDNALength(34)
dnaFinder.setyDNAEndSearchRadius(27)
dnaFinder.setxDNAEndSearchRadius(3)
dnaFinder.setFilterByMedianIntensity(true)
dnaFinder.setMedianIntensityLowerBound(3000)
dnaFinder.setFilterByMSD(true)
dnaFinder.setMSDUpperBound(900000)
dnaFinder.setGenerateDNACountTable(false)
dnaFinder.setGenerateDNATable(true)
dnaFinder.setAddToRoiManager(false)
dnaFinder.setProcessAllSlices(false)
dnaFinder.setFitEnds(true)
dnaFinder.setFitRadius(2)
dnaFinder.setInitialBaseline(0)
dnaFinder.setInitialHeight(3000)
dnaFinder.setInitialSigma(1)

//Run the Command
dnaFinder.run();

//Retrieve output from the command
MarsTable dnaTable = dnaFinder.getDNATable()

//MarsTable dnaCountTable = dnaFinder.getDNACountTable()

//The other possible output is adding the peaks to the RoiManager
//in which case the RoiManager can then be use for further steps.
//However, keep in mind the RoiManager will probably not work
//in headless mode.
```
