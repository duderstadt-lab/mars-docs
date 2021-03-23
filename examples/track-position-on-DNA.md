---
layout: example
title: Track the Position of a Protein on a long DNA Molecule
permalink: /examples/track-position-on-DNA/index.html
---

DNA is one of the essential macromolecules throughout all domains of life. Many proteins interact with DNA, and while doing so move along the DNA molecule. Single molecule approaches can be used to study the kinetics and mechanisms of these processes. This example demonstrates a complete workflow from microscopy data to kinetic information about the enzyme studied on a single-molecule level.

#### <a name="reference"></a>Table of contents

- [Biological system - T7 RNA Polymerase movement on DNA](#biol)
- [Data analysis - The MARS workflow](#background)
- [Open the video & raw corrections](#open)
- [Localization & Tracking](#track)
- [Data correction](#drift)
- [Merge - Build a DNA Molecule Archive](#BuildDNA)
-	[Output - Data interpretation](#look)
- [Conclusion](#conc)
- [References](#refs)

#### <a name="biol"></a> Biological system - T7 RNA Polymerase movement on DNA
In this example the processivity of single T7 RNA polymerase molecules on a 21 kbps DNA molecule is studied. The bacteriophagal T7 RNA polymerase is a member of the class of single-subunit DNA-dependent RNA polymerases and transcribes a double stranded DNA molecule to yield an RNA product. Transcription is always initiated at the T7 promoter site and runs until a specific terminator is reached. <sup>1, 2</sup>

In this single-molecule experiment a fluorescently labeled T7 RNA polymerase and a 21 kbps dsDNA template with a T7 promotor are used. The DNA template is immobilized to a functionalized glass slide through a streptavidin-biotin interaction. The molecule is stretched by buffer flow. The RNA polymerase binds the promotor sequence on this DNA template and transcribes the template in the opposite direction of the flow. Tracking the fluorescent signal over time enables us to follow and characterize the transcription activity of the polymerase. After transcription is completed, a fluorescent dsDNA stain is added to the buffer to visualize the individual DNA molecules in a different color. For more information on the set-up and the biological relevance please visit the paper by Scherr et al. <sup>3</sup>

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img7.png' width='450'></div>  

<div style="text-align: center">
Figure 1: The set-up of the single-molecule experiment investigating movement of the T7 RNA polymerase   on a 21 kbps DNA template. Adapted from Scherr et al. <sup>3</sup>
</div>


#### <a name="background"></a>Data analysis - The MARS workflow
<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img8.png' width='650'></div>  

<div style="text-align: center">
Figure 2: Flowchart showing the data analysis in MARS. The analysis is divided into four parts: raw corrections,  localization & tracking, data correction and merge. These parts will be explained in detail below.
</div>

First, some raw corrections to the collected frame are carried out: in this case only a [beam correction](https://duderstadt-lab.github.io/mars-docs/docs/image/BeamProfileCorrector/). Next the location of the fluorescent dots (T7 RNA polymerase) are [tracked](../docs/image/PeakTracker) over time, and the [DNA molecules](../../docs/image/DNA_finder) themselves will be localized. The subsequent data correction stage removes artefacts like movement through sample [drift](https://duderstadt-lab.github.io/mars-docs/docs/molecule/DriftCorrector/) and [transforms the coordinate frame](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/) so both DNA and protein are within the same coordinate frame work. In the final step the data is merged in a [DNA archive](../docs/molecule/BuildDNAarchive). This archive is then used to investigate T7 RNA polymerase movement on the DNA in the final sections of this example.

---

#### <a name="open"></a> Open the video & raw corrections
**Open the sample video with SCIFIO**  
To open the video first turn the SCIFIO option on in Fiji: Edit>Options>ImageJ2, check the 'Use SCIFIO when opening files' option. Then open the file: use File>Open... and select the metadata file in the folder that contains the images of the video. A video viewer window opens automatically.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img1.png' width='450' /></div>


**Correct for the beam profile**  
In most single-molecule microscopes, the excitation beam generates a gaussian profile across the image. This hinders tracking and fluorescence integration algorithms. Therefore this profile is corrected by dividing all frames by a beam profile image. To do so the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) tool is used.

First, the beam profile image must be created. For this the last frame of channel 0 is used. To retrieve this image use Image>Duplicate... and select only frame 150. A single image should then appear that is a copy of frame 150. The image still contains many protein peaks that must be removed to build the representative image. This can be done by applying a gaussian blur with a radius of 40 (Process>Filters>Gaussian blur). The resulting image will look similar to the one shown below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img2.png' width='450' /></div>

Now, run the [Beam Profile Corrector](../../docs/image/BeamProfileCorrector) command. Make sure the full example video and the beam profile image are open in Fiji. Set the Channel to 0 and the background image to the profile image you have just created. The electronic offset represents the intrinsic background noise from the camera used. You can leave this set to 0 for the purposes of this example. Run the command and you will see that the protein channel has been corrected for the beam profile. Note that sometimes this difference is difficult to spot by eye.

#### <a name="track"></a> Localization & Tracking

**Track the protein dots to create a Single Molecule Archive**  
Now the raw data has been corrected, the location of the proteins is tracked using the [Peak Tracker](../../docs/image/PeakTracker). This generates a SingleMolecule Archive containing the x,y-coordinates of each spot with respect to time. First use the Rectangle tool in Fiji to select the region that should be processed. This should be the top half of the image that contains all the labeled protein signal. Once your selection is complete, run the [Peak Tracker](../../docs/image/PeakTracker) with the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img3.png' width='450' /></div>

When the command is finished, a Single Molecule Archive should appear that contains all tracked molecules.

**Find and fit the DNA molecules**  
Now the proteins have been tracked, we can use the [DNA finder](../../docs/image/DNA_finder) to locate all DNA molecules in channel 1. This can be accomplished using the following settings:

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img5.png' width='450' />
</div>

The only output should be a list of line ROIs added to the RoiManager, each with a UID. It is very important that the frame is set to 159 since the DNA can only be seen in the last frames.

#### <a name="drift"></a> Data correction
**Drift correction of the x and y coordinates**  
Now the coordinates of the proteins and DNA molecules are identified a correction for sample drift in the x and y direction has to be made. Such sample drift can arise for example by slight microscope stage movement over time and would create artefacts in data analysis if not corrected.

To assess the amount of drift that occurred throughout the measurement fixed dots/stuck proteins on the surface are used. These molecules should remain at their fixed position throughout the measurement, so any deviation thereof needs to be corrected. To select for these molecules, the variance of movement is calculated for each dot. All dots with a high variance are mobile, all dots with a very low variance are most likely stuck on the surface and can then be tagged 'background'. These 'background' molecules will then be used for drift correction.

First, calculate the variance for each dot using the [variance calculator](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-var/). Name the parameter 'y_var'. After running the command run the short script below to automatically tag all molecules with a low y_var as 'background'. Use the [script editor](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/introduction-to-groovy-scripting/) to run the script and select 'Groovy' as the running language.

```groovy
#@ MoleculeArchive archive

archive.molecules().filter{ m -> m.getParameter("y_var") < 1}.forEach{ m -> m.addTag("background") }
```

Now run the [drift corrector](https://duderstadt-lab.github.io/mars-docs/docs/molecule/DriftCorrector/) command and supply the settings as shown below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img4.png' width='450' /></div>

Output columns should be x_drift_corr and y_drift_corr. The origin or zero should be the last frame because that is the frame (T) we used with the DNA finder and there was a few minutes between the protein collection and post staining of DNA.


**Transform to overlay tracked proteins with the DNA channel**  
The example video was collected using a dual view system. This system allows for the recording of two wavelengths at the same time using a split detector. The top half of the detector detects the long wavelength (f.e. red) while at the same time a shorter wavelength (f.e. green) is detected on the lower half of the detector. In this way it is possible to detect two colors at the same time. This, however, at the same time means that both channels are recorded in a different coordinate framework. So in this step a correction has to be made to convert one framework to the other so the overlap of the protein with DNA can be studied.

To determine the conversion matrix from one framework to the other, an Affine2D calculation is used. More information about this conversion can be found in the [Affine2D tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/affine2D/HowToCalculateAffine2D/). Run the script below to automatically apply this transformation. Make sure to select 'Groovy' as the script language before running. For more information on running scripts check the [introduction to groovy scripting tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/introduction-to-groovy-scripting/).

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

#### <a name="BuildDNA"></a>Merge - Build a DNA Molecule Archive
Now all coordinates have been corrected and artefacts have been removed, the DNA location data can be merged with the protein tracking information stored in the Single Molecule Archive. This is done using the [build DNA archive](../docs/molecule/BuildDNAarchive) command with the settings as shown below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/buildDNAArchiveSettings.png' width='450' /></div>

The DNA molecule archive should appear containing DNA molecule records that contain tables with all tracked molecules that were found to be located on DNAs as well as their position on DNA and integrated intensity. Now you can see individual tracks of molecules on a specific DNA molecule. You can calculate the speed in bp/s and the exact position on the DNA from the data displayed in the table.
Note that some DNA molecules were found to overlap with more than one moving protein. These are numbered polymerase1, polymerase2, etc.

#### <a name="look"></a> Output - Data interpretation
**Look at the Molecules in the Video**  
To have a glance at the identified molecules in the video the BigDataViewer tool in Rover is used. First, convert the video to N5 format (File>Save As>Export N5) and couple it to the archive. ([This tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/bdv/) explains this file conversion and coupling procedure in more detail) Also supply the Affine2D conversion matrix parameters in the BDV sources tab. These are the same as used previously in this example in the script.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img6.png' width='450' /></div>

Now select a molecule of interest in the molecule tab and open the video viewer (Tools>Show Video). Select view number = 1. Open the settings tab by pressing the small << icon that appears when you hover the mouse over the left edge of the viewer window. Select the right source columns (as shown below) and select both the "circle" marker and the "follow" option. Move through the frames (T) by moving the slider on the bottom of the viewer to look at the tracked movement of the protein through the frames.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/tracking_gif.gif' width='450' /></div>

**Kinetic studies**  
Now all coordinates with respect to time are calculated for each polymerase that was recorded many kinetic detailed studies are possible. These for example include studies of processivity, reaction rate, pausing behavior and studies in presence and absence of other components to study interactions.

As example we will examine the global reaction rate of the observed T7 RNA polymerases in our set-up. To do so, we will use the T column as a measure of time, and the 'position on DNA' column as a measure of reaction progression since it expresses the position of the protein with respect to the nucleotide position on the DNA. When we now divide the total change in position (progression) by the total time we obtain a global reaction rate. To do so run the script below in the script editor. Use 'Groovy' as the running language.

```Groovy
#@ MoleculeArchive archive

import de.mpg.biochem.mars.molecule.*
import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.util.*
import org.scijava.table.*

archive.getMoleculeUIDs().stream().forEach({UID ->
  Molecule molecule = archive.get(UID)
  MarsTable table = molecule.getTable()
  double ymin = table.min("polymerase_1_Position_on_DNA")
  double ymax = table.max("polymerase_1_Position_on_DNA")
  double Tmin = table.min("polymerase_1_T")
  double Tmax = table.max("polymerase_1_T")
  double dist = ymax - ymin
  double time = Tmax - Tmin
  double speed = dist / time
  molecule.setParameter("dist_DNA",dist)
  molecule.setParameter("del_T",time)
  molecule.setParameter("reaction_rate",speed)
 })
```

The script above calculates three parameters:
- dist_DNA: the total distance the polymerase travelled on the DNA (nucleotides)
- del_T: the total time elapsed for this movement (/frame)
- reaction_rate: the global reaction rate expressed as nucleotides/frame

These parameter values can be found in the [parameter tab](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/Molecules/).

To visualize the spread in the calculated global reaction rate between all measured molecules we will make a plot using a [scriptable widget](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/ScriptableWidgets/). To do so move to the [Dashboard](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/RoverDashboard/) and click the histogram icon. Move to the scripting tab (<>) and past the following code. Select 'Python' as running language and press the refresh button. Then move to the graph tab and obtain the histogram as shown below.

```
#@ MoleculeArchive archive
#@OUTPUT String xlabel
#@OUTPUT String ylabel
#@OUTPUT String title
#@OUTPUT Integer bins
#@OUTPUT Double xmin
#@OUTPUT Double xmax
#@OUTPUT Double ymin
#@OUTPUT Double ymax

# Set global outputs
xlabel = "Rate of Reaction (nts/T)"
ylabel = "Frequency"
title = "Distribution"
bins = 10
xmin = 0.0
xmax = 350.0
ymin = 0.0
ymax = 100.0

# Series 1 Outputs
#@OUTPUT Double[] series1_values
#@OUTPUT String series1_strokeColor
#@OUTPUT Integer series1_strokeWidth

series1_strokeColor = "black"
series1_strokeWidth = 2

series1_values = []

for molecule in archive.molecules().iterator():
    series1_values.append(molecule.getParameter("reaction_rate"))
```

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/examples/img/dnaArchive/img9.png' width='450' /></div>


#### <a name="conc"></a> Conclusion
You have now successfully analyzed a full dataset containing stretched DNA molecules and a polymerase that moves on this DNA.

#### <a name="refs"></a> References
<sup>1</sup> McAllister W.T. (1997) Transcription by T7 RNA Polymerase. In: Eckstein F., Lilley D.M.J. (eds) Mechanisms of Transcription. Nucleic Acids and Molecular Biology, vol 11. Springer, Berlin, Heidelberg. https://doi.org/10.1007/978-3-642-60691-5_2   
<sup>2</sup> Berg, Jeremy M., John L. Tymoczko, and Lubert Stryer. "Biochemistry." (2002)   
<sup>3</sup> [Scherr, Wahab, Remus & Duderstadt, 2021, under consideration at Cell Press](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3775178)
