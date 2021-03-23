---
layout: example
title: Track the Position of a Protein on a long DNA Molecule
permalink: /examples/track-position-on-DNA/index.html
---

DNA is one of the essential macromolecules throughout all domains of life. Many proteins interact with DNA, and while doing so move along the DNA molecule. Single molecule approaches can be used to study the kinetics and mechanisms of these processes. This example demonstrates a complete workflow from microscopy data to kinetic information about the enzyme studied on a single-molecule level.

#### <a name="reference"></a>Table of contents

- [Biological system - T7 RNA Polymerase movement on DNA](#biol)
- [Data analysis - The MARS workflow](#background)
- [Open the sample video with SCIFIO](#open)
- [Correct for the beam profile](#beam)
- [Track the proteins to create a Single Molecule Archive](#track)
- [Transform to overlay tracked proteins with the DNA channel](#transform)
- [Driftcorrection of the x and y coordinates ](#drift)
- [Find and fit the DNA molecules](#DNA)
- [Build a DNA Molecule Archive](#BuildDNA)
-	[Look at the Molecules in the Video](#look)
- [Conclusion](#conc)

#### <a name="biol"></a> Biological system - T7 RNA Polymerase movement on DNA
In this example the processivity of single T7 RNA polymerase molecules on a 21 kbps DNA molecule is studied. The bacteriophagal T7 RNA polymerase is a member of the class of single-subunit DNA-dependent RNA polymerases and transcribes a double stranded DNA molecule to yield an RNA product. Transcription is always initiated at the T7 promoter site and runs until a specific terminator is reached. <sup>1, 2</sup>

In this single-molecule experiment a fluorescently labeled T7 RNA polymerase and a 21 kbps dsDNA template with a T7 promotor are used. The DNA template is immobilized to a functionalized glass slide through a streptavidin-biotin interaction. The molecule is stretched by buffer flow. The RNA polymerase binds the promotor sequence on this DNA template and transcribes the template in the opposite direction of the flow. Tracking the fluorescent signal over time enables us to follow and characterize the transcription activity of the polymerase. After transcription is completed, a fluorescent dsDNA stain is added to the buffer to visualize the individual DNA molecules in a different color. For more information on the set-up and the biological relevance please visit the paper by Scherr et al. <sup>3</sup>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img7.png' width='450'>    
*Figure 1: The set-up of the single-molecule experiment investigating movement of the T7 RNA polymerase on a 21 kbps DNA template. Adapted from Scherr et al.* <sup>3</sup>
</div>


#### <a name="background"></a>Data analysis - The MARS workflow


<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img8.png' width='650'>  
*Figure 2: Flowchart showing the data analysis in MARS. The analysis is divided into four parts: raw corrections, localization & tracking, data correction and merge. These parts will be explained in detail below.*
</div>

As indicated in the flow chart, first some raw corrections to the collected frame are carried out: in this case only a [beam correction](https://duderstadt-lab.github.io/mars-docs/docs/image/BeamProfileCorrector/). Next the location of the fluorescent dots (T7 RNA polymerase) are [tracked](../docs/image/PeakTracker) over time, and the [DNA molecules](../../docs/image/DNA_finder) themselves will be localized. The subsequent data correction stage removes artefacts like movement through sample [drift](https://duderstadt-lab.github.io/mars-docs/docs/molecule/DriftCorrector/) and [transforms the coordinate frame](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/) so both DNA and protein are within the same coordinate frame work. In the final step the data is merged in a [DNA archive](../docs/molecule/BuildDNAarchive). This archive is then used to investigate T7 RNA polymerase movement on the DNA in the final sections of this example.



---

#### <a name="open"></a>Open the sample video with SCIFIO

First make sure the SCIFIO option is turned on: Edit>Options>ImageJ2, check the 'Use SCIFIO when opening files' option. This makes sure all metadata can be converted correctly to the archive later on.
Now open the file: use File>Open... and select the metadata file in the folder that contains the images of the video. A video viewer window opens automatically.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img1.png' width='450' /></div>

#### <a name="beam"></a>Correct for the beam profile

In most single-molecule microscopes, the excitation beam generates a gaussian profile across the image. Optimal tracking and fluorescence integration is only achieved after this profile has been corrected by dividing all frames by a representative beam profile image using the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector). The channel 0 of the example video contains individual proteins and should be corrected before tracking in step 3.

First, a representative image containing the beam profile must be created that will be used for correction. The last frame of channel 0 can be used for this purpose since it has the fewest number of protein spots remaining. To retrieve this image use Image>Duplicate... and select only frame 150. A single image should then appear that is a copy of frame 150. The image still contains many protein peaks that must be removed to build the representative image. This can be done by applying a gaussian blur with a radius of 40 (Process>Filters>Gaussian blur).

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img2.png' width='450' /></div>

Finally, run the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) with the full example video open and the profile image you have just created open. Set the Channel to 0 and the background image to the profile image you have just created. The electronic offset represents the intrinsic background noise from the camera used. You can leave this set to 0 for the purposes of this example. Run the command and you will see that the protein channel has been corrected for the beam profile. Note that sometimes this difference is difficult to spot by eye. One can always check by plotting the profile over a line in Fiji (Analyze>Plot Profile). If done correctly, the profile should go from a slightly parabolic shaped one to a flatter profile.

#### <a name="track"></a>Track the proteins to create a Single Molecule Archive

Now that the protein channel has been corrected in step 2, the [Peak Tracker](../../docs/image/PeakTracker) can be used to track all individual proteins in channel 0 over time to generate a Single Molecule Archive. First use the Rectangle tool to select the region that should be processed. This should be the top half of the image that contains all the labeled protein signal. Once your selection is complete, run the [Peak Tracker](../../docs/image/PeakTracker) with the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img3.png' width='450' /></div>

When the command is finished, a Single Molecule Archive should appear that contains all tracked molecules.

#### <a name="transform"></a>Transform to overlay tracked proteins with the DNA channel

The example video was collected using a dualview in which long wavelength emission is shown on the top half of the camera sensor and short wavelength emission is displayed on the bottom half of the sensor. Proteins were labeled a red dye and is longer in wavelength as compared to DNA that was imaged using a green stain that was shorter in wavelength. Therefore, the coordinates of the two channels are offset, appearing either on the bottom or two. To locate the positions of proteins on DNA molecules, the coordinates resulting from tracking must be transformed from the long wavelength channel to the reference frame of the short wavelength DNA channel. This is accomplished using the following script that loops through all molecule records and applies a 2D Affine transform to the x and y positions of all molecules. Make sure to select 'Groovy' as the script language before running. For more information on running scripts check the [introduction to groovy scripting tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/introduction-to-groovy-scripting/).

```groovy
#@ MoleculeArchive archive

import net.imglib2.realtransform.*
import de.mpg.biochem.mars.molecule.*
import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.util.*

AffineTransform2D xfm = new AffineTransform2D()
xfm.set(1.003149836793469, 6.13768352145E-4, 1.223492388374801, 3.61289752847E-4, 1.002457751873398, 507.24885432325897)

archive.getMoleculeUIDs().stream().forEach{ UID ->
	Molecule molecule = archive.get(UID)
	MarsTable table = molecule.getTable()

	for (int row=0; row <table.getRowCount() ; row++ ) {
		double[] source = new double[2]
		source[0] = table.getValue("x", row)
		source[1] = table.getValue("y", row)

		double[] target = new double[2]

		xfm.apply(source, target)

		table.setValue("x", row, target[0])
		table.setValue("y", row, target[1])
	}

	archive.put(molecule)
}

archive.logln(LogBuilder.buildTitleBlock("Transform Molecule Coordinates"))
archive.logln("[m00, m01, m02, m10, m11, m12]")
archive.logln("[" +
xfm.get(0,0) + ", " + xfm.get(0,1) + ", " + xfm.get(0,2) + ", " +
xfm.get(1,0) + ", " + xfm.get(1,1) + ", " + xfm.get(1,2) + "]")
archive.logln(LogBuilder.endBlock())
```
#### <a name="drift"></a>Driftcorrection of the x and y coordinates
In the next the step the x and y coordinates need to be drift corrected. For this the amount of drift needs to be assessed. The protein channel has fixed dots/stuck proteins which can be used. Stuck molecules have a lower movement variance than the mobile proteins. How to calculate the variance with MARS is documented [here](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-var/). Name the parameter 'y_var'. After calculating the variance a short script groovy script is used to tag molecules with a variance below a certain threshold with 'background'.

```groovy
#@ MoleculeArchive archive

archive.molecules().filter{ m -> m.getParameter("y_var") < 1}.forEach{ m -> m.addTag("background") }
```

The tagged molecules can then be used to calculate the drift to be used subsequently to correct the tracking coordinates. Select the 'Drift Corrector' tool (Plugins>MARS>Molecule>Drift Corrector) and supply the settings as shown below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img4.png' width='450' /></div>

Output columns should be x_drift_corr and y_drift_corr. The origin or zero should be the last frame because that is the frame (T) we used with the DNA finder and there was a few minutes between the protein collection and post staining of DNA.

#### <a name="DNA"></a>Find and fit the DNA molecules

Now that we have finished making the Single Molecule Archive, we can use the [DNA finder](../../docs/image/DNA_finder) to locate all DNA molecules in channel 1. This can be accomplished using the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img5.png' width='450' />
</div>

The only output should be a list of line ROIs added to the RoiManager, each with a UID. It is very important that the frame is set to 159 since the DNA can only be seen in the last frames.

#### <a name="BuildDNA"></a>Build a DNA Molecule Archive

Now we can use the Single Molecule Archive together with the DNA molecule line ROI sets that is in the RoiManager to create a DNA Molecule Archive using the [build DNA archive](../docs/molecule/BuildDNAarchive) command (Plugins>MARS>Molecule>Build DNA Archive) with the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/buildDNAArchiveSettings.png' width='450' /></div>

The DNA molecule archive should appear containing DNA molecule records that contain tables with all tracked molecules that were found to be located on DNAs as well as their position on DNA and integrated intensity. Now you can see individual tracks of molecules on a specific DNA molecule. You can calculate the speed in bp/s and the exact position on the DNA.

#### <a name="look"></a> Look at the Molecules in the Video
To have a glance at the identified molecules in the video the BigDataViewer tool in Rover is used. First, convert the video to N5 format (File>Save As>Export N5) and couple it to the archive. ([This tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/bdv/) explains this file conversion and coupling procedure in more detail) Also supply the Affine2D matrix parameters in the BDV sources tab. These are the same as used previously in this example in the script.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img6.png' width='450' /></div>

Now select a molecule of interest in the molecule tab and open the video viewer (Tools>Show Video). Select view number = 1. Open the settings tab by pressing the small << icon that appears when you hover the mouse over the left edge of the viewer window. Select the right source columns (as shown below) and select both the "circle" marker and the "follow" option. Move through the frames (T) by moving the slider on the bottom of the viewer to look at the tracked movement of the protein through the frames.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/tracking_gif.gif' width='450' /></div>


#### <a name="conc"></a> Conclusion
You have now successfully analyzed a full dataset containing stretched DNA molecules and a polymerase that moves on this DNA.


#### References
<sup>1</sup> McAllister W.T. (1997) Transcription by T7 RNA Polymerase. In: Eckstein F., Lilley D.M.J. (eds) Mechanisms of Transcription. Nucleic Acids and Molecular Biology, vol 11. Springer, Berlin, Heidelberg. https://doi.org/10.1007/978-3-642-60691-5_2
<sup>2</sup> Berg, Jeremy M., John L. Tymoczko, and Lubert Stryer. "Biochemistry." (2002)
<sup>3</sup> [Scherr, Wahab, Remus & Duderstadt, 2021, under consideration at Cell Press](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3775178)
