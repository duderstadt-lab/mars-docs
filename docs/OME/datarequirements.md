---
layout: OME
title: Data Requirements
permalink: /docs/OME/datarequirements/index.html
---

The structure of the data that can be used to build MoleculeArchives on using the **Mars** software and plugins is based on the [Open Microscopy Environment (OME)](https://link.springer.com/article/10.1186/gb-2005-6-5-r47) file format. This open source format provides a universal framework for acquiring and storing of imaging data in biological microscopy experiments.


### The Coordinate Framework
Data collection and storage is based on a 5-coordinate framework: x, y, T (time),Z (slice), and C (channel). This allows for the acquisition of data over time, with different channels (f.e. different colors of excitation lasers for multi-color experiments) and if desired through different slices of an object (Z). Each recorded x-y field is called a plane, and is specified by its T, Z and C coordinates (**plane(T,Z,C)**).


<img align='center' src='{{site.baseurl}}/docs/OME/img/img1.png' width='750' />


### How to convert your data to OME format

Coming soon.
