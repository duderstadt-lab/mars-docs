---
layout: example
mathjax: true
title: Mars workflow for smFRET with no acceptor excitation
permalink: /examples/No_aex_FRET/index.html
---

The Mars [examples section](../../examples) provides complete workflows for both [static](../Static_FRET) and [dynamic](../Dynamic_FRET) smFRET datasets collected using alternating excitation (ALEX) that have two channels for each time point with red and green excitation, respectively. This not only provides FRET information during green excitation of the donor and distance dependent transfer to the acceptor, but also provides direct acceptor information during red pulses. This wealth of information offers many advantages for analysis as seen in the other smFRET examples and we highly recommend this collection strategy. However, there might be scenarios in which no direct acceptor excitation pulses are collected. This could be due to limitations of the setup used, a desire by the experimenter to extend the FRET lifetime, or perhaps to achieve higher temporal resolution. In fact, most FRET experiments up until recently were not collected with acceptor excitation information. In these cases, the smFRET workflow needs to be adjusted to calculate correction factors and the FRET efficiency distribution without relying on acceptor excitation information.

Below are the changes required to the [dynamic workflow example](../Dynamic_FRET) when analyzing datasets without direct acceptor excitation pulses. To test out these steps, we reused the Holliday junction [sample data](https://doi.org/10.5281/zenodo.6659531) from the [dynamic workflow example](../Dynamic_FRET) but ignored the acceptor only excitation information after the first steps. Some experimenters collect only a couple acceptor excitation pulses during the entire collection to periodically check the acceptor status. This information would only be used for visual inspection and plotting but not be sufficient for determination of correction factors.

---

#### <a name="reference"></a>Table of contents
- [The FRET Sample Design and Mars Analysis Process](#design)
- [Data preparation](#1)
- [Phase I: Locate molecules and integrate fluorescence](#2)
- [Phase II: Corrections, E calculation, kinetic analysis](#3)
- [Exploration in Python](#4)
- [Troubleshooting](#5)
- [References](#6)

---
#### <a name="design"></a> The FRET Sample Design and Mars Analysis Process
**Dataset Characteristics**  
The [dataset](https://doi.org/10.5281/zenodo.6659531) contains ten separate image sequences each representing a different field of view from the same sample. Imaging was conducted on a micro-mirror TIRF microscope with a dual view using [Micro-Manager 2.0](https://micro-manager.org/Version_2.0). The detection area of the camera is split in half, with each half displaying the signal from a different wavelength. In this way emission can be measured for two wavelengths at the same time and signal correlation is possible. In practice, for this dataset, this means that red emission is collected and shown on the top half, and the green emission on the bottom half of the image (figure 3). Furthermore, the dataset has two channels. One channel for red excitation (C=0) with a 637 nm laser and another for green excitation (C=1) with a 532 nm laser as denoted below by I<sub>emission|excitation</sub>. The intensities of peaks are integrated during analysis, used for corrections and the calculation of the efficiency (E) distribution. In this example, the red excitation channel (C=0) is only integrated for visual inspection purposes, but it not used for analysis, so this workflow also can be used for single channel datasets containing only donor excitation (532 nm in this case). Similarly, no stoichiometry (S) information will be calculated.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/holliday_junction_cartoon.png' width='600' /></div>

*Comment: Some microscopes are equipped with two cameras and collect FRET images simultaneously by splitting so that the donor signal goes to one camera and acceptor signal to the other. These types of datasets also can be analyzed with this Mars workflow. We suggest combining the videos collected by the two cameras side-by-side into a single artificial dualview. This can be accomplished very easily in Fiji by opening both videos and using the combine command under Image>Stacks>Tools>Combine. This approach depends on the two cameras collecting the same number of images at the same time. Once the two videos are combined into one, this workflow can be followed exactly. This approach also ensures proper registration between the cameras that might not be perfectly aligned with sub-pixel accuracy.*

**The Analysis Procedure**  
This example was developed in a modular fashion to illustrate how workflows can be built using Mars and to simplify the process of customization for processing other FRET datasets. The workflow has two major phases. In the first phase, displayed below on the left, the emission peaks from the molecules are located and fluorescence intensity is integrated to generate Molecule Archives for donor only (DO) and FRET populations. These are merged into one Molecule Archive that is used in _Phase II_, displayed on the right, where a sequence of [groovy scripts provided in the mars-tutorials repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts) are used to process the merged Molecule Archive. This includes identification of bleaching positions, scoring intensity traces, and calculating correction factors.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/no_aex_FRET_workflow_overview.png' width='1000'></div>

**_Phase I_: Locate molecules and integrate fluorescence**
   * Beam profile correct the image to account for differences in beam intensity across the field of view.
   * Identify the peaks (molecule locations) in the top and bottom regions.
   * Correlate the peaks in the top and bottom regions to find which molecules have only green emission (donor only, DO), or emission from both dyes (FRET).
   * Extract the intensity (I) vs. time (T) traces of all molecules and store this information in a Molecule Archive.
   * Tag the metadata record of each Molecule Archive with DO or FRET depending on the molecules selected and integrated.
   * Merge the Molecule Archives (DO and FRET) in preparation for _Phase II_.

**_Phase II_: Corrections, E calculation, kinetic analysis**
   * Run the [FRET workflow 1 add molecule tags](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_1_add_molecule_tags.groovy) groovy script. This script adds the tags on the metadata records (DO and FRET) to the corresponding molecule records.
   * Run the [FRET workflow 3 find bleaching positions](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_3_find_bleaching_positions.groovy) groovy script. This script adds the donor and acceptor bleaching positions (T) to the molecule records. Evaluate the intensity traces and add the Accepted tag to passing molecules.
   * Run the [FRET workflow 6 corrections without aex](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_6_corrections_without_aex.groovy) groovy script. This script calculates all correction factors to generate the corrected I vs. T traces as well as the FRET efficiency (E) distribution.
   * Run the [FRET workflow 5 two state fit](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_5_two_state_dwell_times.groovy) groovy script. This script adds a segments table making the high and low E states based using the threshold provided.

The template for a custom window containing buttons for all the commands and scripts used in the FRET workflows was made using the Fiji ActionBar plugin and is provided in the [mars-tutorials](https://github.com/duderstadt-lab/mars-tutorials.git) repository. Follow the steps in the [FRET Power Tools ActionBar tutorial]({{site.baseurl}}/tutorials/fretActionBar/) to setup this helpful button window in your local Fiji to simplify and speed up the workflow.

Final distributions and figures are generated using the [no acceptor excitation FRET example jupyter notebook](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/no_acceptor_excitation/no_acceptor_excitation_FRET_example.ipynb) provided in the mars-tutorials repository. This notebook requires the file path to the final Molecule Archive generated from the scripts above as input.

---
#### <a name="1"></a> Data preparation
**Opening the Video**  
First, [download](https://doi.org/10.5281/zenodo.6659531) the dataset from Zenodo. To follow along with this example, you will only need Pos0. The analysis procedure for the other fields of view is the same.

Make sure you have [Fiji with Mars installed](https://duderstadt-lab.github.io/mars-docs/install/). Open Fiji and make sure SCIFIO is used by default to open videos (Edit>Options>ImageJ2 tick the use SCIFIO box). Mars comes with a SCIFIO reader for Micro-Manager 2.0 that ensures all metadata is recovered using the Mars image processing commands. Open the video by dragging it from your file explorer to the Fiji bar or using File>Open... and selecting any image in the sequence. The video should look similar to the screenshot below. If you are trying this workflow with a different video, Mars accepts many other formats including videos opened using BioFormats. However, some metadata may not be recovered and this workflow would require some adaptation if a different collection strategy was used. If you run into trouble or have questions please make a post on the [Scientific Community Image Forum](https://forum.image.sc) with the mars tag.

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
| m<sub>02</sub>             | 1.01236       |
| m<sub>10</sub>             | 0 .000267             |
| m<sub>11</sub>             | 1.00312          |
| m<sub>12</sub>             | 507.21025          |

Table 1: Affine2D conversion matrix values. Transforms from the top (acceptor emission) to the bottom (donor emission).

More information about the calculation of this matrix can be found in the [Affine2D tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/)

---
### <a name="2"></a> Phase I: Locate molecules and integrate fluorescence
#### Beam profile correction
To correct the raw image for the non-uniform gaussian excitation profile the Mars [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) command is used. A z-projection (Fiji>image>stacks>Z project...) of the average intensity for 10 or more time points at the end of the video can be used to create the beam profile image. Typically, all molecules are bleached at the end of the video and only the beam profile remains. In this case, time points 900 to 910 were used and a median filter (Process>Filters>Median...) with a radius of 5 was applied to remove the signal from any remaining molecules. Do this separately for the individual Z projected images of each channel. If the Z Projection contains both channels, run Image>Duplicate... on each to get the individual images.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/aex_profile.png' width='350'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/dex_profile.png' width='350'></div>

<div style="text-align: center">
Figure 2: Beam profile images for acceptor (aex) and donor (dex) excitation, left and right, respectively. For this microscope there is more background in the top acceptor emission region for both donor and acceptor excitation.
</div>

Now, with the full example video that will be corrected selected, run the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) command (Plugins>Mars>Image>Util). Set the channel to 0 and the background image to the aex profile image you have just created. Run the command and you will see that the channel 0 has been corrected for the beam profile. Note that sometimes this difference is difficult to spot by eye. Repeat this step for channel 1 with the dex background beam profile image. For completeness we also corrected the acceptor excitation, however, this is not used for analysis in this example.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/profile_correction_aex.png' width='450'></div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/profile_correction_dex.png' width='450'></div>
<div style="text-align: center">
Figure 3: Dialog window of the Beam Profile Corrector command.
</div>

*Comments: DO NOT use the aex_profile and dex_profile images provided on Zenodo with the sample data. These should only be used when correcting the beam profile with the FRET workflow 2 profile correction groovy script as done in the [dynamic workflow example](../Dynamic_FRET). The background emission in the acceptor emission region was removed from the dex_profile image provided with the sample data and will therefore not be correct in this case where we also would like to correct the acceptor emission background on donor excitation directly.*

#### Find and integrate molecules
To calculate all FRET parameters and correction factors two populations in the sample are of interest: the donor only (DO) population and the FRET population. To facilitate easy data analysis an archive is created for each of these populations separately. These are then later on merged together while ensuring the metadata records for each of these archives is tagged appropriately so molecules are correctly tagged. These tags are used when calculating the correction factors.

All three archives are created following the same workflow: peak identification, ROI transformation to the other half of the split view, ROI filtering and finally molecule integration to obtain intensity vs. T traces in the Molecule Archive. Below, the workflow to create the FRET archive is shown with screenshots. The procedure is repeated with a few differences described for the creation of the DO Molecule Archive.

##### The FRET Archive
**Find the peaks in the acceptor region (C=1, top)**  
First, for a better fit performance, do a z-projection of the first 10 time points yielding an average intensity image (Fiji>image>stacks>Z project...). Use  this image to find the coordinates of the peaks.
To find the peaks in the top part of the split view in the acceptor emission from FRET first select this part of the screen with the box ROI tool and open the Peak Finder (Plugins>Mars>Image>Peak Finder).

Apply the settings as shown below and check if the peaks are identified correctly by pressing the preview button. If the correct settings are applied the peaks should have an identification marker (circle or point) on them. Press ok to apply the settings and add the ROIs of the identified peaks to the ROI Manager. The ROI Manager will open and will display the peaks listed with their UID.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/image_acceptor_peaks.png' width='550'></div>
<div style="text-align: center">
Identified peaks in the z-stack made from the first 10 frames of the original video.
</div>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Peak_Finder_Input.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Peak_Finder_find.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img7.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img8.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img9.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img10.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img11.png' width='450'></div>
<div style="text-align: center">
Output of the Peak Identifier tool: a list of ROIs in the Fiji ROI Manager.
</div>
<em>Note: We do not integrate the peaks using the [Peak Finder](https://duderstadt-lab.github.io/mars-docs/docs/image/PeakFinder/) command because this would only provide a table with the currently selected peaks. Instead we will use the Molecule Integrator (multiview) in a later step, which produces a Molecule Archive that contains the integrated signals from all peak populations.</em>

**Transform the ROIs to the bottom region of the dual view**  
Next, transform the peak ROIs to the bottom region of the split view. In this way, in the next analysis step, integration of the peak intensities will be possible at both emission wavelengths at the same time. The transformation is done with the [Transform ROIs](https://duderstadt-lab.github.io/mars-docs/docs/roi/TransformROIs/) command (Plugins>Mars>ROI>Transform ROIs). Use the Affine2D matrix as calculated before (see table 1). To ensure we only consider molecules with donor and acceptor signal, we apply the 'colocalize' filter so only peaks that are found in both acceptor (Red) and donor (Green) emission regions remain. Press ok to add the transformed ROIs to the ROI Manager. ROI names will have the format: UID_Red and UID_Green for emission from the same molecule in each region defined in the Output tab of the dialog.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Transform_ROIs_example_image.png' width='550'></div>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Transform_ROIs_Input.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Transform_ROIs_Colocalize.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Transform_ROIs_Output.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img15.png' width='450'></div>


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img18.png' width='450'></div>
<div style="text-align: center">
Output of the Transform ROI tool: a list of ROIs in the Fiji ROI manager with _Red and _Green names.
</div>

**Integrate molecules**  
Now switch to the full video (instead of the z projection) by clicking on its window. Then, integrate the peaks using the [Molecule Integrator (multiview)](https://duderstadt-lab.github.io/mars-docs/docs/image/MoleculeIntegratorMultiView/) command developed for multiview microscopy data (Plugins>Mars>Image>Molecule Integrator (multiview)). Press ok and a Molecule Archive with the results will open automatically upon completion of the calculation. Now the generation of the first archive, the FRET archive, is complete. Select the single metadata record in the Molecule Archive and add the FRET tag. This is required to ensure the population integrated is correctly annotated in the next steps. Save the Molecule Archive with the name FRET_Archive.yama.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/MoleculeIntegrator_multiview_Input_tab.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/MoleculeIntegrator_multiview_Boundaries_tab.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/MoleculeIntegrator_multiview_Integration_tab.png' width='450'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/MoleculeIntegrator_multiview_Output_tab.png' width='450'></div>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/FRET_Archive_with_tag.png' width='650'></div>
<div style="text-align: center">
Molecule Archive generated containing intensity information for all molecules with the FRET tag added to the metadata record.
</div>

The [Molecule Integrator (multiview)](https://duderstadt-lab.github.io/mars-docs/docs/image/MoleculeIntegratorMultiView/) command names the fluorescence intensity columns found in the table of each molecule record based on the channel (excitation) and region (emission) names with the format: channel_region. The region names used are specified in the Output tab of the [Transform ROIs](https://duderstadt-lab.github.io/mars-docs/docs/roi/TransformROIs/) dialog. The channel names are taken from the video metadata. For the sample data C=0 was named 637 and C=1 was named 532 for the excitation wavelengths used. If no channel names are found in the metadata, the channel indexes will be used as the names (0, 1, etc.). For completeness we integrated the acceptor emission upon direct excitation (637_Red). This information is included in the archive for visual inspection, but is not used for analysis in this example workflow.

| Intensity parameter     | Name in Mars     |
| :------------- | :------------- |
| I<sub>aemaex</sub>       | "637_Red" (corresponding to C=0, 637 excitation, Red emission)       |
| I<sub>aemdex</sub>       | "532_Red" (corresponding to C=1, 532 excitation, Red emission)       |
| I<sub>demdex</sub>       | "532_Green" (corresponding to C=1, 532 excitation, Green emission)       |

Table 2: Overview of definitions of intensity values with their corresponding names in the Molecule Archive.

##### The DO Archive
To create the donor only (DO) Archive, follow the procedure in the section 'The FRET Archive' with the following changes:
- **Peak Finder**: select the lower half of the screen, and select channel 1 in the dialog.
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/PeakFinder_DO_Input.png' width='350'></div>
- **Transform ROIs**: apply the inverse transformation, set channel to 1, check 'Remove colocalizing ROIs' and update the region names in the Output tab to reflect a transformation from Green to Red (bottom to top, which is the inverse of what we used when creating the FRET archive).
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/TransformROIs_DO_Input.png' width='350'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Transform_ROIs_DO_Colocalize.png' width='350'>
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/TransformROIs_DO_Output.png' width='350'></div>

Tag the single metadata record in the resulting Molecule Archive with DO and save it with the name DO_Archive.yama.

##### Merge Archives
The final step in _Phase I_ is to merge all the archives we created into one. This is required to use the scripts in _Phase II_ that expect DO and FRET tagged populations. Before merging the archives confirm that the metadata record in each archive is tagged with FRET or DO consistent with the steps above. If not, make sure to tag them now. All the archives (FRET_Archive.yama and DO_Archive.yama) need to be placed in the same folder. Next, use the Merge Archives command (Plugins>Mars>Molecule>Merge Archive) and select the folder. When the command is finished, a merged archive is created that can be found in the same folder (merged.yama).
*Note:* it is very important to save the archives after metadata tagging. Otherwise, the tag will not be in the merged archive and errors will be raised in the subsequent analysis steps.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/img31.png' width='350'></div>

---
### <a name="3"></a> Phase II: Corrections, E calculation, kinetic analysis
**1 add molecule tags**

Open the merged Molecule Archive created at the end of _Phase I_. This should contain three metadata records, one for each archive that was merged and they should have appropriate tags (FRET or DO). Run the [add molecule tags](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_1_add_molecule_tags.groovy) groovy script on the merged Molecule Archive. This script adds the tags on the metadata records to the corresponding molecule records. This can be confirmed by examining the tags on the records in the molecules tab. The rest of the scripts in the workflow require molecule tags.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/add_molecule_tags_metadata.png' width='650'></div>
<div style="text-align: center">
Input: Merged Molecule Archive from _Phase I_ containing three metadata records each with the appropriate tag.
</div>
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/add_molecule_tags_molecules.png' width='650'></div>
<div style="text-align: center">
Output: Updated merged Molecule Archive with all molecule records tagged to match their metadata records.
</div>

**3 find bleaching positions**

To correctly assess the FRET parameters, it is important to find out the bleaching time points of both dyes and to identify the first dye that bleached. Only the time points of the video up until that point are relevant for FRET. Find the bleaching positions using the [find bleaching positions](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_3_find_bleaching_positions.groovy) groovy script.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/find_bleaching_positions_dialog.png' width='450'></div>

As seen below, this adds the positions Bleach_Red and Bleach_Green to molecule records. To identify the bleaching positions, the script uses the [Single Change Point Finder](../../docs/kcp/SingleChangePointFinder/) command, which locates the single largest intensity transition within the respective intensity trace. The subsequent scripts only consider the region prior to the first bleach event, highlighted and labeled for clarity below, as the region where FRET occurred.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/bleaching_positions_example.png' width='450'></div>

<div style="text-align: center">
Representable molecule trace showing the raw intensities of Idemdex (blue line), and Iaemdex (gray line) with respect to the time point (T). The positions "Bleach_Red" and "Bleach_Green" show the frame at which bleaching of the respective dye occurred. The yellow highlighted region "FRET" indicates which time points should be considered when assessing the molecule's FRET parameters.
</div>

*Comments: In this example, the donor and acceptor bleach positions are both detected using the 532 (FRET) channel and their respective regions. This is in contrast to the [dynamic workflow example](../Dynamic_FRET) where emission from direct acceptor excitation (637_Red) is used to detect the acceptor bleach position. This may lead to more errors in bleach detection depending on the dataset. Therefore, detection using the emission from direct acceptor excitation (637_Red) is preferred. When the acceptor bleach position is detected using the 532 (FRET) channel the position marked is actually the end of FRET. This will only correspond to the true acceptor bleach position for molecule where the donor bleaches last. For molecules where the donor bleaches before the acceptor, the acceptor bleach position marked will only represent the end of FRET position using this approach.*

**Plot the traces and add the Accepted tag to passing molecules**  
Open the plot sub-tab in the molecules tab and add the line plots representing the three measured intensities (I<sub>demdex</sub> & I<sub>aemdex</sub>) for each molecule. To do so select the options as shown in the screenshot. The grey line represents I<sub>aemdex</sub> (FRET) and the blue line represents I<sub>demdex</sub> (DO). These are the two intensity vs. time traces that are required for further FRET calculations.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/archive_molecule_traces_example.png' width='950'></div>

Only molecules marked with the Accepted tag will be used for further analysis. Take care to ensure a consistent set of criteria are applied when marking Accepted molecule records. We used the following criteria for this example:
- There is only 1 donor and only 1 acceptor bleaching step observed in the molecule trace
- Both donor and acceptor are present (in the case of FRET-tagged molecules)
- No major deviations or changes in signal intensity are observed that could indicate an artefact
- The fluorescent signal is significantly higher than the noise of the background
- Donor and acceptor signals are anti-correlated in the FRET region
- There is sufficient FRET life time prior to the first dye bleach event

The selection of suitable DO molecules is important to ensure accurate determination of the alpha correction factor representing leakage of donor emission into the acceptor emission region. Longer lifetimes, high signal-to-noise, and low background are particularly important. The results of our classification can be directly examined in the [holliday_junction_merged_corrections_without_aex.yama and accompanying holliday_junction_merged_corrections_without_aex.yama.rover file](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/no_acceptor_excitation) final archive for this example that is available from the [mars-tutorials](https://github.com/duderstadt-lab/mars-tutorials/) repository.

The hot key combinations for molecule tagging can be set in the settings tab of the Molecule Archive. This speeds up the process of tagging significantly. Finally, a validation notebook provided in the [mars-tutorials](https://github.com/duderstadt-lab/mars-tutorials/) repository offers a report with several measures for the quality of the final set Accepted molecules.

**6 corrections without aex**

Once you have completed the analysis steps above, you are left with a Molecule Archive containing the results from the analysis of Pos0 from the Holliday junction dataset. To increase the number of observations, the workflow above should be repeated for additional positions (fields of view). We repeated the workflow for Pos1, Pos2, and Pos3. We then merged the Molecule Archives from all positions together for further analysis. Determination of accurate correction factors in this step depends on sufficient observations.

Run the [corrections without aex](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_6_corrections_without_aex.groovy) groovy script on the Molecule Archive with tagged Accepted molecules. This script calculates correction factors to generate corrected I vs. T traces as well as the FRET efficiency (E) distribution and many other outputs described below. This script can be re-run multiple times if molecule tags are updated or additional data is merged in.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/corrections_without_aex_dialog.png' width='500'></div>

The subsections below outline the various corrections performed by the [corrections without aex](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_6_corrections_without_aex.groovy) groovy script.

*Trace-wise background correction*  

The first correction that is applied to the dataset is a background correction. In this trace-wise process the mean fluorophore intensity after fluorophore bleaching is considered to be the background signal and is subtracted from the calculated fluorophore intensity. This is done for each intensity measurement (I<sub>demdex</sub> & I<sub>aemdex</sub>) separately and in a trace-wise manner. The values of the corrected I parameters are stored in the archive as <sup>ii</sup>I<sub>demdex</sub> & <sup>ii</sup>I<sub>aemdex</sub>.

*Correction for leakage ($\alpha$)*

The next correction is for leakage of donor emission into the acceptor detection region. The leakage ($\alpha$) factor can be calculated using the formulae below. As indicated, these formulae require the calculated fluorescence intensities from the DO population. The $\alpha$ correction factor as well as the calculated F<sub>A|D</sub>, <sup>iii</sup>E<sub>app</sub> and <sup>(DO)</sup> values will appear as parameters in the archive.


$$\begin{equation}
\alpha = \frac{\langle ^{ii}E_{app}^{(DO)} \rangle}{1 - \langle   ^{ii}E_{app}^{(DO)} \rangle}
   \quad\mathrm{and}\quad  
F_{A|D}=^{ii}I_{Aem|Dex} - \alpha ^{ii}I_{Dem|Dex}
\end{equation}$$

$$\begin{equation}  
^{iii}E_{app} = \frac{F_{A|D}}{F_{A|D} + ^{ii}I_{Dem|Dex}}
\end{equation}$$

*Note: For consistency with the other examples, we are using the nomenclature of [Hellenkamp et al.](https://doi.org/10.1038/s41592-018-0085-0) that defines donor leakage into the acceptor region as $\alpha$. The same leakage factor is defined as $\beta$ in [McCann et al.](https://doi.org/10.1016/j.bpj.2010.04.063) and other studies.*

*Detection correction ($\gamma$)*

Finally, we correct for differences in detection using $\gamma$, which normalizes the effective fluorescence quantum yields and detection efficiencies of both donor and acceptor. Without direct acceptor excitation, $\gamma$ can be calculated using FRET traces where the acceptor bleaches before the donor as described in [McCann _et al._](https://doi.org/10.1016/j.bpj.2010.04.063) by taking the ratio of the change in intensities before (_Pre_) and after (_Post_) acceptor photobleaching.

$$\begin{equation}
\gamma = \frac{^{A}I_{Pre} - ^{A}I_{Post}}{^{D}I_{Post} - ^{D}I_{Pre}}
\end{equation}$$

Here $^{A}I_{Pre}$ is the mean acceptor intensity before photobleaching, $^{A}I_{Post}$ is the mean backgound of the acceptor spot after photobleaching, $^{D}I_{Pre}$ is the intensity of the donor before acceptor photobleaching, and $^{D}I_{Post}$ is the intensity of the donor after acceptor photobleaching before donor photobleaching (the donor recovery period). The $\gamma$ factor is then used to determine the corrected donor emission F<sub>D|D</sub> and the fully corrected efficiency (E).

$$\begin{equation}
F_{D|D} = \gamma * ^{ii}I_{Dem|Dex}
   \quad\mathrm{and}\quad   
E = \frac{F_{A|D}}{F_{A|D} + F_{D|D}}
\end{equation}$$

**Molecule table columns**

*Note: The direct acceptor excitation channel (C=0) was integrated generating the 637_Red columns for visual inspection. These columns were not used for the correction factor or efficiency calculations. They are not required for this example workflow in case your video does not contain this information.*

| Molecule table column | Description    | Channel | Region | Added by |
| :------------- | :------------- | :------------- | :------------- | :------------- |
| T | OME time points | all | all | Molecule Integrator (multiview) |
| 637\_Red\_Time\_(s) | Time in seconds if discovered in the image metadata | 637 | Red | Molecule Integrator (multiview) |
| 637_Red_X | X coordinate of molecule position | 637 | Red | Molecule Integrator (multiview) |
| 637_Red_Y | Y coordinate of molecule position | 637 | Red | Molecule Integrator (multiview) |
| 637_Red | Local median background corrected integrated intensity |  637 | Red | Molecule Integrator (multiview) |
| 637_Red_Median_Background | Local median background intensity |  637 | Red | Molecule Integrator (multiview) |
| 637_Red_Uncorrected | Raw integrated intensity with no background correction |  637 | Red | Molecule Integrator (multiview) _Verbose mode_ |
| 637_Red_Mean_Background | Local mean background intensity | 637 | Red | Molecule Integrator (multiview) _Verbose mode_ |
| 532\_Red\_Time\_(s) | Time in seconds if discovered in the image metadata | 637 | Red | Molecule Integrator (multiview) |
| 532_Red_X | X coordinate of molecule position | 532 | Red | Molecule Integrator (multiview) |
| 532_Red_Y | Y coordinate of molecule position | 532 | Red | Molecule Integrator (multiview) |
| 532_Red | Local median background corrected integrated intensity |  532 | Red | Molecule Integrator (multiview) |
| 532_Red_Median_Background | Local median background intensity | 532 | Red | Molecule Integrator (multiview) |
| 532_Red_Uncorrected | Raw integrated intensity with no background correction |  532 | Red | Molecule Integrator (multiview) _Verbose mode_ |
| 532_Red_Mean_Background | Local mean background intensity | 532 | Red | Molecule Integrator (multiview) _Verbose mode_ |
| 532\_Green\_Time\_(s) | Time in seconds if discovered in the image metadata | 532 | Green | Molecule Integrator (multiview) |
| 532_Green_X | X coordinate of molecule position | 532 | Green | Molecule Integrator (multiview) |
| 532_Green_Y | Y coordinate of molecule position | 532 | Green | Molecule Integrator (multiview) |
| 532_Green | Local median background corrected integrated intensity | 532 | Green | Molecule Integrator (multiview) |
| 532_Green_Median_Background | Local median background intensity | 532 | Green | Molecule Integrator (multiview) |
| 532_Green_Uncorrected | Raw integrated intensity with no background correction |  532 | Green | Molecule Integrator (multiview) _Verbose mode_ |
| 532_Green_Mean_Background | Local mean background intensity | 532 | Green | Molecule Integrator (multiview) _Verbose mode_ |
| iiIdemdex | Donor emission after subtracting mean background after photobleaching | 532 | Green | 6 corrections without aex |
| iiIaemdex | Acceptor emission from FRET after subtracting mean background after photobleaching | 532 | Red | 6 corrections without aex |
| iiEapp ($E_{PR}$) | FRET efficiency after correcting for background after photobleaching | - | - | 6 corrections without aex |
| FAD | Acceptor emission from FRET corrected for background, leakage ($\alpha$) | 532 | Red | 6 corrections without aex |
| FDD | Donor emission after correcting for background, excitation and detection ($\gamma$) factor | 532 | Green | 6 corrections without aex |
| SUM_Dex | Sum of acceptor and donor emission during FRET (FAD + FDD). Expected to be stable in FRET region. | - | - | 6 corrections without aex |
| E | Fully corrected FRET efficiency (background, $\alpha$, $\gamma$) | - | - | 6 corrections without aex |

**5 two state fit**

Finally, run the [two state fit](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_5_two_state_dwell_times.groovy) groovy script to identify the dwells times for each state. This very simple script identifies regions with high and low FRET (E) based using the columns and threshold value provided in the dialog. This should be the fully corrected E values and the time in seconds.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/FRET_workflow_5_two_state_fit_dialog.png' width='450'></div>

The dwell time information is added to each molecule record as a segments table that shows up in a new tab. This table has the starting and ending coordinates for the segments. The script calculates the mean E value for each state and assigns all segments with that value (A column). The slope of the segments is zero in this case (B column). This column is used for kinetic analysis of [tracking results](../track-position-on-DNA/).

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/FRET_workflow_5_segments_table.png' width='850'></div>

The segments can be viewed in the molecule plot tab by checking the segments box in the plot options panel. The segments will only be displayed if the X and Y columns match those used to generate the segments table. The mean lifetime of each state is determined by fitting the distribution of dwell times for each state with exponential decay function. This is done in Python in the [jupyter notebook](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/no_acceptor_excitation/no_acceptor_excitation_FRET_example.ipynb) accompanying this example.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/dynamic/FRET_workflow_5_segments_plotted.png' width='850'></div>

*Comments: The [two state fit](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts/FRET_workflow_5_two_state_dwell_times.groovy) script uses a very simple model that leverages the pre-existing information we have about the system used in this example. The [kinetic change point (KCP)](../../docs/kcp/ChangePointFinder/) command provides a more general option for kinetic analysis that makes no assumptions about the number of states or their E values. The timescale of FRET transitions should be carefully considered. The KCP command, as well as other kinetic analysis tools, provide the best results when the lifetime of FRET states is much longer than the sampling rate. This ensures the FRET states are well resolved over the background noise. The transition rates between FRET states can also be determined using hidden Markov models. Currently, there is no HHM command integrated into Mars. Our suggestion would be to use the python package [pomegranate](https://pomegranate.readthedocs.io/en/latest/) in a jupyter notebook. The results can be added in a segments table and the rate constants as parameters to Mars records. We are working on an example notebook for hidden Markov models. Follow this [issue](https://github.com/duderstadt-lab/mars-tutorials/issues/1) for details. Comment on the issue if you have an urgent need for this.*  

---

#### <a name="4"></a> Exploration in Python

The final Molecule Archive from the steps above is available in the [mars-tutorials](https://github.com/duderstadt-lab/mars-tutorials/) repository in the files [holliday_junction_merged_corrections_without_aex.yama and accompanying holliday_junction_merged_corrections_without_aex.yama.rover file](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/no_acceptor_excitation).

Final distributions are generated using the [no acceptor excitation FRET example jupyter notebook](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/no_acceptor_excitation/no_acceptor_excitation_FRET_example.ipynb) provided in the [mars-tutorials repository](https://github.com/duderstadt-lab/mars-tutorials). This notebook requires the file path to a local copy of [holliday_junction_merged_corrections_without_aex.yama](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/no_acceptor_excitation) and Fiji as inputs. Please note that running the notebook requires the set-up of a new python environment. Directions to do so can be found in this [tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/). After running all cells several charts are generated showing efficiency (E) distribution together with the results of kinetic analysis.  

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/fret/no_aex_dynamic/Final_E_and_kinetics_charts.png' width='800'></div>

#### <a name="5"></a> Troubleshooting
This section highlights some of the common errors or problems that may occur during following along with the example as well as possible solutions. Please reference the table below in case you encounter any problems during the analysis. If your question has not been answered in this section, please feel free to [reach out](https://forum.image.sc/tag/mars) to our lab by making a forum post.

| Problem Description     | Solution     |
| :------------- | :------------- |
| Most parameters (E, S, iEapp, iSapp) display NaN values       | This most often happens because some of the metadata tags are missing. Solution: check in the metadata tab if the metadata records are tagged 'FRET', 'AO', and 'DO' respectively. Solution: add the correct tag to the metadata. This problem often arises when the archive has been tagged, but has not been saved, before merging using the Merge Archives command.     |
| No peaks were found by the Peak Finder command       | This might be the case when the Peak Finder settings (especially threshold settings) are not correct. Solution: verify that the correct Peak Finder settings were entered in the dialog and run the analysis again.       |
| The ROI manager is empty after running the Transform ROIs command       | This behavior can be caused by two issues: (1) there were no peaks placed in the ROI Manager by the Peak Finder or (2) the ROI Manager was not closed or reset before doing running the transform ROIs command on a second dataset. Solution: (1) check if the Peak Finder settings are entered correctly and use the 'preview' option to verify that the command finds the peaks. Check if the 'Add to ROI Manager' option is selected in the tab. (2) close the ROI Manager and run the analysis again, starting from the Peak Finder command.     |
| The archive or script editor freezes during analysis       | We have observed this during develop on rare occasions, especially when the same Fiji session has been active on your computer for a long time. Solution: close Fiji and retry the analysis after a software restart. Saving several copies of the archive during processing is best practive.       |
| No I vs. T plots are observed after entering the x and y settings in the plotting menu       | Solution: refresh the plot by pressing the refresh button at the bottom right corner of the plot settings menu       |
| No output is generated after running a Mars tool       | Either the input settings were selected in such a way that no output is expected, or the tool encountered an error. Solution: (1) verify the settings of the tool. (2) Check for red printed errors in the Fiji Console window (Window>Console). Either resolve the error by selecting different settings in the Mars tool dialog window or run the tool again.    |
| No changes are made to the Archive after running one of the scripts       | It might be that the script encountered an error and did not run till completion. Solution: open the 'Console' window (Window>Console) in Fiji and see if any red errors are raised. If so, resolve the error and run the script again.       |

#### <a name="6"></a> References

(1) 	Hyeon C, Lee J, Yoon J, Hohng S, Thirumalai D. Hidden complexity in the isomerization dynamics of Holliday junctions. Nat Chem. 2012 Nov;4(11):907-14. [https://doi.org/10.1038/nchem.1463](https://doi.org/10.1038/nchem.1463).  
