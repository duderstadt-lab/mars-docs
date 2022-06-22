---
layout: example
mathjax: true
title: smFRET dataset analysis of a dynamic sample with Mars
permalink: /examples/Dynamic_FRET/index.html
---

This example presents a Mars workflow for the analysis of dynamic smFRET (single-molecule Förster Resonance Energy Transfer)<sup>[1](https://doi.org/10.1038/nmeth.1208)</sup> datasets collected using TIRF microscopy. The [sample data](https://doi.org/10.5281/zenodo.6659531) were collected using a surface-immobilized Holliday junction functionalized with donor and acceptor fluorophores positioned on different DNA arms <sup>[2](https://www.nature.com/articles/nchem.1463)</sup>. Imaging was conducted under high Mg<sup>2+</sup> concentrations to induce conformational switching of the DNA arms resulting in high and low FRET states.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/index/dynamic_FRET_example.png' width='900' /></div>

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
The [dataset](https://doi.org/10.5281/zenodo.6659531) contains ten separate image sequences each representing a different field of view from the same sample. Imaging was conducted on a micro-mirror TIRF microscope with a dual view using [Micro-Manager 2.0](https://micro-manager.org/Version_2.0). The detection area of the camera is split in half, with each half displaying the signal from a different wavelength. In this way emission can be measured for two wavelengths at the same time and signal correlation is possible. In practice, for this dataset, this means that red emission is collected and shown on the top half, and the green emission on the bottom half of the image (figure 3). Furthermore, the dataset has two channels. One channel for red excitation (C=0) with a 637 nm laser and another for green excitation (C=1) with a 532 nm laser as denoted below by I<sub>emission|excitation</sub>. The intensities of peaks are integrated during analysis, used for corrections and the calculation of final E and S distributions.

**The Analysis Procedure**  
This example was developed in a modular fashion to illustrate how workflows can be built using Mars and to simplify the process of customization for processing other FRET datasets. The workflow has two major phases. In the first phase, displayed below on the left, the emission peaks from the molecules are located and fluorescence intensity is integrated to generate Molecule Archives for donor only (DO), acceptor only (AO), and FRET populations. These are merged into one Molecule Archive that is used in phase two, displayed on the right, where a sequence of [groovy scripts provided in the mars-tutorials repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts) are used to process the merged Molecule Archive. This includes identification of bleaching positions, scoring intensity traces, and calculating correction factors.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/dynamic_FRET_workflow_overview.png' width='1000'></div>

**Phase 1: Locate molecules and integrate fluorescence**
   * Identify the peaks (molecule locations) in the top and bottom regions.
   * Correlate the peaks in the top and bottom regions to find which molecules have only red emission (acceptor only, AO), only green emission (donor only, DO), or emission from both dyes (FRET).
   * Extract the intensity (I) vs. time (T) traces of all molecules and store this information in a Molecule Archive.
   * Tag the metadata record of each Molecule Archive with DO, AO, or FRET depending on the molecules selected and integrated.
   * Merge all three of the Molecule Archives (DO, AO, and FRET) in preparation for phase 2.

**Phase 2: Corrections, E and S calculation, kinetic analysis**
1. Run the [add molecule tags](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_1_add_molecule_tags.groovy) groovy script. This script adds the tags on the metadata records (DO, AO, and FRET) to the corresponding molecule records.
2. Run the [profile correction](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_2_profile_correction.groovy) groovy script. This script corrects for differences in the beam intensity across the field of view.
3. Run the [find bleaching positions](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_3_find_bleaching_positions.groovy) groovy script. This script adds the donor and acceptor bleaching positions (T) to the molecule records. Evaluate the intensity traces and add the Accepted tag to passing molecules.
4. Run the [alex corrections](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_4_alex_corrections.groovy) groovy script. This script calculates all alex correction factors to generate the fully corrected I vs. T traces as well as the FRET efficiency (E) and stoichiometry (S) values.
5. Run the [two state fit](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_5_two_state_dwell_times.groovy) groovy script. This script adds a segments table making the high and low E states based using the threshold provided.

Final distributions and figures are then generated using the [dynamic FRET example jupyter notebook](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/dynamic/dynamic_FRET_example.ipynb) provided in the mars-tutorials repository. This notebook requires the file path to the final Molecule Archive generated from the scripts above as input.

---
#### <a name="1"></a> Data preparation
**Opening the Video**  
First, [download](https://doi.org/10.5281/zenodo.6659531) the dataset from Zenodo. To follow along with this example, you will only need to download Pos0 and the beam_profiles. The analysis procedure for the other fields of view is be the same.

Make sure you have [Fiji with Mars installed](https://duderstadt-lab.github.io/mars-docs/install/). Open Fiji and make sure SCIFIO is used by default to open videos (Edit>Options>ImageJ2 tick the use SCIFIO box). Mars comes with a SCIFIO reader for Micro-Manager 2.0 that ensures all metadata is recovered using the Mars image processing commands. Open the video by dragging it from your file explorer to the Fiji bar or using File>Open... and selecting any image in the sequence. The video should look similar to the screenshot in below. If you are trying this workflow with a different video, Mars accepts many other formats including videos opened using BioFormats. However, some metadata may not be recovered and this workflow would require some adaptation if a different collection strategy was used. If you run into trouble or have questions please make a post on the [Scientific Community Image Forum](https://forum.image.sc) with the mars tag.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img1.png' width='450'></div>

<div style="text-align: center">
Screenshot of the dataset. In the top half of the split view the red emission is shown, in the bottom half the green emission. Note that the excitation channel can be switched with the "c" scrollbar below the image. Currently, C=0 indicating that the red laser was used for excitation of this frame. <em>Note: ImageJ channel index starts at 1 whereas Mars channel index starts at 0.</em>
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

Table 1: Affine2D conversion matrix values. Transforms from the top (acceptor emission) to the bottom (donor emission).

More information about the calculation of this matrix can be found in the [Affine2D tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/)

---
#### <a name="3"></a> Localization of Peaks and Intensity vs. T traces
To calculate all FRET parameters and correction factors three different populations in the sample are of interest: the acceptor only (AO) population, the donor only (DO) population and the FRET population. To facilitate easy data analysis an archive is created for each of these populations separately. These are then later on merged together while ensuring the metadata records for each of these archives is tagged appropriately so molecules are correctly tagged. These tags are used when calculating the correction factors.

All three archives are created following the same workflow: peak identification, ROI transformation to the other halve of the split view, ROI filtering and finally molecule integration to obtain intensity vs. T traces in the Molecule Archive. Below, the workflow to create the FRET archive is shown accompanied by screenshots. Please reference table 3 to repeat the archive creation also for the AO and DO archives.

##### The FRET Archive
**Find the peaks in the red channel (C=0, top)**  
First, for a better fit performance, do a z-projection of the first 10 frames yielding an average image (Fiji>image>stacks>Z project...). Use  this image to find the coordinates of the peaks.
To find the peaks in the top part of the split view in the red channel first select this part of the screen with the box ROI tool and open the Peak Finder (Plugins>Mars>Image>Peak Finder).

Apply the settings as shown below and check if the peaks are identified correctly by pressing the preview button. If the correct settings are applied the peaks should have an identification marker (circle or point) on them. Press ok to apply the settings and add the ROIs of the identified peaks to the ROI manager. The ROI manager will open and will display the peaks listed by their UID in the manager.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img4.png' width='450'></div>
<div style="text-align: center">
Identified peaks in the z-stack made from the first 10 frames of the original video.
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
Output of the Peak Identifier tool: a list of ROIs in the Fiji ROI manager.
</div>
<em>Note: We do not integrate the peaks using the [Peak Finder](https://duderstadt-lab.github.io/mars-docs/docs/image/PeakFinder/) command because this would only provide a table with the currently selected peaks. Instead we will use the Molecule Integrator (multiview) in a later step, which produces a Molecule Archive that contains the integrated signals from all peak populations.</em>

**Transform  the ROIs to the right part of the split view**  
Next, transform the peak ROIs to the bottom region of the split view. In this way, in the next analysis step, integration of the peak intensities will be possible at both emission wavelengths at the same time. The transformation is done with the [Transform ROIs](https://duderstadt-lab.github.io/mars-docs/docs/roi/TransformROIs/) command (Plugins>Mars>ROI>Transform ROIs). Use the Affine2D matrix as calculated before (see table 1). To ensure we only consider molecules with donor and acceptor signal, we will switch to the FRET channel (C=1) and apply the 'colocalize' filter so only peaks that are found in both red and green regions remain. Press ok to add the transformed ROIs to the ROI manager. ROI names will have the format: UID_Red and UID_Green for emission from the same molecule in each region defined in the Output tab of the dialog.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/TransformROIs_FRET_preview.png' width='450'></div>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img12.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img13.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img14.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img15.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img18.png' width='450'></div>
<div style="text-align: center">
Output of the Transform ROI tool: a list of ROIs in the Fiji ROI manager with _Red and _Green names.
</div>

**Integrate molecules**  
Now first switch back to the video (instead of the z projection) by clicking on its window. Then, integrate the peaks using the [Molecule Integrator (multiview)]() command developed for dual view microscopy data (Plugins>Mars>Image>Molecule Integrator (dualview)). Press ok and the archive will open automatically upon completion of the calculation. Now the generation of the first archive, the FRET archive, is complete. Tag the archive as "FRET" and save.

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
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img30.png' width='650'></div>


#### <a name="4"></a> Data Analysis and Corrections
Please download the [data analysis scripts]() from the repository. This will automate all following steps and will eventually generate an archive containing the fully corrected E and S values for each relevant molecule. For a better understanding of the procedure, the parts of the script are discussed in detail below

**Identify the bleaching points for both dyes**  
In order to correctly assess the FRET parameters, it is important to find out the bleaching time points of both dyes and to identify the first dye that bleached. Only the frames of the video up until that point are relevant for possible FRET. As shown in figure 7, this would respectively give us the positions of Bleach_Red and Bleach_Green as well as the region of interest "FRET_region".

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img33.png' width='450'></div>

<div style="text-align: center">
Figure 7: Representable molecule trace showing the raw intensities of Iaemaex (red line), Idemdex (blue line), and Iaemdex (gray line) with respect to the frame number (T). The positions "Bleach_Red" and "Bleach_Green" show the frame at which bleaching of the respective dye occurred. The yellow highlighted region "FRET_region" indicates which frames should be considered when assessing the molecule's FRET parameters.
</div>

To identify these bleaching positions, the script uses a [kinetic change point algorithm](https://duderstadt-lab.github.io/mars-docs/docs/kcp/SingleChangePointFinder/) that finds the single largest intensity transition within the respective intensity trace. This position is then labelled according to the assessed color. Subsequently, a script assesses the values of Bleach_Red and Bleach_Green of the molecule and finds the bleaching step that occurred first. A MarsRegion is then defined that runs from T=0 until the first bleaching event. Only in this region FRET can occur.

**Identify the state transitions in the FRET region of each trace**  
To help visual inspection of the traces, another [kintetic change point algorithm](https://duderstadt-lab.github.io/mars-docs/docs/kcp/ChangePointFinder/) is used to identify the different intensity levels that are visited by the Iaemdex (FRET) trace in the frames that are part of the FRET_region. The results of this analysis are shown in the plot by short horizontal lines indicating possible FRET states between which switching occurred during the experiment.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img32.png' width='450'></div>

<div style="text-align: center">
Figure 8: Representable molecule trace showing the raw intensities of Iaemaex (red line), Idemdex (blue line), and Iaemdex (gray line) with respect to the frame number (T). The positions "Bleach_Red" and "Bleach_Green" show the frame at which bleaching of the respective dye occurred. The yellow highlighted region "FRET_region" indicates which frames should be considered when assessing the molecule's FRET parameters. In addition, the horizontal black lines on the gray trace, indicate intensity levels that were found in the FRET_region. These can be used to help identify FRET state switching.
</div>


#### Data Corrections
**Trace-wise Background Correction**  
The first correction that is applied to the dataset is a background correction. In this trace-wise process the mean fluorophore intensity after fluorophore bleaching is considered to be the background signal and is subtracted from the calculated fluorophore intensity. This is done for each intensity measurement (I<sub>aemaex</sub>, I<sub>demdex</sub> & I<sub>aemdex</sub>) separately and in a trace-wise manner. The values of the corrected I parameters are stored in the archive as <sup>ii</sup>I<sub>aemaex</sub>, <sup>ii</sup>I<sub>demdex</sub> & <sup>ii</sup>I<sub>aemdex</sub> respectively. The mean background intensities are stored as molecule parameters (532_Green_Background, 532_Red_Background, and 637_Red_Background).

**Correction for Leakage ($\alpha$) and Direct excitation ($\delta$)**  
In the next step, two data corrections are carried out: a correction for leakage, the process of donor emission in the acceptor detection channel, and a correction for direct excitation, the process of acceptor emission by direct excitation of the acceptor at the donor wavelength. Both lead to signal intensity distortions when uncorrected and would influence the measured FRET parameters. Therefore they are corrected in this part of the analysis procedure. For more information about these correction parameters the reader is referred to literature<sup>[9](https://doi.org/10.1038/s41592-018-0085-0)</sup>.

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

**Correction for Excitation ($\beta$) and Detection factors ($\gamma$)**  
The last data correction step corrects for excitation ($\beta$), normalizing excitation intensities and cross-sections of both acceptor and donor, and detection factors ($\gamma$), normalizing effective fluorescence quantum yields and detection efficiencies of both donor and acceptor. To find both global correction factors a linear regression analysis is performed with the following formula.
Note: Each value of iiiEapp and iiiSapp is included in the regression. This means that longer traces have a larger weight in the final fit since these traces contribute a larger number of data points to the dataset used to make a fit.

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

===
Section finished till here

---



**Selecting the traces to be included in further analysis**
Before proceeding with the following part of automated analysis, it is required to inspect the traces manually and assign the tag 'Accepted' for all molecules fulfilling the requirements:
- There is only 1 donor and only 1 acceptor bleaching step observed in the molecule trace
- Both donor and receptor are present (in the case of FRET-tagged molecules)
- No major deviations or changes in signal intensity are observed that could indicate an artefact
- The fluorescent signal is significantly higher than the noise of the background
- No switching events are observed in the trace





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
