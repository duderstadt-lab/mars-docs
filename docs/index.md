---
layout: docs
title: Documentation
permalink: /docs/index.html
---

The **Mars** platform provides a collection of Fiji/ImageJ2 commands for performing a wide variety of common image processing and analysis tasks. This page provides documentation of the [ImageJ2 commands](https://duderstadt-lab.github.io/mars-core/javadoc/) and the graphical user interface for molecule classification.  

To start learning how to use Mars, we recommend first working through the introductory **[Let's Make a Molecule Archive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/)** and exploring the [examples](../examples), and videos on the [Mars YouTube channel](https://www.youtube.com/channel/UCkkYodMAeotj0aYxjw87pBQ), then dig further into the documentation.

Script writers and java developers can get started using the **Mars** development tutorial. Complete javadoc for the mars-core API can be found [here](http://duderstadt-lab.github.io/mars-core/javadoc/). Mars naming conventions can be found [here](NamingConventions).

## <a name="commands"></a>Data Requirements

### Supported Image Formats

Mars stores image metadata using an enhanced version of the [Open Microscopy Environment (OME)](https://link.springer.com/article/10.1186/gb-2005-6-5-r47) format. This format provides a universal nomenclature for the storage of 6D images (X, Y, Z, C, T at multiple positions) collected in biological microscopy experiments. More information on the format and further requirements can be found in the [OME](./OME/) section. Mars image processing commands work best with images opened using [SCIFIO](https://scif.io/) that provides translators to OME format. SCIFIO can be activated in ImageJ or Fiji under Edit>Options>ImageJ2... Once activated, File>Open will use SCIFIO by default. An enhanced SCIFIO reader ([mars-scifio](https://github.com/duderstadt-lab/mars-scifio)) compatible with data collected using MicroManager 1.4 and 2.0 is available through the mars update site.

Mars image processing commands will also work with ordinary videos opened in ImageJ using other methods, such as the BioFormats plugin. However, in this case, metadata is generated using best guesses and may be less accurate. The Mars image processing commands assume that different excitation wavelengths and filter combinations are stored in different channels. We appreciate that many labs in the single-molecule community develop their own custom microscopes and collection formats. If excitation wavelength or emission filters are changed between frames and not stored in different channels, these videos will need to be converted. In most cases, the built-in tools in Fiji can handle this conversion process. For example:

* If each excitation wavelength was collected as a separate video, these can all be opened and combined as different channels using the Image>Color>Merge Channels... command.

* If alternating frames were captured using different excitation wavelengths, the Image>Stacks>Tools>Deinterleave can be used to split the sequence into two videos and the Merge Channels... command can then be used to recombine as two different channels.

* If the setup has two cameras for different emission wavelengths collected at the same time, the individual videos can be combined into single larger images using the Image>Stacks>Tools>Combine command and the Molecule Integrator (multiview) command could be used for combined integration.

These are just a couple examples of the many possibilities for conversion available using the Image>Stacks>Tools and Image>Color menus. Custom scripts might be required for more complex situations. As an example, we have written a [conversion script](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_workflows/FRET/scripts/static_FRET_reformat_video.groovy) for the videos in the [static FRET example workflow](../examples/Static_FRET/).

We are eager to help make sure Mars supports other image formats that we have not yet encountered in our own work and happy to help write conversion scripts to make Mars more accessible. Please get in touch by making a post on the [ImageJ forum](https://forum.image.sc) with the mars tag and/or mention @karlduderstadt to make sure we are notified.

## <a name="commands"></a>Command Reference

### Image

| :----------------------------- | :----------- |
| [Peak Finder](./image/PeakFinder) | Counting and subpixel localization of molecules. |
| [DNA finder](./image/DNA_finder) | Find and fit linear DNA molecules |
| [Peak Tracker](./image/PeakTracker) | Find, subpixel localize, and track molecules. |
| [Object Tracker](./image/ObjectTracker)| Fit and track unspecified objects based on segmentation. |
| [Molecule Integrator](./image/MoleculeIntegrator) | Integrate the fluorescence of molecules. |
| [Molecule Integrator (multiview)](./image/MoleculeIntegratorMultiView) | Integrate the fluorescence of molecules in a multiview setup. |

### Image > Util

| :----------------------------- | :----------- |
| [Beam Profile Corrector](./image/BeamProfileCorrector) | Correct images for non-uniform beam profile. |
| [Gradient Calculator](./image/GradientCalculator) | Calculate the vertical gradient for images. |
| [Overlay Channels](./image/OverlayChannels) | Combine different colors into one image set. |

### Molecule

| :----------------------------- | :----------- |
| [Open archive](./molecule/ImportArchive) | Use this command to open an archive.|
| [Open virtual store](./molecule/ImportArchive) | Use this command to open a virtually stored archive.|
| [Build archive from table](./molecule/BuildArchiveFromTable) | Generate a Molecule Archive using a table with a molecule index column. |
| [Build DNA archive](./molecule/BuildDNAarchive) | Builds a DNA Archive from a SingleMoleculeArchive and a list of DNA ROIs|
| [Merge Archives](./molecule/MergeArchives) | Merges a set of Molecule Archives. |
| [Merge Virtual Stores](./molecule/MergeVirtualArchives) | Merges a set of virtual Molecule Archives. |

### Molecule > Util

| :----------------------------- | :----------- |
| [Add time](./molecule/AddTime) | Retrieve and add time information from metadata. |
| [Drift Corrector](./molecule/DriftCorrector) | Correct the molecule coordinates for sample drift during the measurement. |
| [Region Difference Calculator](./molecule/RegionDifferenceCalculator) | Add parameter for difference between two regions. |
| [Variance Calculator](./molecule/varCalculator) | Add variance parameter for molecules. |

### Table

| :----------------------------- | :----------- |
| [Open table](./table/ImportTable) | Load csv or yamt format tables. |
| [Sort](./table/Sort) | Sort table rows by column values. |
| [Filter](./table/Filter) | Filter table rows. |

### Table > Import

| :------------- | :------------- |
| [Import IJ1 Table](./table/Import_IJ1)       | Convert a table in the IJ1 format (f.e. ResultsTable) to a MarsTable       |
| [Import TableDisplay](./table/Import_TableDisplay)       | Convert a table in the SciJava format (f.e. TableDisplay) to a Marstable       |

### Kinetic Change Point Analysis (KCP)

| :----------------------------- | :----------- |
| [Change Point Finder](./kcp/ChangePointFinder) | Find kinetic changes points for a Molecule Archive. |
| [Single Change Point Finder](./kcp/SingleChangePointFinder) | Find single change point positions. |
| [Sigma Calculator](./kcp/SigmaCalculator) | Estimate background sigma level during optimization of change point detection. |

### Transform ROIs

| :----------------------------- | :----------- |
| [Transform ROIs](./roi/TransformROIs) | Transform PointRois using an Affine2D transform. |

### Import

| :----------------------------- | :----------- |
| [LUMICKS h5](./import/lumicks) | Open Lumicks h5 optical tweezers datasets. |
| [Single-molecule dataset (SMD)](./import/smd) | Open json Single-molecule dataset (SMD) datasets. |

## <a name="gui"></a>Mars Rover

Accompanying the **Mars** software is the GUI: **Mars Rover**. This user interface helps to analyse, process, and filter the data in a streamlined and reproducible manner. To familiarise yourself with the most commonly used features explore the [Let's Make a Molecule Archive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/) tutorial.

**Toolbar Tabs**

| :-------------- | :------------- | :------------ |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img0.png' width='100' />|[Toolbar](./MarsRover/Toolbar) | Save, edit and delete. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img1.png' width='50' />|[Rover dashboard](./MarsRover/RoverDashboard)  | Main features of the dataset and [scriptable widgets](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/ScriptableWidgets/). |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img2.png' width='50' />|[Experiments](./MarsRover/Metadata)    | Metadata and data analysis log. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img3.png' width='50' />|[Molecules](./MarsRover/Molecules)     | UIDs with data tables and plot options. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img4.png' width='50' />|[Comments](./MarsRover/Comments)  | Build-in text editor to annotate data sets. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img5.png' width='50' />|[Settings](./MarsRover/Settings)      | Archive specific settings. |

For an extensive overview of the scriptable widgets visit the [scriptable widgets](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/ScriptableWidgets/) page.

**Mars Kymograph**

| :------------- | :------------ |
| [Interval Exporter](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/MarsIntervalExporter/) | Export an interval |
| [Dna Archive Kymograph](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/DnaArchiveKymographBuilder/) | Dna Archive Kymograph |
| [Dna Archive Region](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/DnaArchiveMoleculeRegionBuilder/) | Dna Archive Region for Montages |
| [Montage](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/MontageBuilder/) | Montage builder |
| [Object Archive Region](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/ObjectArchiveRegionBuilder/) | Extract region from Object Archive |
| [Object Formatter](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/ObjectShapeFormatter/) | Format Object Montage |
| [Transverse Flow Archive Region](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/TransverseFlowArchiveMoleculeRegionBuilder/) | Extract region from Transverse Flow Archive |
| [Image Formatter](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/ImageFormatter/) | Format Montage |
