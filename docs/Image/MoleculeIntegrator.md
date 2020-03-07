---
layout: image
title: Molecule Integrator
permalink: /docs/image/MoleculeIntegrator/index.html
---
This command integrates the intensity of fluorescent peaks collected using TIRF. A list of peaks and their positions specified as UID numbers must be provided in the ROIManger as setup prior to running this command. This is generated using the [PeakFinder](../PeakFinder) and/or [Transform ROIs](../ROITools/TransformROIs). Also, the Image to integrate must be the active image. Using the specified settings, the integrated intensity of peaks will be corrected using the local background, specified using inner and outer radii. This will be performed for each frame and the final output is a MoleculeArchive with fluorescence intensity as a function of slice.

If a custom image format is selected, the image metadata will be retrieved from each frame and used for analysis. A huge number of settings are put into the headers of each frame, providing the time of collection, as well as many other setting about the microscope state at the time each frame was collected. This includes the lasers that were on and filters. To simplify processing of complex collections involving multiple colors at different times, this command can automatically detect the color and places each color in a unique column.

If Micromanager is the image format and there is a metadata.txt file in the image sequence directory, the summary section of this file will be parsed and placed in the Image metadata log. This provides the opportunity to add custom settings - ATP conc used, temperature, etc.. . If this file is absent the step will simply be ignored.

The molecule integrator is also designed to work well for data collected using a dual view. In this case, Short wavelength and Long wavelength information is specified in the ROI name in the ROIManger as UID_SHORT or UID_LONG. Then the integrated fluorescence of the different colors is placed in each molecule record, providing multiwavelength information about molecules. Sometimes this involves collecting with two lasers turned on, and other times you could be doing FRET with only a single laser on. In this case the FRET option should be checked.

Finally, the Short and Long wavelength ROIs are provided, this defines the boundaries for integration beyond which a reflected mirror image is used to avoid edge effects and issues at the center of the dual view. Peaks outside these ROIs will not be integrated.

If you have single color data, you can integrate only peaks at one wavelength by integrating only the Long or Short region using the checkboxes above each. For dualview data you need to check both. You could also use this to perform analysis on an image that was not collected with a dualview, but still provide either a long or short ROI that then covers the entire image.

*What does Long and Short Wavelength actually mean?*                                                                                                      

On Dobby, the Long wavelength on the top and in all experiments to date has been red at 637 nm. The short wavelength is on the bottom and could be 405, 488, 532, or possibly 561. Usually, we are using green at 532 nm.

#### Inputs

<img align='center' src='{{site.baseurl}}/docs/image/img/Molecule Integrator Dialog.png' width='400' />

* *Image* - The selected image will by analyzed using the peaks in the RoiManger. This is a required input but doesn't show up in the dialog, it is just the open image you have have selected at the time you run the command.
* *Microscope* - The microscope name added to the  metadata record.
* *Format* - The software used to collect the data. Currently, Micromanger is the only supported format that allows for automatic detection of color information and other settings. If None is selected then no metadata information will be taken from the frames and a generic ImageMetaData record will be created. This generic record will only have slice numbers in the data table, but other information can be manually added later if desired.
* *Colors* - The Detect option means the color information we will automatically determined based on the image metadata information, and other dialog settings. The Specify option means the names inputed for the Long Wavelength Color and Short Wavelength Color will be used.
* *Dichroic* - This is the dichroic you are using the split the light in the dual view. This is used to automatically determine the colors on each side of the dual view using the image metadata.
* *FRET* - If you are performing FRET check this box. It ensure the Long wavelength channel is integrated even when there is no long wavelength laser turned on. Only important when using automatic color detection.
* *Inner Radius* - The radius of pixels around the peak to integrate. 0 means only one pixel, 1 means a radius of out beyond the center pixel and etc..
* *Outer Radius* - The radius of pixels around the Inner Radius to use for background correction. If the Inner Radius is 1 and the Outer Radius is 5. Then the circular region between 1 and 5 will be integrate and serve as the local background to correct the inner region.
* *Integrate long wavelength* - If checked peaks within the long wavelength ROI will be integrated.
* *LONG x0* - Upper left corner x0 position of Long wavelength ROI.
* *LONG y0* - Upper right corner y0 position of Long wavelength ROI.
* *LONG width* - width of the Long wavelength ROI.
* *LONG height* - height of the Long wavelength ROI.
* *Long Wavelength Color* - The name of the long wavelength color that will be used as the color header in the outputted MoleculeArchive. Used if Colors:Specify is selected.
* *SHORT x0* - Upper left corner x0 position of Short wavelength ROI.
* *SHORT y0* - Upper right corner y0 position of Short wavelength ROI.
* *SHORT width* - width of the Short wavelength ROI.
* *SHORT height* - height of the Short wavelength ROI.
* *Short Wavelength Color* - The name of the short wavelength color that will be used as the color header in the outputted MoleculeArchive. Used if Colors:Specify is selected.

#### Output

* *MoleculeArchive* - A MoleculeArchive in which each molecule record has the integrated fluorescence using the color scheme specified or detected. Additionally, the peak position is saved for each frame.

### How to run this Command from a groovy script

```groovy
#@ ImagePlus image
#@ ImageJ ij
#@ RoiManager roiManager
#@output MoleculeArchive(label="archive.yama") archive

import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.molecule.*
import de.mpg.biochem.mars.ImageProcessing.*

//Make an instance of the Command you want to run...
final MoleculeIntegratorCommand moleculeIntegrator = new MoleculeIntegratorCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
moleculeIntegrator.setContext(ij.getContext())

moleculeIntegrator.setImage(image)
moleculeIntegrator.setMicroscope("Dobby")
moleculeIntegrator.setFormat("MicroManager")
moleculeIntegrator.setColors("Detect")
moleculeIntegrator.setDichroic("635lpxr")
moleculeIntegrator.setFRET(true)
moleculeIntegrator.setInnerRadius(1)
moleculeIntegrator.setOuterRadius(5)
moleculeIntegrator.setUseLongWavelength(true)
moleculeIntegrator.setLONGx0(0)
moleculeIntegrator.setLONGy0(0)
moleculeIntegrator.setLONGWidth(1024)
moleculeIntegrator.setLONGHeight(500)
moleculeIntegrator.setLONGWavelengthColor("Red")
moleculeIntegrator.setUseShortWavelength(true)
moleculeIntegrator.setSHORTx0(0)
moleculeIntegrator.setSHORTy0(524)
moleculeIntegrator.setSHORTWidth(1024)
moleculeIntegrator.setSHORTHeight(500)
moleculeIntegrator.setSHORTWavelengthColor("Green")

moleculeIntegrator.setRoiManager(roiManager)

//Run the Command
moleculeIntegrator.run()

//Retrieve output from the command
archive = moleculeIntegrator.getArchive()
```

#### Future Applications

Currently, this command will integrate fixed locations for all frames with correct color information independent of collection sequence. However, the implementation is written in a manner so that the integration module is a separate class and cane easily be used in scripts independent of this command. Internally, the command uses a peak position list for integrating peaks in each frame. So in future versions it would be very easy to integrate moving spots and/or provide a MoleculeArchive and use the molecule position as a function of Time for integration.

This command was coded with these possible future applications, however implementing these options depends on the need for them and the core modules can be used in scripts to accommodate a wide variety of workflows. But there are not current examples. That will also depend on projects needing custom integration options.
