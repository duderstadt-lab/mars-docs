---
layout: image
title: DNA finder
permalink: /docs/image/DNA_finder/index.html
---

This command is used to find vertically aligned DNA molecules in images. Typically these are 20-50 kb DNAs that are surface immobilized and fluorescently stained. After locating the ends of the DNA molecules, this command can also perform 2D Gaussian fitting to determine the location of each end with subpixel accuracy. There are a lot of possible input/output options to allow for customizability so the same tool can be used with multiple image and DNA types.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/DNA Finder Example 1.png' width='450'/></div>

#### Inputs

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/DNA Finder Dialog.png' width='550'/></div>

* *Image* - The active image selected will be used by the DNA Finder. This is a required input but doesn't show up in the dialog.
* *use ROI* - If checked a subregion of the image will be used for processing. Otherwise, the entire image will be used. You can also add a selection with the box tool, by making a rectangular ROI to the image. This Roi will activate this box and add the settings below it.
* *ROI x0* - Upper left corner x0 position of ROI.
* *ROI y0* - Upper right corner y0 position of ROI.
* *ROI width* - width of the ROI.
* *ROI height* - height of the ROI.
* *Channel* - Select which channel to analyze in case a video with multiple channels is provided as input.
* *Gaussian Filter Sigma* - The sigma used for gaussian smoothing. The DNA Finder uses the scijava OpService op derivativeGauss which filters the image with a combined gaussian smoothing and takes the derivative. Usually a sigma of 1 is usually sufficient. Higher sigma values might lead to too much smoothing and obscure important features.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/derivativeSeriesSmall.png' width='800'/></div>

* *Use DoG filter* - If checked the image will be processed with a Difference of Gaussian (DoG) filter before peak finding. Using an appropriately chosen radius this filter enhances real peaks with signal spread among several pixels and suppresses salt and pepper noise as demonstrated in [this systematic study](../DoGFilterProperties). If unchecked the raw image will be used for peak finding.
* *DoG filter radius* - The radius used for DoG filtering. The value chosen should reflect the size of the desired peaks. Decimal numbers are permitted.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/DoGFilterRadiiExample.png' width='600'/></div>

* *Detection threshold* - This is threshold in pixel value used for peak detection. Pixels with values above this threshold are considered peaks and pixels below this threshold are considered background. For everyday peak detection, the DoG filter should be used in which case this threshold using on the DoG filtered image. In this case, we have found that values between 40 and 60 provide [optimal detection](../DoGFilterProperties) for typical single-molecule observations. If the DoG Filter is turned off this threshold reflects raw pixel values in the input image.
* *Minimum distance between edges (in pixels)* - This is the minimum allowed distance between peaks (edges here), This means only the pixel with the highest (or lowest) intensity within this radius will be accepted as a peak, even if there are other peaks above the threshold within this radius region. This is an important setting since most peaks have nearby pixels that are also above the detection threshold, but we only want to detect each peak once. If you see a lot of overlapping DNAs increasing this setting will help.
* *Optimal DNA length (in pixels)* - The mean length in pixels of the DNA you want to find. This value is used when attempting to link positive peaks to their negative counterparts to find DNAs.  
* *DNA end search y (in pixels)* - The y radius used during the DNA end search. Larger values will ensure DNAs of different lengths are found.
* *DNA end search x (in pixels)* - The x radius used during the DNA end search. If the DNAs are vertically aligned this value should be kept low. As this value is increase cross linking between adjacent DNAs might occur.
* *Filter by median intensity* - If checked DNAs will be removed if their median intensity is below the "Median DNA intensity lower bound" value.
* *Median DNA intensity lower bound* - A lower bound threshold for the median intensity of the DNA molecule. Sometimes two small DNA fragments are considered a single DNA. In this case the median DNA intensity is low. This filter can be used to remove these.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/MedianFilter.png' width='500'/></div>

* *Filter by intensity MSD* - If checked DNAs will be removed if the mean squared deviation of their intensity is above the "DNA intensity MSD upper bound" value.
* *DNA intensity MSD upper bound* - An upper bound threshold for the mean squared deviation of the DNA intensity. Sometimes multiple DNAs clump together or form arches. These structures are sometimes detected as single DNA molecules, but have a large mean squared intensity deviation. Setting an upper bound threshold for the MSD will remove these.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/MSDFilter.png' width='500'/></div>

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/MSDFilter2.png' width='500'/></div>

* *Fit ends (subpixel localization)* - If checked the DNA ends will be fit with 2D Gaussians to determine their sub pixel position. If left unchecked, all the remaining settings will be ignored and the peaks will be reported with their integer pixel positions. See the [[Peak Finder]] for further information about fitting.
* *Fit 2nd order* - Will perform the subpixel fitting on the second derivative image. If turned off the first derivative image is used. The second derivative image will result in more accurate length estimate but may be less stable.
* *Fit Radius* - The radius of pixels used for fitting. 0 is one pixel, 1 is 9 pixels, 2 is 25 pixels. Usually 2 is a pretty good estimate depending on the peak size. There needs to be some pixels at the edges close to background for an ideal fit.
* The Baseline, Height, and Sigma are all varied and no max error thresholds are applied. No max error thresholds are applied. If fitting results in NaN values, the pixel values will be used.
* *Preview label type* - To help with determination of median intensity and mean squared deviation thresholds, the values of these parameters can be displayed in preview mode. This radio button determines which of the two values to display. The labels may include k for x1,000 or m for x1,000,000 for clarity.
* *Preview* - Check this box to turn on preview mode. This will display an overlay of the DNAs that were located on the image. Sometime if you have a very large image this might be really slow, be patient. Otherwise, just make an ROI for preview testing, which will run faster. Once you have found the right settings you can cancel, remove the ROI, and run the command on the whole image with the correct settings.

#### Outputs

Several different kinds of output are possible depending on the settings used.

* *Generate DNA count table* - If checked a table will appear in which each row has the number of detected peaks for each slice.
* *Generate DNA table* - If checked a table will be generated listing the positions of all detected peaks. x1, y1 for the top edges and x2, y2 for the bottom edges. Also, the length calculated from the coordinates as well as the median intensity and mean squared deviation of the intensity.
* *Add to RoiManager* - If checked, lines peaks will be added to the RoiManager. By default there will be UID names.
* *Molecule Names in Manager* - If checked the lines in the RoiManager will be names molecule0, molecule1, etc...
* *Process all Frames* - If checked the command will run on all frames in the video. For tables the T for each DNA is given in a separate column. For the RoiManager the position is set based on the T.

### How to run this Command from a groovy script

```groovy
#@ Dataset dataset
#@ ImageJ ij
#@OUTPUT MarsTable dnaTable

import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.image.*
import de.mpg.biochem.mars.image.command.*

//Make an instance of the Command you want to run...
DNAFinderCommand dnaFinder = new DNAFinderCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
dnaFinder.setContext(ij.getContext())

dnaFinder.setDataset(dataset)
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
dnaTable = dnaFinder.getDNATable()

//MarsTable dnaCountTable = dnaFinder.getDNACountTable()

//The other possible output is adding the peaks to the RoiManager
//in which case the RoiManager can then be use for further steps.
//However, keep in mind the RoiManager will probably not work
//in headless mode.
```
