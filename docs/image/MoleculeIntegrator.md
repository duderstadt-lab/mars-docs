---
layout: image
title: Molecule Integrator
permalink: /docs/image/MoleculeIntegrator/index.html
---
This command integrates the intensity of fluorescent peaks collected using TIRF. A list of peaks and their positions specified as UID numbers must be provided in the ROIManger as setup prior to running this command. This is generated using the [PeakFinder](../PeakFinder) and/or [Transform ROIs](https://duderstadt-lab.github.io/mars-docs/docs/roi/TransformROIs/). Also, the Image to integrate must be the active image. Using the specified settings, the integrated intensity of peaks will be corrected using the local background, specified using inner and outer radii. This will be performed for each frame and the final output is a Molecule Archive with fluorescence intensity as a function of frame (T).

The molecule integrator is also designed to work well for data collected using a dual view. In this case, Short wavelength and Long wavelength information is specified in the ROI name in the ROIManger as UID_SHORT or UID_LONG. Then the integrated fluorescence of the different colors is placed in each molecule record, providing multiwavelength information about molecules. Sometimes this involves collecting with two lasers turned on, and other times you could be doing FRET with only a single laser on. In this case the FRET option should be checked.

Finally, the Short and Long wavelength ROIs are provided, this defines the boundaries for integration beyond which a reflected mirror image is used to avoid edge effects and issues at the center of the dual view. Peaks outside these ROIs will not be integrated.

If you have single color data, you can integrate only peaks at one wavelength by integrating only the Long or Short region using the checkboxes above each. For dualview data you need to check both. You could also use this to perform analysis on an image that was not collected with a dualview, but still provide either a long or short ROI that then covers the entire image.

*What does Long and Short Wavelength actually mean?*                                                                                                      

The dualview splits the signal by wavelength and one side will have the Long wavelengths and the other the short wavelengths. These are then split into regions. A standard dichroic would split around 630 nm so that red and above is on one half and the rest of the colors are on the other half. For example, red might be on the top half of the image and on the bottom could be 405, 488, 532, or possibly 561. Short and long will obviously mean different things depending on the dichroic and laser lines used.

#### Inputs

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img8.png' width='400'/></div>

* *Image* - The selected image will by analyzed using the peaks in the RoiManager. This is a required input but doesn't show up in the dialog, it is just the open image you have have selected at the time you run the command.
* *Inner Radius* - The radius of pixels around the peak to integrate. 0 means only one pixel, 1 means a radius of out beyond the center pixel and etc.
* *Outer Radius* - The radius of pixels around the Inner Radius to use for background correction. If the Inner Radius is 1 and the Outer Radius is 5. Then the circular region between 1 and 5 will be integrate and serve as the local background to correct the inner region.
* *LONG x0* - Upper left corner x0 position of Long wavelength ROI.
* *LONG y0* - Upper right corner y0 position of Long wavelength ROI.
* *LONG width* - width of the Long wavelength ROI.
* *LONG height* - height of the Long wavelength ROI.
* *SHORT x0* - Upper left corner x0 position of Short wavelength ROI.
* *SHORT y0* - Upper right corner y0 position of Short wavelength ROI.
* *SHORT width* - width of the Short wavelength ROI.
* *SHORT height* - height of the Short wavelength ROI.
* *Microscope* - The microscope name added to the  metadata record.
* *FRET short wavelength name* - The name of the short wavelength color that will be used as the color header in the outputted MoleculeArchive.
* *FRET long wavelength name* - The name of the long wavelength color that will be used as the color header in the outputted MoleculeArchive.
* *Channels* - Autodetects which channels are present in the video (when supplied in the video metadata). Then allows for assignment of the correct function for each channel (f.e. None, Short, Long or FRET).



#### Output

* *MoleculeArchive* - A MoleculeArchive in which each molecule record has the integrated fluorescence using the color scheme specified or detected. Additionally, the peak position is saved for each frame.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img9.png' width='400'/></div>

### How to run this Command from a groovy script

```groovy
#@ Dataset dataset
#@ ImageJ ij
#@OUTPUT MoleculeArchive(label="archive.yama") archive

import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.molecule.*
import de.mpg.biochem.mars.image.*
import de.mpg.biochem.mars.util.*
import de.mpg.biochem.mars.image.commands.*
import java.util.HashMap
import java.util.Map

//Make an instance of the Command you want to run...
final MoleculeIntegratorCommand moleculeIntegrator = new MoleculeIntegratorCommand()

//Populates @Parameters Services using the current context
//which we get from the ImageJ input.
moleculeIntegrator.setContext(ij.getContext())

moleculeIntegrator.setInnerRadius(2)
moleculeIntegrator.setOuterRadius(7)
moleculeIntegrator.setLONGx0(0)
moleculeIntegrator.setLONGy0(0)
moleculeIntegrator.setLONGWidth(50)
moleculeIntegrator.setLONGHeight(25)
moleculeIntegrator.setSHORTx0(0)
moleculeIntegrator.setSHORTy0(25)
moleculeIntegrator.setSHORTWidth(50)
moleculeIntegrator.setSHORTHeight(25)
moleculeIntegrator.setMicroscope("Microscope")

//Below is an example of how to manually add the lists of peaks positions
//that should be integrated for all time points
//Alternatively, the lines below could be removed and Roi can be taken from
//the RoiManager. In that case the positions are assume to be constant
//for all time points.

String mol1UID = MarsMath.getUUID58()
String mol2UID = MarsMath.getUUID58()
String mol3UID = MarsMath.getUUID58()

Map<Integer, Map<String, Peak>> longIntegrationMap = new HashMap<>()
for (int t = 0; t < 50; t++) {
	Map<String, Peak> peaks = new HashMap<>()
	peaks.put(mol1UID, new Peak(mol1UID, 10.0d, 10.0d))
	peaks.put(mol2UID, new Peak(mol2UID, 32.5d, 8d))
	peaks.put(mol3UID, new Peak(mol3UID, 43.7d, 16.7d))
	longIntegrationMap.put(t, peaks)
}
moleculeIntegrator.addIntegrationMap("FRET Red", 0, moleculeIntegrator
	.getLONGInterval(), longIntegrationMap)

Map<Integer, Map<String, Peak>> longIntegrationMap2 = new HashMap<>()
for (int t = 0; t < 50; t++) {
	Map<String, Peak> peaks = new HashMap<>()
	peaks.put(mol1UID, new Peak(mol1UID, 10.0d, 10.0d))
	peaks.put(mol2UID, new Peak(mol2UID, 32.5d, 8d))
	peaks.put(mol3UID, new Peak(mol3UID, 43.7d, 16.7d))
	longIntegrationMap2.put(t, peaks)
}
moleculeIntegrator.addIntegrationMap("Red", 2, moleculeIntegrator
	.getLONGInterval(), longIntegrationMap2)

Map<Integer, Map<String, Peak>> shortIntegrationMap = new HashMap<>()
for (int t = 0; t < 50; t++) {
	Map<String, Peak> peaks = new HashMap<>();
	peaks.put(mol1UID, new Peak(mol1UID, 10.0d, 35d))
	peaks.put(mol2UID, new Peak(mol2UID, 32.5d, 33d))
	peaks.put(mol3UID, new Peak(mol3UID, 43.7d, 41.7d))
	shortIntegrationMap.put(t, peaks)
}
moleculeIntegrator.addIntegrationMap("FRET Green", 0, moleculeIntegrator
	.getSHORTInterval(), shortIntegrationMap)

Map<Integer, Map<String, Peak>> shortIntegrationMap2 = new HashMap<>()
for (int t = 0; t < 50; t++) {
	Map<String, Peak> peaks = new HashMap<>()
	peaks.put(mol1UID, new Peak(mol1UID, 10.0d, 35d))
	peaks.put(mol2UID, new Peak(mol2UID, 32.5d, 33d))
	peaks.put(mol3UID, new Peak(mol3UID, 43.7d, 41.7d))
	shortIntegrationMap2.put(t, peaks)
}
moleculeIntegrator.addIntegrationMap("Blue", 1, moleculeIntegrator
	.getSHORTInterval(), shortIntegrationMap2)

//Run the Command
moleculeIntegrator.run()

//Retrieve output from the command
archive = moleculeIntegrator.getArchive()
```
