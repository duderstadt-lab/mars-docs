---
layout: image
title: Molecule Integrator (multiview)
permalink: /docs/image/MoleculeIntegratorMultiView/index.html
---

This command integrates the intensity of fluorescent peaks in images collected with dual or multiview setups in which different emission wavelengths are separated onto different regions of a camera sensor. A list of point or circle ROIs must be provided in the ROI Manager as setup prior to running this command. The same peak in different regions of the multiview should have the same UID but with the addition of a suffix with the subregion name. This could be the color or the name of the labeled component (i.e. UID_red, UID_green or UID_DNA, UID_protein). The ROIs are generated using the [PeakFinder](../PeakFinder) and [Transform ROIs](https://duderstadt-lab.github.io/mars-docs/docs/roi/TransformROIs/) commands.

The integrated intensity of peaks will be corrected for local background. The integration and background regions are specified using inner and outer radii. The final output is a Molecule Archive containing single molecule records with the fluorescence intensity for each wavelength as a function of time point (T). This command can be used to analyze experiments where two or more lasers are turned on to image different components simultaneously, or FRET experiments with only a single laser on at a given time. The [FRET example](../../../examples/fret) provides a step-by-step guide using this command with sample data available.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/MultiViewIntegrationOverview.png' width='800'/></div>

#### Input

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img35.png' width='350'/><img  src='{{site.baseurl}}/docs/image/img/img38.png' width='350'/></div>

* *Image* - The active image is analyzed with the peaks in the ROI Manager. The name of the image will be displayed in the dialog.

#### Boundaries
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img39.png' width='350'/></div>
* *LONG x0* - Upper left corner x0 position of Long wavelength ROI.
* *LONG y0* - Upper right corner y0 position of Long wavelength ROI.
* *LONG width* - width of the Long wavelength ROI.
* *LONG height* - height of the Long wavelength ROI.
* *SHORT x0* - Upper left corner x0 position of Short wavelength ROI.
* *SHORT y0* - Upper right corner y0 position of Short wavelength ROI.
* *SHORT width* - width of the Short wavelength ROI.
* *SHORT height* - height of the Short wavelength ROI.


* *Inner Radius* - The radius of pixels around the peak to integrate. 0 means only one pixel, 1 means a radius of out beyond the center pixel and etc.
* *Outer Radius* - The radius of pixels around the Inner Radius to use for background correction. If the Inner Radius is 1 and the Outer Radius is 5. Then the circular region between 1 and 5 will be integrated and serve as the local background to correct the inner region.
* *Channel Integration options* - The options 'Integrate' and 'Do not integrate' are offered for all channels discovered in the input image. Channel names are used when provided. Otherwise, channel index starting from 0 is used.


#### Output
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/image/img/img37.png' width='350'/><img  src='{{site.baseurl}}/docs/image/img/img41.png' width='350'/></div>

* *Microscope* - The microscope name added to the  metadata record.
* *Metadata UID* - Options to either generate a metadata UID based on the metadata UID supplied in the image metadata or a new one that is random.
* *Thread count* - Determines how much computing power of your computer will be devoted to this calculation. A higher thread count decreases computing time.
* *Verbose* - This option includes extra table columns with more integration information. By default, the integrated peak signal corrected for median background and the median background used for the correction are included in the table of single molecule records generated. With the Verbose option checked, the table will also include the uncorrected integrated intensity and mean background.

The median background and mean background columns represent an estimate of the background signal within the inner radius region. First the median or mean pixel values in the outer region are calculated and then multiplied by the number of pixels in the inner region to represent an accurate estimate of the background signal in the inner region.

#### Result

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
