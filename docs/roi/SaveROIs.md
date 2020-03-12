---
layout: roi
title: Save ROIs
permalink: /docs/roi/SaveROIs/index.html
---
This command is used to save small videos of all ROIs currently in the ROIManager. These should be box ROIs centered around molecules of interest that are typically generated using the [Build ROIs from MoleculeArchive](BuildROIsFromMoleculeArchive) command. A directory where the files should be generated is required. The advantage of this command over default Fiji or Mars commands is that each frame is only opened once and all ROI videos are built in parallel. So if you have a 14 GB movie in virtual memory, each frame only needs to pass through memory once. The typical Mars batch approach would result in loading the whole video into memory to generate each ROI. The final output is a collection of tif movies, one for each ROI with a name corresponding to the name from the ROI manager. Typically these names are UIDs, however, you can of course manually add ROIs, name them, and use this command to export them.

If you plan to use the [Open ROIs](OpenROIs) command after running this command, all the ROI exported should have the same dimensions.

#### Inputs

* *ROIs in RoiManager* - There must be ROIs in the ROIManager.
* *Directory* - Running this command will bring up a window to choose a directory for saving all the output ROI video files.

#### Output

* *ROI tif video files* - This command generates tif video files for all ROIs with names corresponding to names in the RoiManager.
