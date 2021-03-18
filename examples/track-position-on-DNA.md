---
layout: example
title: Track protein position on DNA Pipeline
permalink: /examples/track-position-on-DNA/index.html
---

#### Quick summary

DNA is one of the essential macromolecules throughout all domains of life. A lot of proteins interact with DNA and move along. Single molecule approaches can be used to study these processes. This example demonstrates a complete workflow for tracking the positions of proteins moving on individual DNA molecules.

#### <a name="reference"></a>Table of contents

- [Background information - The MARS workflow](#background)
- [Open the sample video with SCIFIO](#open)
- [Correct for the beam profile](#beam)
- [Track the proteins to create a Single Molecule Archive](#track)
- [Transform to overlay tracked proteins with the DNA channel](#transform)
- [Driftcorrection of the x and y coordinates ](#drift)
- [Find and fit the DNA molecules](#DNA)
- [Build a DNA Molecule Archive](#BuildDNA)
- [Conclusion](#conc)

#### <a name="background"></a>Background information
The first step in the workflow is tracking the proteins as a function of time using the [Peak Tracker](../../docs/image/PeakTracker). Once optimal peak finding and tracking settings have been found this command generates a Molecule Archive containing the tracking results. Next, the stained DNA molecules are located and fit using the [DNA finder](../../docs/image/DNA_finder). Finally, the [build DNA archive](../docs/molecule/BuildDNAarchive) command is used to generate a DNA Molecule Archive containing DNA molecule records with the positions of all proteins on them as a function of time.

This pipeline has been developed in a modular fashion to allow for maximum flexibility in image preprocessing and manual adjustment depending on the individual demands of the current project.

#### <a name="open"></a>Open the sample video with SCIFIO

First make sure the SCIFIO option is turned on: Edit>Options>ImageJ2, check the 'Use SCIFIO when opening files' option. This makes sure all metadata can be converted correctly to the archive later on.
Now open the file: use File>Open... and select the metadata file in the folder that contains the images of the video. A video viewer window opens automatically.

#### <a name="beam"></a>Correct for the beam profile

In most single-molecule microscopes, the excitation beam generates a gaussian profile across the image. Optimal tracking and fluorescence integration is only achieved after this profile has been corrected by dividing all frames by a representative beam profile image using the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector). The channel 0 of the example video contains individual proteins and should be corrected before tracking in step 3.

First, a representative image containing the beam profile must be created that will be used for correction. The last frame of channel 0 can be used for this purpose since it has the fewest number of protein spots remaining. To retrieve this image use Image>Duplicate... with a setting of 150. A single image should then appear that is a copy of frame 150. The image still contains many protein peaks that must be removed to build the representative image. This can be done by applying a gaussian blur with a radius of 40 (Process>Filters>Gaussian blur).

Finally, run the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) with the full example video open and the profile image you have just created open. Set the Channel to 0 and the background image to the profile image you have just created. The electronic offset represents the intrinsic background noise from the camera used. You can leave this set to 0 for the purposes of this example. Run the command and you will see that the protein channel has been corrected for the beam profile. Note that sometimes this difference is difficult to spot by eye. One can always check by plotting the profile over a line in Fiji (Analyze>Plot Profile). If done correctly, the profile should go from a slightly parabolic shaped one to a flatter profile.

#### <a name="track"></a>Track the proteins to create a Single Molecule Archive

Now that the protein channel has been corrected in step 2, the [Peak Tracker](../../docs/image/PeakTracker) can be used to track all individual proteins in channel 0 over time to generate a Single Molecule Archive. First use the Rectangle tool to select the region that should be processed. This should be the top half of the image that contains all the labeled protein signal. Once your selection is complete, run the [Peak Tracker](../../docs/image/PeakTracker) with the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/peakTrackerSettings.png' width='450' /></div>

When the command is finished, a Single Molecule Archive should appear that contains all tracked molecules.

#### <a name="transform"></a>Transform to overlay tracked proteins with the DNA channel

The example video was collected using a dualview in which long wavelength emission is shown on the top half of the camera sensor and short wavelength emission is displayed on the bottom half of the sensor. Proteins were labeled a red dye and is longer in wavelength as compared to DNA that was imaged using a green stain that was shorter in wavelength. Therefore, the coordinates of the two channels are offset, appearing either on the bottom or two. To locate the positions of proteins on DNA molecules, the coordinates resulting from tracking must be transformed from the long wavelength channel to the reference frame of the short wavelength DNA channel. This is accomplished using the following script that loops through all molecule records and applies a 2D Affine transform to the x and y positions of all molecules. Make sure to select 'Groovy' as the script language before running.

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
In the next the step the x and y coordinates need to be drift corrected. For this the amount of drift needs to be assessed. The protein channel has fixed dots/stuck proteins which can be used. Stuck molecules have a lower mean square displacement than the mobile proteins. How to calculate the variance with MARS is documented [here](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-var/). After calculating the variance a short script groovy script is used to tag molecules with a variance below a certain threshold with 'background'.

```groovy
#@ MoleculeArchive archive

archive.molecules().filter{ m -> m.getParameter("MSD") < 1}.forEach{ m -> m.addTag("background") }
```

The tagged molecules can then be used to calculate the drift with the 'Drift Calculator' (Plugins> MARS> Molecules> Drift Calculator). 'Zero' needs to be 'end' since the last frame contains the DNA molecules.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/driftcalculator.png' width='350' />
</div>

After running the function the drift is calculated. In the next step the x and y coordinates need to corrected with the calculated drift using the 'Drift Corrector' tool.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/driftcorrector.png' width='350' />
</div>

Output columns should be x_drift_corr and y_drift_corr. The origin or zero should be the last frame because that is the frame (T) we used with the DNA finder and there was a few minutes between the protein collection and post staining of DNA.

#### <a name="DNA"></a>Find and fit the DNA molecules

Now that we have finished making the Single Molecule Archive, we can use the [DNA finder](../../docs/image/DNA_finder) to local all DNA molecules in channel 0. This can be accomplished using the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/dnaFinderSettings.png' width='450' />
</div>

The only output should be a list of line ROIs added to the RoiManager, each with a UID. It is very important that the frame is set to 159 since the DNA can only be seen in the last frames.

#### <a name="BuildDNA"></a>Build a DNA Molecule Archive

Now we can use the Single Molecule Archive together with the DNA molecule line ROI sets that is in the RoiManager to create a DNA Molecule Archive using the [build DNA archive](../docs/molecule/BuildDNAarchive) command with the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/buildDNAArchiveSettings.png' width='450' /></div>

The DNA molecule archive should appear containing DNA molecule records that contain tables with all tracked molecules that were found to be located on DNAs as well as their position on DNA and integrated intensity.

#### <a name="conc"></a> Conclusion

Now you can see individual tracks of molecules on a specific DNA molecule. You can calculate the speed in bp/s and the exact position on the DNA.
