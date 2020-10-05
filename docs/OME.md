---
layout: OME
title: Open Microscopy Environment - OME
permalink: /docs/OME/index.html
---

Mars Molecule Archives store image metadata using an enhanced version of the [Open Microscopy Environment (OME)](https://link.springer.com/article/10.1186/gb-2005-6-5-r47) format. Data collection and storage is based on a 6-coordinate framework: Image, X, Y, Z (slice), T (time) and C (channel). This represents image acquisitions involving multiple positions, at different time points, and for different channels (f.e. different colors of excitation lasers for multi-color experiments) and, if desired, through different slices of an object (Z). Each recorded x-y field is called a plane, and is specified by its T, Z and C coordinates ( **plane(T,Z,C)** ).


<img align='center' src='{{site.baseurl}}/docs/OME/img/img1.png' width='750' />


MarsMetadata records store this information in MarsOMEImages for each position, which in turn contain MarsOMEPlanes for each set of coordinates sampled. These objects are capable of holding a wealth of information about the state of the microscope over time (i.e. temperature, physical position, delta time, gain, exposure, etc.) in a structured manner with proper units as well as arbitrary image and plane information using key-value pairs.    

MarsMetadata records have many enhancements beyond the basic OME format. They inherit all the features of MarsRecords that includes uuid identifiers, tags, notes, multi-type key-value parameters as well as MarsRegions and MarsPositions used for highlighting events in time for further analysis via scripts. MarsMetadata records also contain a log with the entire processing history that records all mars commands run on the archive. Finally, integration with the BigDataViewer is facilitated through the storage of BDVSource objects containing the location of hd5 image files, 3D affine coordinate transformation to apply, and whether to apply drift correction across all positions using information calculating for all planes.

[Merging](/docs/molecule/MergeArchives/) allows for Molecule Archives to store multiple MarsMetadata records containing unique classifiers for fine-grained analysis across experiments. This provides a seventh dimension to the OME framework. As yet another enhancement and to provide optimal performance within Mars, all this information, including the OME specification, is saved to file in smile-encoded or plain-text JSON as opposed to the xml typically used for OME storage.

### How to convert data to the OME format

If you open your videos with SCIFIO as outlined [on the main docs page](/docs/), it will do the work for you using build-in translators from many different formats to OME. If you open your images as plain old ImageJ videos, Mars will attempt to build the OME record from scratch using the available dimension information. Molecule Archives can be created from scratch within scripts. In that case, MarsMetadata records can be populated manually or with an OMEXMLMetadata object generated either from SCIFIO or from BioFormats. Example scripts to come. Please feel free to give us a nudge by posting on the ImageJ forum.
