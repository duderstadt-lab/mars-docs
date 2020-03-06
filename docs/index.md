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
| [Build archive from table](./Molecule/BuildArchiveFromTable) | Generate a MoleculeArchive using a table with a molecule index column. |
| [Region Difference Calculator](./Molecule/RegionDifferenceCalculator) | Add parameter for difference between two regions. |
| [Add time](./Molecule/AddTime) | Retrieve and add time information from metadata. |
| [Drift Calculator](./Molecule/DriftCalculator) | Calculate drift in position. |
| [Drift Corrector](./Molecule/DriftCorrector) | Remove drift from the molecule positions. |
| [MSD Calculator](./Molecule/MSDCalculator) | Add mean squared displacement parameter for molecules. |
| [Merge Archives](./Molecule/MergeArchives) | Merges a set of MoleculeArchives. |
| [Merge Virtual Stores](./Molecule/MergeVirtualStore) | Merges a set of virtual MoleculeArchives. |

### Table

| :----------------------------- | :----------- |
| [Import table](./Table/ImportTable) | Load csv or yamt format tables. |
| [Sort](./Table/Sort) | Sort table rows by column values. |
| [Filter](./Table/Filter) | Filter table rows. |
| [Build histogram](./Table/BuildHistogram) | Bin observations to generate a histogram table |

### Image processing

| :----------------------------- | :----------- |
| [Peak Finder](./Image/PeakFinder) | Counting and subpixel localization of molecules. |
| [Peak Tracker](./Image/PeakTracker) | Find, subpixel localize, and track molecules. |
| [Beam Profile Corrector](./Image/BeamProfileCorrector) | Correct images for non-uniform beam profile. |
| [Molecule Integrator](./Image/MoleculeIntegrator) | Integrate the fluorescence of molecules. |
| [Discoidal Averaging Filter](./Image/DiscoidalAveragingFilter) | Filter images to sharpen peaks. |
| [Gradient Calculator](./Image/GradientCalculator) | Calculate the vertical gradient for images. |
| [Overlay Channels](./Image/OverlayChannels) | Combine different colors into one image set. |

### KCP

| :----------------------------- | :----------- |
| [Change Point Finder](./KCP/ChangePointFinder) | Find kinetic changes points for a MoleculeArchive. |
| [Single Change Point Finder](./KCP/SingleChangePointFinder) | Find single change point positions. |
| [Segment Distribution Builder](./KCP/SegmentDistributionBuilder) | Build distributions using change point segments. |
| [Sigma Calculator](./KCP/SigmaCalculator) | Estimate background sigma level during optimization of change point detection. |

## <a name="gui"></a>Molecule Explorer Reference
