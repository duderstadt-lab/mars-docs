---
layout: docs
title: Documentation
permalink: /docs/index.html
---

The **Mars** platform provides a collection of ImageJ2 commands for performing a wide variety of common image processing and secondary analysis tasks. This page provides documentation of the [ImageJ2 commands](https://duderstadt-lab.github.io/mars-core/javadoc/) and the graphical user interface for molecule classification.  

To start learning how to use Mars, we recommend first working through the introductory **[Let's Make a Molecule Archive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/)** and exploring the [examples](../examples), then dig further into the documentation.

Script writers and java developers can get started using the **Mars** development tutorial. Complete javadoc for the mars-core API can be found [here](http://duderstadt-lab.github.io/mars-core/javadoc/).

## <a name="commands"></a>Data Requirements - OME

### Open Microscopy Environment (OME)
The structure of the data that can be used to build MoleculeArchives on using the **Mars** software and plugins is based on the [Open Microscopy Environment (OME)](https://link.springer.com/article/10.1186/gb-2005-6-5-r47) file format. This open source format provides a universal framework for acquiring and storing of imaging data in biological microscopy experiments.
More information on the format and further requirements can be found in the [OME](./OME/) section.


## <a name="commands"></a>Command Reference

### Molecule

| :----------------------------- | :----------- |
| [Open archive](./molecule/ImportArchive) | Use this command to open an archive.|
| [Open virtual store](./molecule/ImportArchive) | Use this command to open a virtually stored archive.|
| [Build DNA archive](./molecule/BuildDNAarchive) | |
| [Build archive from table](./molecule/BuildArchiveFromTable) | Generate a Molecule Archive using a table with a molecule index column. |
| [Region Difference Calculator](./molecule/RegionDifferenceCalculator) | Add parameter for difference between two regions. |
| [FMT Generate bps](./molecule/FMTbps) | |
| [Add time](./molecule/AddTime) | Retrieve and add time information from metadata. |
| [Drift Calculator](./molecule/DriftCalculator) | Calculate drift in position. |
| [Drift Corrector](./molecule/DriftCorrector) | Remove drift from the molecule positions. |
| [Variance Calculator](./molecule/varCalculator) | Add variance parameter for molecules. |
| [Merge Archives](./molecule/MergeArchives) | Merges a set of Molecule Archives. |
| [Merge Virtual Stores](./molecule/MergeVirtualArchives) | Merges a set of virtual Molecule Archives. |

### Import

| :------------- | :------------- |
| [Import IJ1 Table](./import/Import_IJ1)       | Import a table in the IJ1 format.       |
| [Import TableDisplay](./import/Import_TableDisplay)       | Import a table in the TableDisplay format.       |


### Table

| :----------------------------- | :----------- |
| [Open table](./table/ImportTable) | Load csv or yamt format tables. |
| [Sort](./table/Sort) | Sort table rows by column values. |
| [Filter](./table/Filter) | Filter table rows. |


### Image processing

| :----------------------------- | :----------- |
| [DNA finder](./image/DNA_finder) | |
| [Peak Finder](./image/PeakFinder) | Counting and subpixel localization of molecules. |
| [Peak Tracker](./image/PeakTracker) | Find, subpixel localize, and track molecules. |
| [Beam Profile Corrector](./image/BeamProfileCorrector) | Correct images for non-uniform beam profile. |
| [Molecule Integrator](./image/MoleculeIntegrator) | Integrate the fluorescence of molecules. |
| [Gradient Calculator](./image/GradientCalculator) | Calculate the vertical gradient for images. |
| [Overlay Channels](./image/OverlayChannels) | Combine different colors into one image set. |

### Kinetic Change Point Analysis (KCP)

| :----------------------------- | :----------- |
| [Change Point Finder](./kcp/ChangePointFinder) | Find kinetic changes points for a Molecule Archive. |
| [Single Change Point Finder](./kcp/SingleChangePointFinder) | Find single change point positions. |
| [Sigma Calculator](./kcp/SigmaCalculator) | Estimate background sigma level during optimization of change point detection. |

### Transform ROIs

| [Transform ROIs](./roi/TransformROIs) | Transform PointRois using an Affine2D transform. |

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
