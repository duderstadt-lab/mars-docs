---
layout: example
mathjax: true
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
- [Calculate all Intensity Parameters and use these to create an uncorrected FRET Histogram](#5)
- [Trace-wise Background Correction](#6)
- [Correction for Leakage and Direct excitation](#7)
- [Correction for Excitation and Detection factors](#8)
- [Plotting & Data Exploration in Python](#9)
- [Conclusion: Global Analysis Outcomes and Comparisons to Literature](#10)


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

---

#### <a name="1"></a> Data preparation - Opening and Converting the video


1. Open the video with SciFIO turned on. Convert to a 2 channel video using the deinterleavev2.groovy script.

#### <a name="2"></a> Data preparation - Calculation of the Affine2D matrix

- [Affine2D tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/)
- short intro on why and what we calculate
- why do we need to calculate the matrix in both directions
- Follow along or use the matrix supplied below

To calculate the Affine2D matrix open the file ['calib20140405_0.tif']() from the calibration folder. Since the beads used in this calibration measurement are fluorescent at both measured wavelengths their position matches between both halves of the split view. Therefore this video can be used to calculate the Affine2D transformation matrix between the halves. To do so, select a time point of choice and duplicate the frame twice (Image>Duplicate). This will create two images of the video at the same time point. Rename the images 'left' and 'right' and in each image black out the respective halve of the view by selecting that region with the square ROI tool and pressing delete. This will result in a set of images resembling the ones in the screenshot below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img1.png' width='450'></div>

Next, open the registration tool (Plugins>Registration>Descriptor-based registration (2d/3d)) and select 'left' as the first image, 'right' as the second. Press OK and supply the settings as shown below in the next dialoge. Adjust the sigma and threshold values accordingly such that the peaks in the image are identified and press done. Use the default Gaussian fit parameters in the next window and press OK. This should open both an image displaying the overlap as well as a log window that displays the actual Affine2D matrix. Save the values of this matrix into a text document. Close all windows (including the left/right images), re-open them and repeat this analysis selecting 'right' as the first image and 'left' as the second.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img3.png' width='450'></div>

The following values were found: (note that matrices calculated at a different T or with different settings can result in slightly different values)

|                 | Left to Right matrix     | Right to Left matrix |
| :-------------  | :------------- | : ------ |
| m<sub>00</sub>             | 0.986          | 1.014 |
| m<sub>01</sub>             | 0              | 0     |
| m<sub>02</sub>             | 268.639        | -272.404|
| m<sub>10</sub>             | 0              | 0.004 |
| m<sub>11</sub>             | 0.993          | 1.007 |
| m<sub>12</sub>             | 1.170          | -2.299|


#### <a name="3"></a> Localization and Intensity vs. T traces
To calculate all FRET parameters and correction factors three different populations in the sample are of interest: the acceptor only (AO) population, the donor only (DO) population and the FRET population. To facilitate easy data analysis an archive is created for each of these populations separately. These can then later on be merged together while retaining information on to which population the identified molecule belongs, information that is required to do the actual calculations.

All three archives are created following the same pipeline: peak identification, ROI transformation to the other halve of the split view, ROI filtering and finally molecule integration to obtain intensity vs. T traces. Below, the pipeline to create the FRET archive is shown accompanied by screenshots. Please reference table 1 to repeat the archive creation also for the AO and DO archives. Alternatively, these archives can be downloaded from the [repository]().

-> Add an image showing archive logistics (FRET + AO + DO archive)

##### The FRET Archive
**1. Identify the peaks in the red channel**  
To find the peaks in the left part of the split view in the red channel first select this part of the screen with the box ROI tool and open the Peak Finder (Plugins>Mars>Image>Peak Finder). Apply the settings as shown below and check if the peaks are identified correctly by pressing the preview button. If the correct settings are applied the peaks should have an identification marker (circle or point) on them. Press ok to apply the settings and add the ROIs of the identified peaks to the ROI manager. The ROI manager will open and will display the peaks listed by their UUID in the manager.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img4.png' width='450'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img5.png' width='250'></div>

**2. Transform  the ROIs to the right part of the split view**  
Next, transform the ROIs to the right part of the split view by using the Transform ROI tool (Plugins>Mars>ROI>Transform ROIs). Use the Affine2D matrix (left to right) as calculated before. Press ok to find the transformed ROIs in the ROI manager as UUID_short and UUID_long entries.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img6.png' width='450'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img7.png' width='250'></div>

**3. Apply the molecule integrator to extract I vs. T traces**  
Integrate the peaks using the molecule integrator tools developed for dual view microscopy data (Plugins>Mars>Image>Molecule Integrator (dualview)). Provide the split view dimensions as shown in the screenshot and apply the 'colocalize' filter to only include peaks that are found both in red and green. For channel 0 select to only integrate the peaks in red (long) and in channel 1 integrate the peaks in both colors (both). Press ok and the archive will open automatically upon completion of the calculation.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img8.png' width='450'></div>


##### The AO and DO Archives

|                | FRET archive     | AO archive     | DO archive     |
| :------------- | :------------- | :------------- | :------------- |
| Molecule      | figure of DNA       | figure of DNA       | figure of DNA       |
| What to measure | Intensity of fluorescence in red upon red excitation, Intensity of fluorescence in green upon green excitation, Intensity of fluorescence in red upon green excitation (FRET)  | Intensity of fluorescence in red upon red excitation, Leaking fluorescence to the green channel upon red excitation, Leaking fluorescence to the red channel when excited with green  | Leaking fluorescence of red upon green excitation, Intensity of fluorescence in green upon green excitation  |
| Parameter names  | I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>  | I<sub>aemaex</sub><sup>(AO)</sup>, I<sub>demdex</sub><sup>(AO)</sup> & I<sub>aemdex</sub><sup>(AO)</sup>  |  I<sub>aemdex</sub><sup>(DO)</sup> & I<sub>demdex</sub><sup>(DO)</sup> |
| Analysis procedure  | Find peaks in red, Transform ROIs (L->R) to colocalize (C=1), Molecule integrator (C=0 long, 1 both)  | Find peaks in red, Transform ROIs (L->R) to filter out colocalizing peaks (C=1), Molecule integrator (C=0 long, 1 both)  | Find peaks in green, Transform ROIs (R->L) to filter out colocalizing peaks (C=0), Molecule integrator (C=1 both)  |

<div style="text-align: center">
Table 1: Overview of the three different archives that are created in this step of the analysis: the FRET archive, Acceptor Only (AO) archive and Donor Only (DO) archive. The first row of the table shows a representable molecule for the category, the second row explains what information needs to be extracted from this category of molecules, the third row the corresponding parameter names and the final row describes the analysis procedure that needs to be followed in Mars.
</div>


-> Make figures to illustrate what DO, AO and FRET situations would look like

##### Tag and Merge the Archives
To make the downstream processing procedure easier the next step is to tag the created archives accordingly and merge them to form one big archive. Use the tag function in the metadata tab to assign the tags 'FRET', 'AO' & 'DO' accordingly and save the archives in the same folder. Next select the merge archives tool (Plugins>Mars>Molecule>Merge Archive) and select the folder. When the command is finished a merged archive is created that can be found in the selected folder. Open the archive.

##### Plot the Traces to do a Visual Inspection
To do a visual inspection of the identified traces open the plot window in the molecules tab and add three line plots representing the three measured intensities (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>), if applicable, for each molecule. In the screenshot below the red line represents I<sub>aemaex</sub>, the grey line I<sub>aemdex</sub>, and the blue line I<sub>demdex</sub>.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img9.png' width='450'></div>


#### <a name="4"></a> Filter Traces to only have 1 Donor and/or Acceptor
In the next step, the identified traces will be filtered for having only 1 donor and/or acceptor instead of multiple. This removes molecules from the analysis that might have clumped together therefore showing the presence of multiple fluorophores at one location. This prevents bias in the calculation of the FRET parameters caused by unjustified intensity values.

First, assign a global (metadata) region for the interval 2<T<500 named 'active' to all metadata records. By analyzing this region of the data only the first frames displaying drastic intensity differences are removed from further analysis.
Next, identify the bleaching steps in each trace using the 'change point finder' tool (Plugins>Mars>KCP>Change Point Finder). Apply the settings as shown below. Repeat this step for all intensity channels (Y column: 1 Green, 1 Red & 0)

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img10.png' width='450'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img11.png' width='450'></div>

The filtering step itself is automized in a [script](). Please [download]() the script and open it in Fiji to run in the script editor. For more information on running scripts in Fiji visit the [introduction to Groovy scripting tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/introduction-to-groovy-scripting/).
In short, the script calculates the difference between the measured intensities in the change point analysis and assigns a Boolean parameter to the molecule indicating if this difference is larger than zero (∆I > 0). Next, it evaluates whether a single bleaching step or multiple bleaching steps are observed and assigns this to the Boolean parameters 'Red_singlebleach' and 'Green_singlebleach respectively'. After the declaration of these parameters, the molecules that are part of the FRET archive are tagged. A tag ('Active') is added to the molecule in case bleaching is observed both in red and green, in other cases the tag 'background' is assigned. Subsequently, the tag 'Active_single' is assigned to molecules that show a single bleaching event in both colors.

#### <a name="5"></a> Calculate all Intensity Parameters and use these to create an uncorrected FRET Histogram

The next step is to assign the measured fluorophore intensities to the corresponding intensity parameters (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>). To do so, download [script 2]() from the repository and run it in the Fiji script editor. This script determines the highest fluorophore intensity level as reported in the segments table and assigns this to the corresponding parameter name. Later on in the pipeline discriminating the different populations (FRET, AO, DO) as well as selecting only the 'Active_single'-tagged molecules is possible by means of using the assigned tags.

To generate a crude and uncorrected 2D histogram of the FRET values obtained in the analysis these intensity  parameters are supplemented into the following two formulae calculating the uncorrected apparent E and S value for each molecule. To do so download [script 3]() and run in the Fiji script editor. Note that for these calculations only the intensity values corresponding to molecules from the FRET archive, tagged with 'Active_single', are taken into consideration.

$$\begin{equation}
^iS_{app} = \frac{I_{Aem|Dex}^{Fret} + I_{Dem|Dex}^{Fret}}{I_{Aem|Dex}^{Fret} + I_{Dem|Dex}^{Fret} + I_{Aem|Aex}^{Fret}}
\end{equation}$$

$$\begin{equation}
^iE_{app} = \frac{I_{Aem|Dex}^{Fret}}{I_{Aem|Dex}^{Fret} + I_{Dem|Dex}^{Fret}}
\end{equation}$$

Next, display the calculated $$^iS_{app}$$ and $$^iE_{app}$$ in the bubble chart widget in rover. Open the bubble chart on the main dashboard, switch to the coding tab (<>) and paste the Python code that can be downloaded in [script 4](). Run by pressing the refresh button and move back to the bubble plot icon tab to show the plot that should be similar to the one below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img12.png' width='450'></div>

#### <a name="6"></a> Trace-wise Background Correction
The first correction that is applied to the dataset is a background correction. In this trace-wise process the mean fluorophore intensity after fluorophore bleaching is considered to be the background signal and is subtracted from the calculated fluorophore intensity. This is done for each intensity measurement (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>) separately and in a trace-wise manner. The values of the corrected I parameters are stored in the archive as <sup>ii</sup>I<sub>aemaex</sub>, <sup>ii</sup>I<sub>demdex</sub> & <sup>ii</sup>I<sub>aemdex</sub> respectively. To do this, download [script 5]() and run in the Fiji script editor.

#### <a name="7"></a> Correction for Leakage ($\alpha$) and Direct excitation ($\delta$)
In this step two data corrections are carried out: a correction for leakage, the process of donor emission in the acceptor detection channel, and a correction for direct excitation, the process of acceptor emission by direct excitation of the acceptor at the donor wavelength. Both lead to signal intensity distortions when uncorrected and would influence the measured FRET parameters. Therefore they are corrected in this part of the analysis procedure. For more information about these correction parameters the reader is referred to [literature]().

The leakage ($\alpha$) and direct excitation ($\delta$) correction factors can be calculated using the formulae below. As indicated, these formulae require the calculated fluorescence intensities from the DO and AO populations respectively. These are calculated using [this script]() from the data that was collected in the DO and AO archives previously. Subsequently, they are implemented in the latter three formulae to find the corrected E and S values. Please download [the script]() and run on the archive using the Fiji script editor. The $\alpha$ and $\delta$ correction factors as well as the calculated F<sub>A|D</sub>, <sup>iii</sup>E<sub>app</sub><sup>(DO)</sup>, and <sup>iii</sup>S<sub>app</sub><sup>(AO)</sup> values will appear as parameters in the archive.


$$\begin{equation}
\alpha = \frac{\langle ^{ii}E_{app}^{(DO)} \rangle}{1 - \langle   ^{ii}E_{app}^{(DO)} \rangle}
    \quad\mathrm{and}\quad
\delta = \frac{\langle ^{ii}S_{app}^{(AO)} \rangle}{1 - \langle ^{ii}S_{app}^{(AO)} \rangle}
\end{equation}$$

$$\begin{equation}
F_{A|D}=^{ii}I_{Aem|Dex} - \alpha ^{ii}I_{Dem|Dex} - \delta ^{ii}I_{Aem|Aex}
\end{equation}$$

$$\begin{equation}  
^{iii}E_{app} = \frac{F_{A|D}}{F_{A|D} + ^{ii}I_{Dem|Dex}}
   \quad\mathrm{and}\quad   
^{iii}S_{app} = \frac{F_{A|D} + ^{ii}I_{Dem|Dex}}{F_{A|D} + ^{ii}I_{Dem|Dex} + ^{ii}I_{Aem|Aex}}
\end{equation}$$

#### <a name="8"></a> Correction for Excitation ($\beta$) and Detection factors ($\gamma$)

The last data correction step corrects for excitation ($\beta$), normalizing excitation intensities and cross-sections of both acceptor and donor, and detection factors ($\gamma$), normalization of effective fluorescence quantum yields and detection efficiencies of both donor and acceptor.
To find both global correction factors a linear regression analysis is performed with the following formulae:


$$\begin{equation}  

^{iii}S_{app}^{(FRET)} = \frac{1}{1 + \gamma * \beta + (1-\gamma)*\beta *^{iii}E_{app}^{(FRET)}}
\end{equation}$$

$$\begin{equation}  
      \quad\mathrm{and}\quad
frac{1}{^{iii}S_{app}^{(FRET)}} = 1 + \gamma* \beta + (1-\gamma)*\beta *^{iii}E_{app}^{(FRET)} = b + a * ^{iii}E_{app}^{(FRET)

\end{equation}$$

From this equation the dependencies of $\beta$ and $\gamma$ on a and b can be deducted to be:


$$\begin{equation}
\beta = a + b + 1
       \quad\mathrm{and}\quad
\gamma = \frac{b - 1}{a + b + 1}
\end{equation}$$

Once the values of $\beta$ and $\gamma$ have been calculated, the fully corrected E and S values can be calculated. To do so, first F<sub>D|D</sub> and F<sub>A|A</sub> are calculated, then added to the archive, followed by the calculation of E and S. Download [this script]() and run on the archive to obtain these values.


$$\begin{equation}
F_{D|D} = \gamma * ^{ii}I_{Dem|Dex}
   \quad\mathrm{and}\quad   
F_{A|A} = \frac{1}{\beta} * ^{ii}I_{Aem|Aex}
\end{equation}$$

$$\begin{equation}
E = \frac{F_{A|D}}{F_{D|D} + F_{A|D}}
  \quad\mathrm{and}\quad
S = \frac{F_{A|D} + F_{D|D}}{F_{D|D} + F_{A|D} + F_{A|A}}

\end{equation}$$

#### <a name="9"></a> Plotting & Data Exploration in Python
To explore the data, open [this script]() and copy the code to the scatterplot widget in the Rover dashboard. Run in 'Python' and obtain a scatterplot similar to the one below. In this plot the grey points refer to molecules in the 'FRET' archive, blue points to molecules in the 'DO' archive and red points to molecules in the 'AO' archive. To find the ensemble averages E and S for the population, please open [this Jupyter notebook] to perform the Gaussian fit.
_Note that the analysis of a single video from the dataset such as done in this example leads to a relatively low number of data points. This affects the accuracy of the calculated correction factors and E and S values. The accuracy can be improved by analyzing all videos supplied in the repository corresponding to this dataset. The outcome of such an analysis is presented in the next section._

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img13.png' width='450'></div>

#### <a name="10"></a> Conclusion: Global Analysis Outcomes and Comparisons to Literature



---


$$\begin{equation}

   \quad\mathrm{and}\quad   

\end{equation}$$


$$\begin{equation}
\end{equation}$$







-
