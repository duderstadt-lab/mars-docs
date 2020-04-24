---
layout: docs
title: Documentation
permalink: /docs/index.html
---

The **Mars** platform provides a collection of ImageJ2 commands for performing a wide variety of common image processing and secondary analysis tasks. This page provides documentation of the [ImageJ2 commands](#commands) and the graphical user interface for molecule classification.  

To start learning how to use Mars, we recommend first working through the introductory **[Let's Make a MoleculeArchive](../tutorials/create-a-Molecule-Archive)** and exploring the [examples](../examples), then dig further into the documentation.

Script writers and java developers can get started using the **Mars** development tutorial. Complete javadoc for the mars-core API can be found [here](http://duderstadt-lab.github.io/mars-core/javadoc/).

## <a name="commands"></a>Command Reference

### Molecule

| :----------------------------- | :----------- |
| [Import archive](./molecule/ImportArchive) | Use this command to open an archive.|
| [Build archive from table](./molecule/BuildArchiveFromTable) | Generate a MoleculeArchive using a table with a molecule index column. |
| [Region Difference Calculator](./molecule/RegionDifferenceCalculator) | Add parameter for difference between two regions. |
| [Add time](./molecule/AddTime) | Retrieve and add time information from metadata. |
| [Drift Calculator](./molecule/DriftCalculator) | Calculate drift in position. |
| [Drift Corrector](./molecule/DriftCorrector) | Remove drift from the molecule positions. |
| [MSD Calculator](./molecule/MSDCalculator) | Add mean squared displacement parameter for molecules. |
| [Merge Archives](./molecule/MergeArchives) | Merges a set of MoleculeArchives. |
| [Merge Virtual Stores](./molecule/MergeVirtualStore) | Merges a set of virtual MoleculeArchives. |

### Table

| :----------------------------- | :----------- |
| [Import table](./table/ImportTable) | Load csv or yamt format tables. |
| [Sort](./table/Sort) | Sort table rows by column values. |
| [Filter](./table/Filter) | Filter table rows. |
| [Build histogram](./table/BuildHistogram) | Bin observations to generate a histogram table |

### Image processing

| :----------------------------- | :----------- |
| [Peak Finder](./image/PeakFinder) | Counting and subpixel localization of molecules. |
| [Peak Tracker](./image/PeakTracker) | Find, subpixel localize, and track molecules. |
| [Beam Profile Corrector](./image/BeamProfileCorrector) | Correct images for non-uniform beam profile. |
| [Molecule Integrator](./image/MoleculeIntegrator) | Integrate the fluorescence of molecules. |
| [Gradient Calculator](./image/GradientCalculator) | Calculate the vertical gradient for images. |
| [Overlay Channels](./image/OverlayChannels) | Combine different colors into one image set. |

### KCP

| :----------------------------- | :----------- |
| [Change Point Finder](./kcp/ChangePointFinder) | Find kinetic changes points for a MoleculeArchive. |
| [Single Change Point Finder](./kcp/SingleChangePointFinder) | Find single change point positions. |
| [Segment Distribution Builder](./kcp/SegmentDistributionBuilder) | Build distributions using change point segments. |
| [Sigma Calculator](./kcp/SigmaCalculator) | Estimate background sigma level during optimization of change point detection. |

### ROI Tools

| :----------------------------- | :----------- |
| [Transform ROIs](./roi/TransformROIs) | Transform PointRois using an Affine2D transform. |
| [Open ROIs](./roi/OpenROIs) | Open a grid of small video for manually sorting. |
| [Save ROIs](./roi/SaveROIs) | Efficiently save all ROIs currently in the RoiManager to individual videos. |
| [Build ROIs from MoleculeArchive](./roi/BuildROIsFromMoleculeArchive) | Create box ROIs using coordinates from molecules in the archive. |

## <a name="gui"></a>Mars Rover

Accompanying the **Mars** software is the GUI: **Mars Rover**. This user interface helps to analyse, process, and filter the data in a streamlined and reproducible manner. To familiarise yourself with the most commonly used features explore the [Let's Make a MoleculeArchive](https://duderstadt-lab.github.io/mars-docs/tutorials/create-a-Molecule-Archive/) tutorial.

Toolbar Tabs

| :-------------- | :------------- | :------------ |
| |[Toolbar](./MarsRover/Toolbar) | Save, edit and delete. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img1.png' width='50' />|[Rover desktop](./MarsRover/RoverDesktop)  | Main features of the dataset and scriptable widgets. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img2.png' width='50' />|[Experiments](./MarsRover/Experiments)    | Metadata and data analysis log. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img3.png' width='50' />|[Molecules](./MarsRover/Molecules)     | UIDs with data tables and plot. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img4.png' width='50' />|[Documentation](./MarsRover/Documentation)  | Build-in text editor to annotate data sets. |
| <img align='center' src='{{site.baseurl}}/docs/img/Icons/img5.png' width='50' />|[Molecule filter]       | [insert info] |
