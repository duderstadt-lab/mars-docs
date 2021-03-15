---
layout: marsrover
title: Mars Rover BigDataViewer
permalink: /docs/MarsRover/BDV/index.html
---

Mars Rover has the option to show an identified and/or tracked molecule in the original video through the in built BigDataViewer functionality. This is a very useful function to be used for the visual inspection of molecules after analysis and gives the user the opportunity to validate their analysis visually.

The in-build video viewer is based on the [BigDataViewer](https://imagej.net/BigDataViewer#Description)<sup>1</sup> Fiji plugin. It can work with videos in either the XML/HDF5 or the [N5]((https://github.com/saalfeldlab/n5))<sup>2</sup> data formats. This [tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/bdv/) can be referenced to find information on how to convert your video to a video in either the N5 or XML/HDF5 formats. We recommend using the N5 format for larger videos as this format allows for multi-threaded reading and writing and volatile viewing options. This significantly decreases the loading time and computing load in general.

- Window to couple file - what do all parameters mean
- BDV window
- Setting menu on the left








<sup>1</sup> Pietzsch, T.; Saalfeld, S. & Preibisch, S. et al. (2015), "BigDataViewer: visualization and processing for large image data sets", _Nature Methods_ **12(6)**: 481-483, PMID 26020499

<sup>2</sup> Heinrich, L.; Bennett D. & Ackermann D. et al. (2020), "Automatic whole cell organelle segmentation in volumetric electron microscopy", _BioRXiv_, DOI: https://doi.org/10.1101/2020.11.14.382143
