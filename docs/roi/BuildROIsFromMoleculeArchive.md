---
layout: roi
title: Build ROIs from MoleculeArchive
permalink: /docs/roi/BuildROIsFromMoleculeArchive/index.html
---
This command allow you to create box ROIs in the ROIManager corresponding to the mean position of each molecule in a MoleculeArchive. Input options allow you to define the box dimensions and archive. This command is intended to be used after the [PeakTracker](../image/PeakTracker) to extract videos for each tracked molecule. For this use, you would run the [SaveROIs](SaveROIs) command after running this command. This would create individual small videos for each molecule. They could then be opened individually or using the [OpenROIs](OpenROIs) command. This command will provide a grid and allows for sorting of videos.

#### Inputs

<img align='center' src='{{site.baseurl}}/docs/roi/img/Build ROIs from MoleculeArchive Dialog.png' width='500' />

* *tags* - Tags used to filter the MoleculeArchive. Only molecules with the tags in the comma separated list will have boxes created for them.
* *x0* - Upper left corner x position of box. Should be a negative offset based on the  mean x, y position.
* *y0* - Upper left corner y position of the box. Should be a negative offset based on the  mean x, y position.
* *width* - Width of the box. Should be twice the x0 offset value to center the molecule in the box.
* *height* - Height of the box. Should be twice the y0 offset value to center the molecule in the box.

#### Output

* *Box ROIs in the RoiManager* - Box ROIs with the specified dimensions entered on the mean position of each molecule in the molecule archive will be added to the ROIManager with UIDs as names.

### How to run this Command from a groovy script
I don't think you would ever want to run this in a script, but this can be added if needed. It might make more sense to directly script this functionality without using the plugin.

This command is very useful for manually inspecting videos generated for each molecule and then tagging manually picked molecules from the archive (see [[AnalysisProtocolID00002]]).
