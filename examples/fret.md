---
layout: example
mathjax: true
title: smFRET dataset analysis with Mars
permalink: /examples/FRET/index.html
---

In this example a typical Mars pipeline for the analysis of a smFRET (single-molecule Förster Resonance Energy Transfer)<sup>[1](https://doi.org/10.1038/nmeth.1208), [2](https://doi.org/10.1073/pnas.58.2.719), [3](https://doi.org/10.1073/pnas.93.13.6264)</sup> datasets is discussed. This popular single-molecule technique is used extensively by many labs to investigate the behavior of bio-macromolecules like proteins, RNA, and DNA on a single-molecule level. Among numerous interesting and revealing studies, some examples of important processes that have been studied in this way are: the structural organization of bacterial RNA polymerase <sup>[4](https://doi.org/10.1016/s0092-8674(02)00667-0)</sup>, protein-protein complex formation in neurotransmission pathways <sup>[5](https://doi.org/10.1038/nsmb.1763)</sup>, dynamics of multi-domain proteins like Hsp90 in solution <sup>[6](https://doi.org/10.1038/nmeth.4081)</sup>, protein folding upon ligand interaction <sup>[7](https://doi.org/10.1016/J.SBI.2007.12.003)</sup>, and studies of the cAMP/PKA pathways <sup>[8](https://doi.org/10.1016/J.BBRC.2017.04.097)</sup>.

Mars is very well equipped to be used to analyse the data gathered in such studies. It offers a highly effective, easy to use, well documented, and powerful open source alternative to home-built software currently used by some labs and focusses on reproducibility and transparency of the analysis process. To showcase its applicability to analyse smFRET datasets, in this example, the data of the benchmark study published by Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> is analysed and compared. We show the typical pathway one can follow to analyse the data and find the FRET values E and S from this dataset.


#### <a name="reference"></a>Table of contents

- [The FRET Sample Design and MARS Analysis Process](#design)
- [Data preparation](#1)
- [Localization of Peaks and Intensity vs. T traces](#3)
- [Data Analysis and Corrections](#4)
- [Plotting & Data Exploration in Python](#9)
- [Conclusion: Global Analysis Outcomes and Comparisons to Literature](#10)
- [References](#11)
- [Troubleshooting](#12)

---

#### <a name="design"></a> The FRET Sample Design and MARS Analysis Process

**Sample Design**  
In the experiment as designed by Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> the distance between two fluorophores (a donor and an acceptor) on a DNA molecule is probed. Two different samples (figure 1: 1-lo and 1-mid) are measured with a different interfluorophore distance as indicated in the figure. Since the FRET efficiency (E) is inversely correlated to the distance between the fluorophores, a different E value is to be expected for both molecules. The fluorescence of both samples was measured using both TIRF and confocal single-molecule fluorescence set-ups after which the FRET parameters E and S were determined. In this example, the 1-lo dataset recorded on a TIRF microscope set-up will be analysed. The raw dataset can be downloaded [here](https://zenodo.org/record/1249497#.YMccli0RrGI). More information on the data acquirement and sample specifics can be found in the publication by Hellenkamp *et al.*<sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img14.png' width='450'></div>

<div style="text-align: center">
Figure 1: Representation of the two different molecules included in the study by Hellenkamp et al. The red sphere (left side DNA molecule) represents the acceptor fluorophore, in this case Atto647N, and the green sphere (right side DNA molecule) represents the donor fluorophore, in this case Atto550. In this example the analysis of the 1-lo dataset is discussed. In the final paragraph of this example results from the 1-mid dataset are discussed as well. <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>
</div>

**The Analysis Procedure**  
The analysis of an smFRET dataset consists of a number of typical steps: (figure 2)
- Data recording: recording of the fluorescent emission signal both upon acceptor excitation (figure 2, C=0) and donor excitation (C=1) over time.
- Data analysis: starting by the extraction of intensity vs. time traces by means of peak integration followed by the application of several correction factors accounting for background differences, signal leakage, direct excitation effects and detection factors.
- Data interpretation: calculation of the FRET efficiency (E) and stoichiometry (S) for each molecule followed by the identification of the ensemble average E and S values for the population.

For more in-depth information on the analysis procedure and accompanying formulae justification the reader is kindly referred to the study published by Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img20.png' width='650'></div>

<div style="text-align: center">
Figure 2: General overview of the analysis process of the smFRET dataset consisting of data recording and data analysis steps in Mars leading to the calculation of the FRET efficiency (E) and stoichiometry (S) values.
</div>


**Dataset Characteristics**  
The data as provided by Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> was recorded with a TIRF microscope setup equipped with dual view collection. The detection area of the camera is split in half, each half displaying the signal after a different wavelength filter. In this way emission can be measured for two wavelengths at the same time and signal correlation is possible. In practise, for this dataset, this means that red emission is collected and shown on the left half, and the green emission on the right half of the window (figure 3). The excitation color alternates between red (C=0) and green (C=1) such that the different channels and split orientation give the peak intensities as denoted below each half by I<sub>emission|excitation</sub>. These intensities are integrated during analysis and after correction lead to the final E and S values.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img21.png' width='750'></div>

<div style="text-align: center">
Figure 3: Overview of the different signals that are obtained from the analysis dependent on channel number and location on the split view.
</div>

**The MARS Workflow**  
The previously introduced analysis steps can be implemented in Mars directly. Figure 4 shows the specific steps and tools that are used in the analysis. [Documentation](https://duderstadt-lab.github.io/mars-docs/docs/) is available for each Mars tool specifically.
In short, first the video format is converted to contain channel information (excitation color, C=0, 1). Next, peaks are identified and correlated with the other half of the split view using the ROI transformation tool. The molecule integrator tool, subsequently, yields the peak intensity values vs. time for each peak and leads to the generation of three archives for the video: a FRET archive (peaks with both donor and acceptor emission), an acceptor only archive (AO, peaks with acceptor emission only), and a donor only archive (DO, peaks with donor emission only). These are tagged accordingly and merged into one archive. The kinetic change point (KCP) analysis and various data correction steps yield corrected intensity values to calculate E and S values of each molecule. These are plotted in the Rover Dashboard using one of the in-built widgets. A final plot with a Gaussian distribution fit to find the population average is then made using Python.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img22.png' width='850'></div>

<div style="text-align: center">
Figure 4: Schematic representation of the Mars workflow starting from the video as supplied from the database, ending with a fully analyzed S vs. E plot generated in Mars. All steps executed with Mars are shown on the orange background. In this analysis the final plot is made both using a Mars dashboard widget as well as a Python-based plot. (AO: acceptor only, DO: donor only)
</div>

This example pipeline highlights the flexibility of Mars to be adapted to the specific dataset catering to the needs for specific implementation steps and flexible archive merging.


---

#### <a name="1"></a> Data preparation
**Opening and Converting the Video**  
First, [download](https://zenodo.org/record/1249497) the dataset from Hellenkamp *et al.*<sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>. Scroll down on the linked page and download the files named: '1_lo_TIFF.zip', '1_med_TIFF.zip' & 'Calibration_Files_for_TIFFs.zip'.
To prepare the video for easy analysis, turn SciFIO on in Fiji (Edit>Options>ImageJ2 tick the box) and open the first video (FSII1a_g30r84t200_0.tif) located in the '1_lo_TIFF.zip' folder. The original video does not contain specific channel information but simply displays the results of the different excitation colors in an alternating fashion. To convert the video to have channel information, run [script 1](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script1_convert%20video%20to%20dual%20channel.groovy). For information on how to run a script in Fiji please read [this tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/introduction-to-groovy-scripting/). If the script ran correctly, a new window should have opened showing a video with the channels represented in red and green.


**Calculation of the Affine2D Matrix**  
Next, to prepare for the data analysis, an Affine2D conversion matrix needs to be calculated. This matrix is used to correlate the peak position as found on one half of the split view to the corresponding position on the other half of the split view. More information about this calculation can be found in the [Affine2D tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/). Either follow along in the next steps to calculate the matrix or use the result from the calculation as provided in the table below.

To calculate the Affine2D matrix open the file ['calib20140402_0.tif'](https://zenodo.org/record/1249497) from the calibration folder 'Calibration_Files_for_TIFFs.zip'. This file shows a recording of a bead sample that fluoresces in multiple colors upon excitation with a wide variety of wavelengths. In this way, matching of the positions in both halves of the split view is possible. To do so, select a time point of choice and duplicate the frame twice (Image>Duplicate). This will create two images of the video at the same time point. Rename the images 'left' and 'right' and in each image black out the respective halve of the view by selecting that region with the square ROI tool and pressing delete. This will result in a set of images resembling the ones in the screenshot below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img1.png' width='450'></div>

Next, open the registration tool (Plugins>Registration>Descriptor-based registration (2d/3d)) and select 'left' as the first image, 'right' as the second. Press OK and supply the settings as shown below in the next dialoge. After pressing OK, adjust the sigma and threshold values accordingly such that the peaks in the image are identified and press done. Use the default Gaussian fit parameters in the next window and press OK. This should open both an image displaying the overlap as well as a log window that displays the actual Affine2D matrix. Save the values of this matrix into a text document. Close all windows (including the left/right images), re-open them and repeat this analysis selecting 'right' as the first image and 'left' as the second to find the conversion matrix in the other direction.

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

Table 1: Affine2D conversion matrix values

---

#### <a name="3"></a> Localization of Peaks and Intensity vs. T traces
To calculate all FRET parameters and correction factors three different populations in the sample are of interest: the acceptor only (AO) population, the donor only (DO) population and the FRET population. To facilitate easy data analysis an archive is created for each of these populations separately. These are then later on merged together while retaining information on to which population the identified molecule belongs, information that is required to do the correction factor calculations.

All three archives are created following the same pipeline: peak identification, ROI transformation to the other halve of the split view, ROI filtering and finally molecule integration to obtain intensity vs. T traces in an Archive. Below, the pipeline to create the FRET archive is shown accompanied by screenshots. Please reference table 2 to repeat the archive creation also for the AO and DO archives. Alternatively, these archives can be downloaded from the [repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Archives).

##### The FRET Archive
**1. Identify the red peaks in the red channel (C=0, left)**  
To find the peaks in the left part of the split view in the red channel first select this part of the screen with the box ROI tool and open the Peak Finder (Plugins>Mars>Image>Peak Finder). Apply the settings as shown below and check if the peaks are identified correctly by pressing the preview button. If the correct settings are applied the peaks should have an identification marker (circle or point) on them. Press ok to apply the settings and add the ROIs of the identified peaks to the ROI manager. The ROI manager will open and will display the peaks listed by their UUID in the manager.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img27.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img28.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img29.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img30.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img31.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img32.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img5.png' width='450'></div>

**2. Transform  the ROIs to the right part of the split view**  
Next, transform the coordinates (ROIs) of the peaks that were just identified to match the right part of the split view. In this way, in the next analysis step, integration of the peak intensity is possible at both emission wavelengths at the same time. Transform the ROIs to the right part of the split view by using the Transform ROI tool (Plugins>Mars>ROI>Transform ROIs). Use the Affine2D matrix (left to right) as calculated before (see table 1). Provide the split view dimensions as shown in the screenshot and apply the 'colocalize' filter to only include peaks that are found both in red and green. Press ok to find the transformed ROIs in the ROI manager as UUID_short and UUID_long entries.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img33.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img34.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img35.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img36.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img7.png' width='450'></div>

**3. Apply the molecule integrator to extract I vs. T traces**  
Integrate the peaks using the molecule integrator tools developed for dual view microscopy data (Plugins>Mars>Image>Molecule Integrator (dualview)). For channel 0 select to only integrate the peaks in red (long) and in channel 1 integrate the peaks in both colors (both). Press ok and the archive will open automatically upon completion of the calculation. Now the generation of the first archive, the FRET archive, is complete. Repeat the three steps above for both the AO and DO archive with the details as listed below in table 3.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img37.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img38.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img39.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/img40.png' width='450'></div>

The molecule integrator automatically names the intensity traces according to the provided channel and wavelength color information. Please reference table 3 for an overview of the corresponding archive names to the intensity parameter names as used in the example.

| Intensity parameter     | Name in Mars     |
| :------------- | :------------- |
| I<sub>aemaex</sub>       | "0" (corresponding to C=0, red excitation, red emission)       |
| I<sub>aemdex</sub>       | "1 Red" (corresponding to C=1, green excitation, red emission)       |
| I<sub>demdex</sub>       | "1 Green" (corresponding to C=1, green excitation, green emission)       |


Table 2: Overview of definitions of intensity values with their corresponding names in the Molecule Archive.



##### The AO and DO Archives

|                | FRET archive     | AO archive     | DO archive     |
| :------------- | :------------- | :------------- | :------------- |
| Molecule      | <img align='center' src='{{site.baseurl}}/examples/img/fret/img17.png' width='250'> | <img align='center' src='{{site.baseurl}}/examples/img/fret/img19.png' width='250'> | <img align='center' src='{{site.baseurl}}/examples/img/fret/img18.png' width='240'> |
| What to measure | Intensity of fluorescence in red upon red excitation (I<sub>aemaex</sub>), Intensity of fluorescence in green upon green excitation (I<sub>demdex</sub>), Intensity of fluorescence in red upon green excitation (FRET, I<sub>aemdex</sub>)  | Intensity of fluorescence in red upon red excitation (I<sub>aemaex</sub>), Leaking fluorescence to the green channel upon green excitation (I<sub>demdex</sub>), Leaking fluorescence to the red channel when excited with green (I<sub>aemdex</sub>)  | Leaking fluorescence of red upon green excitation (I<sub>aemdex</sub>), Intensity of fluorescence in green upon green excitation (I<sub>demdex</sub>)  |
| Parameter names  | I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>  | I<sub>aemaex</sub><sup>(AO)</sup>, I<sub>demdex</sub><sup>(AO)</sup> & I<sub>aemdex</sub><sup>(AO)</sup>  |  I<sub>aemaex</sub><sup>(DO)</sup>, I<sub>aemdex</sub><sup>(DO)</sup> & I<sub>demdex</sub><sup>(DO)</sup> |
| Analysis procedure  | Find peaks in red, Transform ROIs (Affine: L->R) to colocalize (C=1) | Find peaks in red, Transform ROIs (Affine: L->R) to filter out colocalizing peaks (C=1)  | Find peaks in green, Transform ROIs (Affine: R->L) to filter out colocalizing peaks (C=0)  |

<div style="text-align: center">
Table 3: Overview of the three different archives that are created in this step of the analysis: the FRET archive, Acceptor Only (AO) archive and Donor Only (DO) archive. The first row of the table shows a representable molecule for the category, the second row explains what information needs to be extracted from this category of molecules, the third row the corresponding parameter names, and the final row describes the analysis procedure that needs to be followed in Mars.
</div>

Change the settings in the previously described tools (peak finder, transform ROIs & molecule integrator) according to the table to generate the AO and DO archives. In summary:

**DO Archive**  
- Peak finder: channel = 1
- Transform ROIs: R->L Affine 2D Matrix, short to long, channel = 0, 'Remove colocalizing ROIs' = True.

**AO Archive**  
- Peak finder: channel = 0
- Transform ROIs: L->R Affine 2D Matrix, long to short, channel = 1, 'Remove colocalizing ROIs' = True.



**Tag and Merge the Archives**  
To make the downstream processing procedure easier the next step is to tag the created archives accordingly and merge them to form one big archive. Use the tag function in the metadata tab to assign the tags 'FRET', 'AO' & 'DO' accordingly and save the archives in the same folder. Next select the merge archives tool (Plugins>Mars>Molecule>Merge Archive) and select the folder. When the command is finished a merged archive is created that can be found in the selected folder. Open the archive.
*Note:* it is very important to save the archive after tagging. Otherwise the tag will not be in the merged archive and errors will be raised in the subsequent analysis steps.

**Plot the Traces to do a Visual Inspection**  
To do a visual inspection of the identified traces open the plot window in the molecules tab and add three line plots representing the three measured intensities (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>), if applicable, for each molecule. To do so select the options as shown in the screenshot.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img24.png' width='450'></div>

In the screenshot below the red line represents I<sub>aemaex</sub>, the grey line I<sub>aemdex</sub> (FRET), and the blue line I<sub>demdex</sub>. These are the three intensity vs. time traces that are required to do further FRET calculations on.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img9.png' width='650'></div>

---

#### <a name="4"></a> Data Analysis and Corrections
##### Filter Traces to only have 1 Donor and/or Acceptor
In the next step, the identified traces will be filtered for having only 1 donor and/or acceptor instead of multiple. This removes molecules from the analysis that have clumped together and prevents bias in the calculation of the FRET parameters caused by unjustified intensity values.

First, assign a global (metadata) region for the interval 2<T<500 named 'active' to all metadata records (Alternatively, use [script B](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/ScriptB_assign%20region%20and%20KCP_1lo_0_4.groovy) to do this and the next step in an automated fashion). By analyzing this region of the data only the first frames displaying drastic intensity differences are removed from further analysis.
Next, identify the bleaching steps in each trace using the 'change point finder' tool (Plugins>Mars>KCP>Change Point Finder). Apply the settings as shown below. Repeat this step for all intensity channels (Y column: 1 Green, 1 Red & 0)

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img10.png' width='450'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img11.png' width='450'></div>

The filtering step itself is automized in [script 2](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script2_filtering%20and%20tagging.groovy). Please [download](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script2_filtering%20and%20tagging.groovy) the script and open it in Fiji to run in the script editor. For more information on running scripts in Fiji visit the [introduction to Groovy scripting tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/introduction-to-groovy-scripting/).
In short, the script calculates the difference between the measured intensities in the change point analysis and assigns a Boolean parameter to the molecule indicating if this difference is larger than zero (∆I > 0). Next, it evaluates whether a single bleaching step or multiple bleaching steps are observed and assigns this to the Boolean parameters 'Red_singlebleach', 'Green_dual_state', and 'Green_tri_state' respectively. These parameters account for single acceptor bleaching and for single donor bleaching before (Green_dual_state) and after the acceptor (Green_tri_state). Next, the script looks for outliers where the background signal ratio between the green and red signal is too large. In that case traces are tagged 'Outlier_signal' and will be excluded from further analysis.
After the declaration of these parameters, the molecules that are part of the FRET archive are tagged. A tag ('Active') is added to the molecule in case bleaching is observed both in red and green. The tag 'Active_single' is used in case the trace shows single bleaching events only.

##### Calculate all Intensity Parameters and use these to create an uncorrected FRET Histogram





The next step is to assign the measured fluorophore intensities to the corresponding intensity parameters (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>). To do so, download [script 3](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script3_apparent%20Ivalues.groovy) from the repository and run it in the Fiji script editor. This script determines the highest fluorophore intensity level as reported in the segments table and assigns this to the corresponding parameter name.

To generate a crude and uncorrected 2D histogram of the FRET values obtained in the analysis these intensity  parameters are supplemented into the following two formulae calculating the uncorrected apparent E and S value for each molecule. To do so download [script 4](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script4_iESapp%20calculation.groovy) and run in the Fiji script editor. Note that for these calculations only the intensity values corresponding to molecules from the FRET archive, tagged with 'Active_single', are taken into consideration.

$$\begin{equation}
^iS_{app} = \frac{I_{Aem|Dex}^{Fret} + I_{Dem|Dex}^{Fret}}{I_{Aem|Dex}^{Fret} + I_{Dem|Dex}^{Fret} + I_{Aem|Aex}^{Fret}}
\end{equation}$$

$$\begin{equation}
^iE_{app} = \frac{I_{Aem|Dex}^{Fret}}{I_{Aem|Dex}^{Fret} + I_{Dem|Dex}^{Fret}}
\end{equation}$$

Next, display the calculated <sup>i</sup>S<sub>app</sub> and <sup>i</sup>E<sub>app</sub> in the bubble chart widget in rover. Open the bubble chart on the main dashboard, switch to the coding tab (<>) and paste the Python code that can be downloaded in [script 9](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script9_plottingiEappiSapp.python). Run by pressing the refresh button and move back to the bubble plot icon tab to show the plot that should be similar to the one below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img12.png' width='250'></div>

##### Trace-wise Background Correction
The first correction that is applied to the dataset is a background correction. In this trace-wise process the mean fluorophore intensity after fluorophore bleaching is considered to be the background signal and is subtracted from the calculated fluorophore intensity. This is done for each intensity measurement (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>) separately and in a trace-wise manner. Note that to calculate the correction value only traces from the FRET archive are taken into consideration. The values of the corrected I parameters are stored in the archive as <sup>ii</sup>I<sub>aemaex</sub>, <sup>ii</sup>I<sub>demdex</sub> & <sup>ii</sup>I<sub>aemdex</sub> respectively. To do this, download [script 5](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script5_ArchivewiseBackgroundCorrection.groovy) and run in the Fiji script editor.

##### Correction for Leakage ($\alpha$) and Direct excitation ($\delta$)
In this step two data corrections are carried out: a correction for leakage, the process of donor emission in the acceptor detection channel, and a correction for direct excitation, the process of acceptor emission by direct excitation of the acceptor at the donor wavelength. Both lead to signal intensity distortions when uncorrected and would influence the measured FRET parameters. Therefore they are corrected in this part of the analysis procedure. For more information about these correction parameters the reader is referred to literature<sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>.

The leakage ($\alpha$) and direct excitation ($\delta$) correction factors can be calculated using the formulae below. As indicated, these formulae require the calculated fluorescence intensities from the DO and AO populations respectively. These are calculated using [script 6](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script6_alpha_delta_correction.groovy) from the data that was collected in the DO and AO archives previously. Subsequently, they are implemented in the latter three formulae to find the corrected E and S values. Please download the script and run on the archive using the Fiji script editor. The $\alpha$ and $\delta$ correction factors as well as the calculated F<sub>A|D</sub>, <sup>iii</sup>E<sub>app</sub><sup>(DO)</sup>, and <sup>iii</sup>S<sub>app</sub><sup>(AO)</sup> values will appear as parameters in the archive.


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

##### Correction for Excitation ($\beta$) and Detection factors ($\gamma$)

The last data correction step corrects for excitation ($\beta$), normalizing excitation intensities and cross-sections of both acceptor and donor, and detection factors ($\gamma$), normalizing effective fluorescence quantum yields and detection efficiencies of both donor and acceptor. To find both global correction factors a linear regression analysis is performed with the following formulae:


$$\begin{equation}  

^{iii}S_{app}^{(FRET)} = \frac{1}{1 + \gamma * \beta + (1-\gamma)*\beta *^{iii}E_{app}^{(FRET)}}
\end{equation}$$

$$\begin{equation}  
\frac{1}{^{iii}S_{app}^{(FRET)}} = 1 + \gamma* \beta + (1-\gamma)*\beta *^{iii}E_{app}^{(FRET)} = a * ^{iii}E_{app}^{(FRET)} + b

\end{equation}$$

From this equation the dependencies of $\beta$ and $\gamma$ on a and b can be deducted to:


$$\begin{equation}
\beta = a + b + 1
       \quad\mathrm{and}\quad
\gamma = \frac{b - 1}{a + b + 1}
\end{equation}$$

Once the values of $\beta$ and $\gamma$ have been calculated, the fully corrected E and S values can be calculated. To do so, first F<sub>D|D</sub> and F<sub>A|A</sub> are calculated, then added to the archive, followed by the calculation of E and S. Download [script 7](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script7_beta_gamma_correction.groovy) and run on the archive to obtain these values. Note that these values are extremely sensitive to outliers and will have low significance when used on datasets with a low number of data points. Alternatively, an adjusted [script 7](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script7_fixed.groovy) is available in the repository where both a=1 and b=1 thereby effectively omitting this correction step.


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

---

#### <a name="9"></a> Plotting & Data Exploration in Python
To explore the data, open [script 10](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script10_plottingES.python) and copy the code to the scatterplot widget in the Rover dashboard. Run in 'Python' and obtain a scatterplot similar to the one below. In this plot the grey points refer to molecules in the 'FRET' archive, blue points to molecules in the 'DO' archive and red points to molecules in the 'AO' archive. To find the ensemble averages E and S for the population, please open [this Jupyter notebook](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Gaussian%20and%20Bimodal%20fit%20to%20find%20E%20-%20jpype.ipynb) to perform the Gaussian fit. Running the notebook requires the set-up of a new python environment. Directions to do so can be found in this [tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/). Alternatively, run [script 8](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/Full%20Analysis%20-%20lo%20follow%20along%20example/Scripts/Script8_Extract%20E%20and%20S%20values%20as%20a%20list.groovy) in Fiji, and copy the printed E and S value lists to [this Jupyter notebook](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_pipelines/FRET/210618%20-%20Gaussian%20and%20Bimodal%20fit%20to%20find%20E.ipynb). In the latter case no environment needs to be configured.  
_Note that the analysis of a single video from the dataset such as done in this example leads to a low number of data points. This affects the accuracy of the calculated correction factors and E and S values. The accuracy can be improved by analyzing all videos supplied in the repository corresponding to this dataset. The outcome of such an analysis is presented in the next paragraph._

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img13.png' width='450'></div>

The analysis of the E and S value distributions in Python show that the population average is obtained at E= 0.22 and S= 0.50. This is close to the expected value of E= 0.15 +/- 0.02 and S= 0.50 when considering the low number of data points and high error margin arising from that.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img25.png' width='350'></div>

<div style="text-align: center">
This image was reproduced from Hellenkamp et al. <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> with added annotations for comparison to the outcomes of the analysis presented in this example.
</div>


---

#### <a name="10"></a> Conclusion: Global Analysis Outcomes and Comparisons to Literature
The described analysis can be repeated for all provided videos for both the 1-lo and 1-mid sample datasets to obtain the E and S values more reliably for both samples. The outcome of such an extensive analysis can be found in the [repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_pipelines/FRET/Full%20Analysis%20-%20mid%20and%20lo%20combined) with all accompanying scripts and archives.
The final plot of this analysis shows that the FRET populations are observed at S= 0.54 with E-values of E= 0.23 (1-lo) and E= 0.56 (1-mid). This is very close to the expected values as published by Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img23.png' width='450'></div>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img25.png' width='350'></div>

<div style="text-align: center">
This image was reproduced from Hellenkamp et al. <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> with added annotations for comparison to the outcomes of the analysis presented in this example.
</div>

In conclusion, this example Mars pipeline showed that Mars is a software platform that is equipped to do a robust, transparent, traceable, and reliable data analysis for FRET experiments. This analysis outcome comparison to the benchmark study by Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> shows that very similar FRET values are obtained and that therefore Mars is an excellent tool to be used to analyze these type of datasets.

#### <a name="12"></a> Troubleshooting
This section highlights some of the common errors or problems that may occur during following along with the example as well as possible solutions. Please reference the table below in case you encounter any problems during the analysis. If your question has not been answered in this section, please feel free to [reach out](https://forum.image.sc/tag/mars) to our lab by making a forum post.

| Problem Description     | Solution     |
| :------------- | :------------- |
| Most parameters (E, S, iEapp, iSapp) display NaN values       | This most often happens because some of the metadata tags are missing. Solution: check in the metadata tab if the metadata records are tagged 'FRET', 'AO', and 'DO' respectively. Solution: add the correct tag to the metadata. This problem often arises when the archive has been tagged, but has not been saved, before merging using the 'merge archives' tool.     |
| No datapoints are obtained in the Rover bubble plot       | This indicates that either the plotting script did not run correctly or somewhere during the analysis no data was found or was interpreted wrongly yielding either no E and S values or values that are out of bound. Solution: first check if the plotting script ran correctly. Move to the 'log' tab in the plotting widget (the little book icon). If the script did not run correctly an error is displayed. Solve the error by adjusting the script in the script tab. Common mistakes include copy-paste errors or selection of the wrong language (needs to be Python). If this is not the answer to your problem, check in the archive whether the parameters 'iEapp', 'iSapp', 'E', and 'S' have reasonable values for the molecules that were tagged 'Active_single'. (1) In case NaN values are observed please take a look at the first problem-solution pair in this overview. (2) In case 'weird' values are observed for E and S either one of the correction steps or the peak integration steps are off. Check the parameter values of alpha, beta, gamma, and delta in the metadata parameters tab (should all be between 0 and 2). In case beta and gamma are not within boundaries, try to run script7_fixed instead of the normal script7. Else, verify if the correct settings have been used in the 'peak finder' and 'molecule integrator' tools. Refresh the plot after solving the issue.        |
| No peaks were found by the 'peak finder'       | This might be the  case when the 'peak finder' settings (especially threshold settings) are entered wrongly. Solution: verify that the correct 'peak finder' settings were entered in the dialog and run the analysis again.       |
| The ROI manager is empty after running the 'transform ROIs' tool       | This behavior can be caused by two issues: (1) there were no peaks placed in the ROI manager by the 'peak finder' or (2) the ROI manager was not closed or reset before doing running the transform ROIs tool on a second dataset. Solution: (1) check if the 'peak finder' settings are entered correctly and use the 'preview' option to verify that the tool finds the peaks. Check if the 'Add to ROI manager' option is selected in the menu. (2) close the ROI manager and run the analysis again, starting from the 'peak finder' command.     |
| The archive or script editor freezes during the analysis       | This occasionally happens, especially when Fiji has been active on your computer for a little longer. Solution: close Fiji and retry the analysis after a software restart.       |
| No I vs. T plots are observed after entering the x and y settings in the plotting menu       | Solution: refresh the plot by pressing the refresh button at the bottom right corner of the plot settings menu       |
| No output is generated after running a Mars tool       | Either the input settings were selected in such a way that no output is expected, or the tool encountered an error. Solution: (1) verify the settings of the tool. (2) Check for red printed errors in the Fiji Console window (Window>Console). Either resolve the error by selecting different settings in the Mars tool dialog window or run the tool again.    |
| No changes are made to the Archive after running one of the scripts       | It might be that the script encountered an error and did not run till completion. Solution: open the 'Console' window (Window>Console) in Fiji and see if any red errors are raised. If so, resolve the error and run the script again.       |




#### <a name="11"></a> References

(1) 	Roy, R.; Hohng, S.; Ha, T. A Practical Guide to Single-Molecule FRET. Nat. Methods 2008, 5 (6), 507–516. https://doi.org/10.1038/nmeth.1208.  
(2) 	Stryer, L.; Haugland, R. P. Energy Transfer: A Spectroscopic Ruler. Proc. Natl. Acad. Sci. U. S. A. 1967, 58 (2), 719–726. https://doi.org/10.1073/pnas.58.2.719.  
(3) 	Ha, T.; Enderle, T.; Ogletree, D. F.; Chemla, D. S.; Selvin, P. R.; Weiss, S. Probing the Interaction between Two Single Molecules: Fluorescence Resonance Energy Transfer between a Single Donor and a Single Acceptor. Proc. Natl. Acad. Sci. U. S. A. 1996, 93 (13), 6264–6268. https://doi.org/10.1073/pnas.93.13.6264.  
(4) 	Mekler, V.; Kortkhonjia, E.; Mukhopadhyay, J.; Knight, J.; Revyakin, A.; Kapanidis, A. N.; Niu, W.; Ebright, Y. W.; Levy, R.; Ebright, R. H. Structural Organization of Bacterial RNA Polymerase Holoenzyme and the RNA Polymerase-Promoter Open Complex. Cell 2002, 108 (5), 599–614. https://doi.org/10.1016/s0092-8674(02)00667-0.  
(5) 	Choi, U. B.; Strop, P.; Vrljic, M.; Chu, S.; Brunger, A. T.; Weninger, K. R. Single-Molecule FRET–Derived Model of the Synaptotagmin 1–SNARE Fusion Complex. Nat. Struct. Mol. Biol. 2010, 17 (3), 318–324. https://doi.org/10.1038/nsmb.1763.  
(6) 	Hellenkamp, B.; Wortmann, P.; Kandzia, F.; Zacharias, M.; Hugel, T. Multidomain Structure and Correlated Dynamics Determined by Self-Consistent FRET Networks. Nat. Methods 2017, 14 (2), 174–180. https://doi.org/10.1038/nmeth.4081.  
(7) 	Schuler, B.; Eaton, W. A. Protein Folding Studied by Single-Molecule FRET. Curr. Opin. Struct. Biol. 2008, 18 (1), 16–26. https://doi.org/10.1016/J.SBI.2007.12.003.  
(8) 	Colombo, S.; Broggi, S.; Collini, M.; D’Alfonso, L.; Chirico, G.; Martegani, E. Detection of CAMP and of PKA Activity in Saccharomyces Cerevisiae Single Cells Using Fluorescence Resonance Energy Transfer (FRET) Probes. Biochem. Biophys. Res. Commun. 2017, 487 (3), 594–599. https://doi.org/10.1016/J.BBRC.2017.04.097.  
(9) 	Hellenkamp, B.; Schmid, S.; Doroshenko, O.; Opanasyuk, O.; Kühnemuth, R.; Rezaei Adariani, S.; Ambrose, B.; Aznauryan, M.; Barth, A.; Birkedal, V.; et al. Precision and Accuracy of Single-Molecule FRET Measurements—a Multi-Laboratory Benchmark Study. Nat. Methods 2018, 15 (9), 669–676. https://doi.org/10.1038/s41592-018-0085-0.  
