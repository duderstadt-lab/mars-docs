---
layout: example
mathjax: true
title: smFRET dataset analysis of a dynamic sample with Mars
permalink: /examples/FRET_dynamic/index.html
---

In this example, a typical Mars workflow to analyse a dynamic TIRF smFRET (single-molecule Förster Resonance Energy Transfer)<sup>[1](https://doi.org/10.1038/nmeth.1208)</sup> dataset is presented. This example is based on holliday junction DNA functionalized with two fluorophores <sup>[2](https://www.nature.com/articles/nchem.1463)</sup>. Due to  Mg<sup>2+</sup>-induced switching events, the structure of these holliday junction switches between two states, both with a different FRET efficiency between both dyes.
Analogously, the procedure highlighted in this example can be extended to other experiments where two states are visited. Examples of this include: enzyme binding events, conformational changes of protein or DNA, and interaction studies.

Please also visit the [Mars workflow to analyze a static TIRF smFRET dataset](https://duderstadt-lab.github.io/mars-docs/examples/FRET/).

---

#### <a name="reference"></a>Table of contents
- [The FRET Sample Design and Mars Analysis Process](#design)
- [Data preparation](#1)
- [Localization of Peaks and Intensity vs. T traces](#3)
- [Data Analysis and Corrections](#4)
- [Plotting & Data Exploration in Python](#9)
- [Conclusion: Global Analysis Outcomes and Comparisons to Literature](#10)
- [References](#11)
- [Troubleshooting](#12)

---
#### <a name="design"></a> The FRET Sample Design and Mars Analysis Process
**Sample Design**  

**Dataset Characteristics**  
The dataset provided was recorded with a TIRF microscope setup equipped with dual view collection. The detection area of the camera is split in half, each half displaying the signal after a different wavelength filter. In this way emission can be measured for two wavelengths at the same time and signal correlation is possible. In practise, for this dataset, this means that red emission is collected and shown on the top half, and the green emission on the bottom half of the window (figure 3). The excitation color alternates between red (C=0) and green (C=1) such that the different channels and split orientation give the peak intensities as denoted below each half by I<sub>emission|excitation</sub>. These intensities are integrated during analysis and after correction lead to the final E and S values.

**The Analysis Procedure**  
1. Open the video, perform basic corrections on the raw video data.
2. Identify the peaks (molecule locations) in both colors.
3. Correlate the peaks in the two colors to find which molecules have a red dye only (acceptor only, AO), green dye only (donor only, DO), or both dyes (FRET).
4. Extract the intensity (I) vs. time (T) traces of all dyes on each molecules and store this information in a Molecule Archive.
5. Normalize the intensity data and correct for photo-based effects.
6. In the fully corrected I vs. T traces, identify the different states a molecule possesses, also identify when (on of) the dyes are bleached.
7. Calculate the FRET efficiency (E) and stoichiometry (S) for each state of each molecule.
8. Create plots of all E and S values, perform Gaussian fits and find the FRET states the molecule possesses in this experiment.

Please visit the [documentation](https://duderstadt-lab.github.io/mars-docs/docs/) pages for specific information about the used tools.

---
#### <a name="1"></a> Data preparation
**Opening the Video**  
First, [download]() the dataset from our tutorials repository. To follow along with this example, we will only analyze part of the data (position 0). Also download the other files in the folder. These will be used to do corrections.

In Fiji, turn on SciFIO (Edit>Options>ImageJ2 tick the box). Then open the video by dragging it from your file explorer to the Fiji bar. The video should look similar to the screenshot in figure 4.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img1.png' width='450'></div>

<div style="text-align: center">
Figure 4: Screenshot of the dataset. In the top half of the split view the red emission is shown, in the bottom half the green emission. Note that the excitation channel can be switched with the "c" scrollbar below the image. Currently, C=0 indicating that the red laser was used for excitation of this frame.
</div>

**Video Correction**   
In most single-molecule microscopes, the excitation beam generates a gaussian profile across the image. This hinders tracking and fluorescence integration algorithms. Therefore this profile is corrected by dividing all frames by a beam profile image. To do so the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) tool is used.

First, the beam profile image must be created. For this, frame 988 of channel 0 and channel 1 are used. To retrieve this image use Image>Duplicate... and select only frame 988. After completion, a single image should appear which is a copy of the respective frame. The image still contains many protein peaks that must be removed to build the representative image. This can be done by applying a gaussian blur with a radius of 40 (Process>Filters>Gaussian blur). The resulting images will look similar to the ones shown below. Alternatively, [download]() these beam profile images from our repository. Save the images in a way that it is clear which one is red, which one is green.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img2.png' width='250'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img3.png' width='250'></div>

<div style="text-align: center">
Figure 2: Examples of a beam profile created from frame 988 of this dataset. A Gaussian blur with a radius of 40 was applied.
</div>


Now, run the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) command (Plugins>Mars>Image>Util). Make sure the full example video and the beam profile image are open in Fiji. Set the Channel to 0 and the background image to the red profile image you have just created. Run the command and you will see that the protein channel has been corrected for the beam profile. Note that sometimes this difference is difficult to spot by eye. Repeat this step for channel 1 with the green background beam profile image.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img_beam.png' width='450'></div>
<div style="text-align: center">
Figure 3: Dialog window of the Beam Profile Corrector tool.
</div>

**Affine2D matrix**   
To correlate the coordinates in the top half of the split view with those of the bottom half of the split view, we use a transformation matrix (Affine2D matrix).
For this dataset, the following matrix will be used:  

|                 | Left to Right matrix     |
| :-------------  | :------------- |
| m<sub>00</sub>             | 1.00276          |
| m<sub>01</sub>             | 0.000208              |
| m<sub>02</sub>             | 1.71236       |
| m<sub>10</sub>             | 0 .000267             |
| m<sub>11</sub>             | 1.00312          |
| m<sub>12</sub>             | 506.91025          |

Table 1: Affine2D conversion matrix values

More information about the calculation of this matrix can be found in the [Affine2D tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/)

---
#### <a name="3"></a> Localization of Peaks and Intensity vs. T traces
To calculate all FRET parameters and correction factors three different populations in the sample are of interest: the acceptor only (AO) population, the donor only (DO) population and the FRET population. To facilitate easy data analysis an archive is created for each of these populations separately. These are then later on merged together while retaining information on to which population the identified molecule belongs, information that is required to do the correction factor calculations.

All three archives are created following the same workflow: peak identification, ROI transformation to the other halve of the split view, ROI filtering and finally molecule integration to obtain intensity vs. T traces in an Archive. Below, the workflow to create the FRET archive is shown accompanied by screenshots. Please reference table 3 to repeat the archive creation also for the AO and DO archives.

##### The FRET Archive
**1. Identify the red peaks in the red channel (C=0, top)**  
First, for a better fit performance, do a z-projection of the first 10 frames yielding an average image (Fiji>image>stacks>Z project...). Use  this image to find the coordinates of the peaks.
To find the peaks in the top part of the split view in the red channel first select this part of the screen with the box ROI tool and open the Peak Finder (Plugins>Mars>Image>Peak Finder).


Apply the settings as shown below and check if the peaks are identified correctly by pressing the preview button. If the correct settings are applied the peaks should have an identification marker (circle or point) on them. Press ok to apply the settings and add the ROIs of the identified peaks to the ROI manager. The ROI manager will open and will display the peaks listed by their UID in the manager.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img4.png' width='450'></div>
<div style="text-align: center">
Figure 4: Identified peaks in the z-stack made from the first 10 frames of the original video.
</div>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img5.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img6.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img7.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img8.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img9.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img10.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img11.png' width='450'></div>
<div style="text-align: center">
Figure 4: Output of the Peak Identifier tool: a list of ROIs in the Fiji ROI manager.
</div>

**2. Transform  the ROIs to the right part of the split view**  
Next, transform the coordinates (ROIs) of the peaks that were just identified to match the bottom part of the split view. In this way, in the next analysis step, integration of the peak intensity is possible at both emission wavelengths at the same time. Transform the ROIs to the right part of the split view by using the Transform ROI tool (Plugins>Mars>ROI>Transform ROIs). Use the Affine2D matrix as calculated before (see table 1). Provide the split view dimensions as shown in the screenshot and apply the 'colocalize' filter to only include peaks that are found both in red and green. Press ok to find the transformed ROIs in the ROI manager as UUID_short and UUID_long entries.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img12.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img13.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img14.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img15.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img18.png' width='450'></div>
<div style="text-align: center">
Figure 5: Output of the Transform ROI tool: a list of ROIs in the Fiji ROI manager with _short and _long names.
</div>


**3. Apply the molecule integrator to extract I vs. T traces**  
Now first switch back to the video (instead of the z projection) by clicking on its window. Then, integrate the peaks using the molecule integrator tools developed for dual view microscopy data (Plugins>Mars>Image>Molecule Integrator (dualview)). Press ok and the archive will open automatically upon completion of the calculation. Now the generation of the first archive, the FRET archive, is complete. Tag the archive as "FRET" and save.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img19.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img20.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img21.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img22.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img23.png' width='450'></div>
<div style="text-align: center">
Figure 6: Output of the Molecule Integration tool: an archive with the intensity information for each identified molecule.
</div>

The molecule integrator automatically names the intensity traces according to the provided channel and wavelength color information. Please reference table 3 for an overview of the corresponding archive names to the intensity parameter names as used in the example.

| Intensity parameter     | Name in Mars     |
| :------------- | :------------- |
| I<sub>aemaex</sub>       | "0" (corresponding to C=0, red excitation, red emission)       |
| I<sub>aemdex</sub>       | "1 Red" (corresponding to C=1, green excitation, red emission)       |
| I<sub>demdex</sub>       | "1 Green" (corresponding to C=1, green excitation, green emission)       |


Table 2: Overview of definitions of intensity values with their corresponding names in the Molecule Archive.

##### The AO Archive
Follow the procedure exactly as written in the section 'The FRET Archive' with the only difference that in this case in the ROI Transform window the 'Remove colocalizing ROIs' should be set to true (see screenshot below). Tag the archive as "AO" and save.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img24.png' width='350'></div>

##### The DO Archive
Follow the procedure exactly as written in the section 'The FRET Archive' with the following changes:
- Peak finder: select the lower half of the screen, and select channel 1 in the dialog.
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img26.png' width='350'></div>
- Transform ROI: apply inverse transformation, set channel to 0, and choose "short to long" as the transformation direction. (see screenshots below)
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img27.png' width='350'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img28.png' width='350'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img29.png' width='350'></div>

Tag the archive as "DO" and save.


**Tag and Merge the Archives**  
To make the downstream processing procedure easier the next step is to tag the created archives accordingly and merge them to form one big archive. Use the tag function in the metadata tab to assign the tags 'FRET', 'AO' & 'DO' accordingly and save the archives in the same folder. Next select the merge archives tool (Plugins>Mars>Molecule>Merge Archive) and select the folder. When the command is finished a merged archive is created that can be found in the selected folder. Open the archive.
*Note:* it is very important to save the archive after tagging. Otherwise the tag will not be in the merged archive and errors will be raised in the subsequent analysis steps.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img31.png' width='250'></div>



**Plot the Traces to do a Visual Inspection**  
To do a visual inspection of the identified traces open the plot window in the molecules tab and add three line plots representing the three measured intensities (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>), if applicable, for each molecule. To do so select the options as shown in the screenshot.
In the screenshot below the red line represents I<sub>aemaex</sub>, the grey line I<sub>aemdex</sub> (FRET), and the blue line I<sub>demdex</sub>. These are the three intensity vs. time traces that are required to do further FRET calculations on.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img30.png' width='650'></div>



---


<sup>[1]</sup>(https://doi.org/10.1038/nmeth.1208)
<sup>[2]</sup>(https://www.nature.com/articles/nchem.1463)

---

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img14.png' width='450'></div>

<div style="text-align: center">
Figure 1:
</div>


---






---

---










**Plot the Traces to do a Visual Inspection**  
To do a visual inspection of the identified traces open the plot window in the molecules tab and add three line plots representing the three measured intensities (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>), if applicable, for each molecule. To do so select the options as shown in the screenshot.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img24.png' width='450'></div>

In the screenshot below the red line represents I<sub>aemaex</sub>, the grey line I<sub>aemdex</sub> (FRET), and the blue line I<sub>demdex</sub>. These are the three intensity vs. time traces that are required to do further FRET calculations on.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/img9.png' width='650'></div>

---

#### <a name="4"></a> Data Analysis and Corrections
Please download the [data analysis scripts](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/Follow%20along%20example) from the repository. This will automate all following steps and will eventually generate an archive containing the fully corrected E and S values for each relevant molecule. For a better understanding of the procedure, the parts of the script are discussed in detail below.

**Add the metadata tags to all corresponding molecule records**
For easier record handling, first, run [migrateTagsFromMetadataToMolecules.groovy](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/Follow%20along%20example/migrateTagsFromMetadataToMolecules.groovy) to make migrate all the assigned metadata tags (AO, DO, FRET) to the corresponding molecule records. This makes it easier to look at the data in a later stadium of the analysis.

**Do a position-specific excitation correction accounting for beam profile differences**
For reliable calculations, it is of importance that calculations are not distorted by differences in the beam intensity across the field (beam profile differences) or by differences between donor and acceptor excitation. To account for this, the data is first normalized according to equation 1. In short, the profile-corrected intensity is obtained by multiplying the uncorrected intensity by a normalized ratio between the donor and acceptor of the signal at a certain position. This correction term is calculated using the two calibration images supplied by Hellenkamp et al and correlates the donor and acceptor signal to one another while at the same time correcting for the beam profile of the microscope set-up.

$$\begin{equation}
_ {(profile)}^{i}I_{Aem|Dex} = ^{i}I_{Aem|Dex}* \frac{I_{D}(x',y')}{I_{A}(x,y)}
\end{equation}$$

To do so, open script [ProfileCorrection.groovy](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/Follow%20along%20example/ProfileCorrection.groovy) as well as the two calibration images rr_iMap and og_iMap that were supplied along with the video data from the Zenodo link. Also make sure to have the molecule archive created in the previous steps of this protocol opened. Then run the script. An additional column will be added to the data table for each molecule record where the corrected intensity I<sub>aexaem</sub> will be found named "0_Profile_Corrected".

**Identify the bleaching positions for donor and acceptor**
To identify the bleaching positions for both donor and acceptor in each trace, the single change point finder tool from Mars is applied. After running this tool, the positions of the bleaching events of both dyes will be added to the corresponding molecule record. This information can later on be used to filter molecule traces (make sure each trace has both 1 donor and 1 acceptor) and to do the FRET calculations obtaining E and S values for each molecule.

Start by opening the [findBleachingPositions.groovy](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/Follow%20along%20example/findBleachingPositions.groovy) script from the repository. This script automates the command and makes sure bleaching positions are found for DO, AO and FRET molecules in the colors relevant. Run the script. This will add bleaching positions to the archive shown as markers with dotted lines in the plots.

<<Figure>>

**Selecting the traces to be included in further analysis**
Before proceeding with the following part of automated analysis, it is required to inspect the traces manually and assign the tag 'Accepted' for all molecules fulfilling the requirements:
- There is only 1 donor and only 1 acceptor bleaching step observed in the molecule trace
- Both donor and receptor are present (in the case of FRET-tagged molecules)
- No major deviations or changes in signal intensity are observed that could indicate an artefact
- The fluorescent signal is significantly higher than the noise of the background
- No switching events are observed in the trace

Please go through all molecule plots in the archive and tag the molecules accordingly. For convenience, the [automated tagging feature in the cog tab](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/Settings/) of the archive might be used. Click on the link for more information on how to use this feature.



**Trace-wise Background Correction**
The first correction that is applied to the dataset is a background correction. In this trace-wise process the mean fluorophore intensity after fluorophore bleaching is considered to be the background signal and is subtracted from the calculated fluorophore intensity. This is done for each intensity measurement (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>) separately and in a trace-wise manner. The values of the corrected I parameters are stored in the archive as <sup>ii</sup>I<sub>aemaex</sub>, <sup>ii</sup>I<sub>demdex</sub> & <sup>ii</sup>I<sub>aemdex</sub> respectively.

To do these calculations on the archive, open the script [smFRET_workflow_1_profile_corrected.groovy](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/Follow%20along%20example/smFRET_workflow_1_profile_corrected.groovy) and run in the script editor. All calculated values discussed will be added to the archive.

**Correction for Leakage ($\alpha$) and Direct excitation ($\delta$)**
In this step two data corrections are carried out: a correction for leakage, the process of donor emission in the acceptor detection channel, and a correction for direct excitation, the process of acceptor emission by direct excitation of the acceptor at the donor wavelength. Both lead to signal intensity distortions when uncorrected and would influence the measured FRET parameters. Therefore they are corrected in this part of the analysis procedure. For more information about these correction parameters the reader is referred to literature<sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>.

The leakage ($\alpha$) and direct excitation ($\delta$) correction factors can be calculated using the formulae below. As indicated, these formulae require the calculated fluorescence intensities from the DO and AO populations respectively. Subsequently, they are implemented in the latter three formulae to find the corrected E and S values. The $\alpha$ and $\delta$ correction factors as well as the calculated F<sub>A|D</sub>, <sup>iii</sup>E<sub>app</sub><sup>(DO)</sup>, and <sup>iii</sup>S<sub>app</sub><sup>(AO)</sup> values will appear as parameters in the archive.


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

This correction has been done in the previously run script.

**Correction for Excitation ($\beta$) and Detection factors ($\gamma$)**
The last data correction step corrects for excitation ($\beta$), normalizing excitation intensities and cross-sections of both acceptor and donor, and detection factors ($\gamma$), normalizing effective fluorescence quantum yields and detection efficiencies of both donor and acceptor. To find both global correction factors a linear regression analysis is performed with the following formula. To do these calculations, the average E or S value respectively of each molecule is used to prevent unequal weighting of longer traces.


$$\begin{equation}  

^{iii}S_{app}^{(FRET)} = \frac{1}{1 + \gamma * \beta + (1-\gamma)*\beta *^{iii}E_{app}^{(FRET)}}
\end{equation}$$

$$\begin{equation}  
\frac{1}{^{iii}S_{app}^{(FRET)}} = 1 + \gamma* \beta + (1-\gamma)*\beta *^{iii}E_{app}^{(FRET)} = b * ^{iii}E_{app}^{(FRET)} + a

\end{equation}$$

From this equation the dependencies of $\beta$ and $\gamma$ on a and b can be deducted to:


$$\begin{equation}
\beta = a + b - 1
       \quad\mathrm{and}\quad
\gamma = \frac{a - 1}{a + b - 1}
\end{equation}$$

Once the values of $\beta$ and $\gamma$ have been calculated, the fully corrected E and S values can be calculated. To do so, first F<sub>D|D</sub> and F<sub>A|A</sub> are calculated, then added to the archive, followed by the calculation of E and S. Note that these values are extremely sensitive to outliers and will have low significance when used on datasets with a low number of data points.


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

To do these calculations on the archive, open the script [smFRET_workflow_2.groovy](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/Follow%20along%20example/smFRET_workflow_2.groovy) and run in the script editior. All calculated values discussed will be added to the archive.

---

#### <a name="9"></a> Plotting & Data Exploration in Python
To explore the data save the generated archive and open the [Jupyter notebook](https://github.com/duderstadt-lab/mars-tutorials). Set up the connection to your local copy of Fiji as indicated and change the file path to the archive. Please note that running the notebook requires the set-up of a new python environment. Directions to do so can be found in this [tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/). After running all cells a plot is obtained showing the fully corrected E and S values for all molecules that matched the selection criteria in this archive.  
_Note that the analysis of a single video from the dataset such as done in this example leads to a low number of data points. This affects the accuracy of the calculated correction factors and E and S values. On top of that, a reliable beta/gamma correction is possible only when the data consists of 2 populations at the minimum. Therefore the reliability of the outcome of this example is compromised. The accuracy can be improved by analyzing all videos supplied in the repository corresponding to this dataset. The outcome of such an analysis is presented in the next paragraph._

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/1-lo_analyzed.png' width='350'></div>

This analysis shows that an average population FRET value of E = 0.28 was obtained from this single dataset. Please note that this value is rather distorted because of the low number of data points and data available for correction. Also, since only one sample is taken into consideration the beta/gamma correction is distorting the results. Therefore it is not a good estimate for the FRET efficiency of the sample. Please continue to the following section for a better estimate considering all available videos.


---

#### <a name="10"></a> Conclusion: Global Analysis Outcomes and Comparisons to Literature
The described analysis can be repeated for all provided videos for both the 1-lo and 1-mid sample datasets to obtain the E and S values more reliably for both samples. The outcome of such an extensive analysis can be found in the [repository](https://github.com/duderstadt-lab/mars-tutorials) with all accompanying scripts and archives. Please note that in order to analyze all data a specific procedure needs to be followed. Please read the read-me file in the repository with further instructions.

The final plot of this analysis shows that the FRET populations are observed at E= 0.18 (1-lo) and E= 0.52 (1-mid). This is in agreement with the values as published by Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> who reported E-values of E = 0.15 +/- 0.02 (1-lo) and E = 0.56 +/- 0.03 (1-mid) respectively.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/Final_with_fits.png' width='450'></div>


In conclusion, this example Mars workflows showed that Mars is a software platform that is equipped to do a robust, transparent, traceable, and reliable data analysis for FRET experiments. Since the results of the analysis are in complete agreement with those of Hellenkamp *et al.* <sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup> this proves Mars is well-equipped to be used in the analysis of these type of datasets.


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
