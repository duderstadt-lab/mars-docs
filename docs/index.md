---
layout: docs
title: Documentation
permalink: /docs/index.html
---

The **Mars** platform provides a collection of ImageJ2 commands for performing a wide variety of common image processing and secondary analysis tasks. This page provides documentation of the [ImageJ2 commands](#commands) and the graphical user interface for molecule classification.  

To start learning how to use Mars, we recommend first working through the introductory **[Let's Create a MoleculeArchive](../tutorials/create-MoleculeArchive)** and exploring the [examples](../examples), then dig further into the documentation.

Script writers and java developers can get started using the **Mars** development tutorial. Complete javadoc for the mars-core API can be found [here](http://duderstadt-lab.github.io/mars-core/javadoc/).

## <a name="commands"></a>Command Reference
### Molecule

| :----------------------------- | :----------- |
| [Import archive](./Molecule/ImportArchive) | Use this command to open an archive.|
| [Build archive from table](BuildArchiveFromTable) | Generate a MoleculeArchive using a table with a molecule index column. |
| [Region Difference Calculator](RegionDifferenceCalculator) | Add parameter for difference between two regions. |
| [Add time](AddTime) | Retrieve and add time information from metadata. |
| [Drift Calculator](DriftCalculator) | Calculate drift in position. |
| [Drift Corrector](DriftCorrector) | Remove drift from the molecule positions. |
| [MSD Calculator](MSDCalculator) | Add mean squared displacement parameter for molecules. |
| [Merge Archives](MergeArchives) | Merges a set of MoleculeArchives. |
| [Merge Virtual Stores](MergeVirtualStore) | Merges a set of virtual MoleculeArchives. |

### Table

| :----------------------------- | :----------- |
| [Import table](ImportTable) | Load csv or yamt format tables. |
| [Sort](Sort) | Sort table rows by column values. |
| [Filter](Filter) | Filter table rows. |
| [Build histogram](BuildHistogram) | Bin observations to generate a histogram table |

### Image processing

| :----------------------------- | :----------- |
| [Peak Finder](./ImageProcessing/PeakFinder) | Counting and subpixel localization of molecules. |
| [Peak Tracker](PeakTracker) | Find, subpixel localize, and track molecules. |
| [Beam Profile Corrector](BeamProfileCorrector) | Correct images for non-uniform beam profile. |
| [Molecule Integrator](MoleculeIntegrator) | Integrate the fluorescence of molecules. |
| [Discoidal Averaging Filter](DiscoidalAveragingFilter) | Filter images to sharpen peaks. |
| [Gradient Calculator](GradientCalculator) | Calculate the vertical gradient for images. |
| [Overlay Channels](OverlayChannels) | Combine different colors into one image set. |

### KCP

| :----------------------------- | :----------- |
| [Change Point Finder](ChangePointFinder) | Find kinetic changes points for a MoleculeArchive. |
| [Single Change Point Finder](SingleChangePointFinder) | Find single change point positions. |
| [Segment Distribution Builder](SegmentDistributionBuilder) | Build distributions using change point segments. |
| [Sigma Calculator](SigmaCalculator) | Estimate background sigma level during optimization of change point detection. |

## <a name="gui"></a>Molecule Explorer Reference
