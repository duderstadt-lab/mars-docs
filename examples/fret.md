---
layout: example
title: FRET dataset analysis using Mars
permalink: /examples/FRET/index.html
---

introduction

#### <a name="reference"></a>Table of contents

- [FRET sample design](#design)
- [General analysis procedure of FRET datasets](#calc)
- [Required Parameters for the Calculations](#required)
- [Dataset Characteristics](#data)
- [The MARS workflow](#workflow)
- [Data preparation - Opening and Converting the video](#1)
- [Data preparation - Calculation of the Affine2D matrix](#2)
- [Localization and Intensity vs. T traces](#3)
- [Filter Traces to only have 1 Donor and/or Acceptor](#4)


#### <a name="design"></a> FRET sample design


<div style="text-align: center">
Figure 1: . <sup>3</sup>
</div>

#### <a name="calc"></a> General Analysis Procedure of FRET Datasets

#### <a name="required"></a> Required Parameters for the Calculations


Required parameters:
DO Eapp:
- Iaemdex for peaks in green that are not present in red - intensity in the red part of the screen
- Idemdex for peaks in green that are not present in red - intensity in the green part of the screen


AO Sapp
- Iaemdex for peaks in red that are not present in green but are excited with green a little bit
- Idemdex for peaks in red that are not present in green but still give some small green signal upon green excitation
- Iaemaex for peaks in red that are excited in red (this one I have already in the red only frames)

#### <a name="data"></a> Dataset Characteristics
-> Make a figure showing the two populations, the overlap and each subpopulation of AO and DO for clarification.

Important features:
- Channel 0: excitation in red (Aex)
- Channel 1: excitation in green (Dex)
- Left part split: acceptor emission (Aem)
- Right part split: donor emission (Dem)

-> Make a table of this information showing how to obtain each intensity (from which combination)


#### <a name="workflow"></a> The MARS workflow

#### <a name="1"></a> Data preparation - Opening and Converting the video


1. Open the video with SciFIO turned on. Convert to a 2 channel video using the deinterleavev2.groovy script.

#### <a name="2"></a> Data preparation - Calculation of the Affine2D matrix

- link to Affine2D tutorial
- short intro on why and what we calculate
- why do we need to calculate the matrix in both directions
- Follow along or use the matrix supplied below

To calculate the Affine2D matrix open the file ['calib20140405_0.tif']() from the calibration fo (T) stack. Since the beads used in this calibration measurement are fluorescent at both measured wavelengths their position matches between both halves of the split view. Therefore this video can be used to calculate the Affine2D transformation matrix between the halves. To do so, select a time point of choice and duplicate the frame twice (Image>Duplicate). This will create two images of the video at the same time point. Rename the images 'left' and 'right' and in each image black out the respective halve of the view by selecting that region with the square ROI tool and pressing delete. This will result in a set of images resembling the ones in the screenshot below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img1.png' width='450'></div>

Next, open the registration tool (Plugins>Registration>Descriptor-based registration (2d/3d)) and select 'left' as the first image, 'right' as the second. Press OK and supply the settings as shown below in the next dialoge. Adjust the sigma and threshold values accordingly such that the peaks in the image are identified and press done. Use the default Gaussian fit parameters in the next window and press OK. This should open both an image displaying the overlap as well as a log window that displays the actual Affine2D matrix. Save the values of this matrix into a text document. Close all windows (including the left/right images), re-open them and repeat this analysis selecting 'right' as the first image and 'left' as the second.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img3.png' width='450'></div>

The following values were found: (note that matrices calculated at a different T or with different settings can result in slightly different values)
|      | Left to Right matrix     | Right to Left matrix |
| :------------- | :------------- | : ------ |
| m00       | 0.986       | 1.014 |
| m01       | 0           | 0     |
| m02       | 268.639     | -272.404|
| m10       | 0           | 0.004 |
| m11       | 0.993       | 1.007 |
| m12       | 1.170       | -2.299|


#### <a name="3"></a> Localization and Intensity vs. T traces
To calculate all FRET parameters and correction factors three different populations in the sample are of interest: the acceptor only (AO) population, the donor only (DO) population and the FRET population. To facilitate easy data analysis an archive is created for each of these populations separately. These can then later on be merged together while retaining information on to which population the identified molecule belongs, information that is required to do the actual calculations.

All three archives are created following the same pipeline: peak identification, ROI transformation to the other halve of the split view, ROI filtering and finally molecule integration to obtain intensity vs. T traces. Below, the pipeline to create the FRET archive is shown accompanied by screenshots. Please reference table 1 to repeat the archive creation also for the AO and DO archives. Alternatively, these archives can be downloaded from the [repository]().

##### The FRET Archive
**1. Identify the peaks in the red channel**
To find the peaks in the left part of the split view in the red channel first select this part of the screen with the box ROI tool and open the Peak Finder (Plugins>Mars>Image>Peak Finder). Apply the settings as shown below and check if the peaks are identified correctly by pressing the preview button. If the correct settings are applied the peaks should have an identification marker (circle or point) on them. Press ok to apply the settings and add the ROIs of the identified peaks to the ROI manager. The ROI manager will open and will display the peaks listed by their UUID in the manager.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img4.png' width='450'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img5.png' width='450'></div>

**2. Transform  the ROIs to the right part of the split view**
Next, transform the ROIs to the right part of the split view by using the Transform ROI tool (Plugins>Mars>ROI>Transform ROIs). Use the Affine2D matrix (left to right) as calculated before. Press ok to find the transformed ROIs in the ROI manager as UUID_short and UUID_long entries.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img6.png' width='450'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img7.png' width='450'></div>

**3. Apply the molecule integrator to extract I vs. T traces**
Integrate the peaks using the molecule integrator tools developed for dual view microscopy data (Plugins>Mars>Image>Molecule Integrator (dualview)). Provide the split view dimensions as shown in the screenshot and apply the 'colocalize' filter to only include peaks that are found both in red and green. For channel 0 select to only integrate the peaks in red (long) and in channel 1 integrate the peaks in both colors (both). Press ok and the archive will open automatically upon completion of the calculation.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img8.png' width='450'></div>


##### The AO and DO Archives

|                | FRET archive     | AO archive     | DO archive     |
| :------------- | :------------- | :------------- | :------------- |
| Molecule      | figure of DNA       | figure of DNA       | figure of DNA       |
| What to measure | Intensity of fluorescence in red upon red excitation, Intensity of fluorescence in green upon green excitation, Intensity of fluorescence in red upon green excitation (FRET)  | Intensity of fluorescence in red upon red excitation, Leaking fluorescence to the green channel upon red excitation, Leaking fluorescence to the red channel when excited with green  | Leaking fluorescence of red upon green excitation, Intensity of fluorescence in green upon green excitation  |
| Parameter names  | I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>  | I</sub>aemaex</sub><sup>AO</sup>, I<sub>demdex</sub><sup>AO</sup> & I<sub>aemdex</sub><sup>AO</sup>  |  I<sub>aemdex</sub><sup>DO</sup> & I<sub>demdex</sub><sup>DO</sup> |
| Analysis procedure  | Find peaks in red, Transform ROIs (L->R) to colocalize (C=1), Molecule integrator (C=0 long, 1 both)  | Find peaks in red, Transform ROIs (L->R) to filter out colocalizing peaks (C=1), Molecule integrator (C=0 long, 1 both)  | Find peaks in green, Transform ROIs (R->L) to filter out colocalizing peaks (C=0), Molecule integrator (C=1 both)  |

<div style="text-align: center">
Table 1: Overview of the three different archives that are created in this step of the analysis: the FRET archive, Acceptor Only (AO) archive and Donor Only (DO) archive. The first row of the table shows a representable molecule for the category, the second row explains what information needs to be extracted from this category of molecules, the third row the corresponding parameter names and the final row describes the analysis procedure that needs to be followed in Mars.
</div>


-> Make figures to illustrate what DO, AO and FRET situations would look like

##### Tag and Merge the Archives
To make the downstream processing procedure easier the next step is to tag the created archives accordingly and merge them to form one big archive. Use the tag function in the metadata tab to assign the tags 'FRET', 'AO' & 'DO' accordingly and save the archives in the same folder. Next select the merge archives tool (Plugins>Mars>Molecule>Merge Archive) and select the folder. When the command is finished a merged archive is created that can be found in the selected folder. Open the archive.

##### Plot the Traces to do a Visual Inspection
To do a visual inspection of the identified traces open the plot window in the molecules tab and add three line plots representing the three measured intensities (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>), if applicable, for each molecule. In the screenshot below the purple line represents I<sub>aemaex</sub>, the yellow line I<sub>aemdex</sub>, and the green line I<sub>demdex</sub>.


#### <a name="4"></a> Filter Traces to only have 1 Donor and/or Acceptor

















-