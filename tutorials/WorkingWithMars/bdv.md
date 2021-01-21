---
layout: workingwithmars
title: "How to Find Tracked Molecules in the Video"
permalink: /tutorials/workingwithmars/bdv/index.html
---

_level: advanced, duration: 10 min_

This tutorial explores the **Rover** option to show a tracked molecule in the original video. This eminently useful function can be used for visual examination of the molecule after analysis for validation of the observed behavior. This would rule out flaws in tracking having a major impact on the drawn conclusion from the data.

The video viewer is based on [BigDataViewer](https://imagej.net/BigDataViewer#Description)<sup>1</sup> and as such requires converting the video of interest to the BigDataViewer file format XML/HDF5 or in the [XML/N5](https://github.com/saalfeldlab/n5) format. [Instructions](https://imagej.net/BigDataViewer#Exporting_Datasets_for_the_BigDataViewer) to convert your video to the XML/HDF5 format can be found in the BigDataViewer documentation.

Steps 1-3 are required to set up the connection between the Molecule Archive and the video. These only have to be executed once per Archive and can then be saved to the Archive for later reference. Please note that the explicit file path is saved, meaning that if the video files were to be stored at a different position, the file path would need to  be updated. If the connection has been established and saved, step 4 and 5 can be executed independently. To start, download the ['TestVideo.tif'](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files/Working%20with%20Mars) dataset from the [git tutorials repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files/Working%20with%20Mars).

### 1. Converting the Video to XML/H5 or XML/N5 Format  
First open the video of interest in Fiji as a stack. Next select the BigDataViewer plugin and select the function "Export Current Image as XML/HDF5" or select "Export Current Image as XML/N5". This will open a dialogue in which the export path has to be set. Press ok and wait until the export process has finished. Find the exported video at the specified file path.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img1.png' width='450'/></div>
<div style="text-align: center">
<img  src='{{site.baseurl}}/tutorials/img/bdv/img2.png' width='350'/>
<img  src='{{site.baseurl}}/tutorials/img/bdv/img14.png' width='350'/></div>


For further information and in-depth documentation about this software and file format please have a look in the [BigDataViewer documentation](https://imagej.net/BigDataViewer#Exporting_Datasets_for_the_BigDataViewer). The XML and H5 files for TestVideo.tif can also be found in the [tutorial repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files/Working%20with%20Mars).

### 2. Coupling the file to the Molecule Archive
The next step is to couple the generated HDF5 or N5 file to the corresponding  Archive. Open the Molecule Archive (Plugins>Molecule Archive Suite>Molecule>Open Archive) and go to the 'Metadata' tab. Move to the 'Bdv Views' tab in the middle window and move to text input field. (see circles on the image) Provide the desired name tag and press the + button. Select the file and continue. Make sure to select the correct format on the button left of the entry field by clicking on it to switch between N5 and HD5.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img3.png' width='450'/></div>

This will add the link to the XML file to the Molecule Archive. A row should appear in the 'Bdv Views' tab showing the provided name tag, some parameter columns and the file path to the XML file.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img4.png' width='600'/></div>

Note that during this coupling step the path to the file is specified. When changing the location of the video on the computer, a new coupling to the new location of the file has to be set up.

### 3. Setting ROI Parameters for each Tracked Molecule
The next step is to assign ROI parameters (roi_x and roi_y) that refer to the average position of the tracked molecule (in case of a linear movement this is equal to the middle of the trace). These ROI parameters are later provided to the video viewer to find the molecule of interest in the video. To retrieve these parameters for each molecules run the following script. Check the [Introduction to Groovy Scripting](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/introduction-to-groovy-scripting/) tutorial for a detailed introduction on how to run a script.

```groovy
#@ MoleculeArchive archive

archive.molecules().forEach{molecule ->
     molecule.setParameter("roi_x", molecule.getTable().mean("x"))
     molecule.setParameter("roi_y", molecule.getTable().mean("y"))
}

```

To run the script go to file>new>script in the main Fiji menu, paste the script and save it as .Groovy file to set the language. Then press run to execute it. This will open a dialogue in which the Molecule Archive has to be selected.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img5.png' width='400'/></div>
<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img6.png' width='300'/></div>

Check if the roi_x and roi_y parameters are added to the molecules by moving to the Molecule tab in **Rover** and pressing the 'parameter' tab. Save the Molecule Archive (File>Save) to keep the established connection with the video for future times the Archive is opened.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img7.png' width='600'/></div>

### 4. Show the Tracked Molecule in the Video
Now a connection between the MoleculeArchive and the XML file has been established and the roi coordinates for each molecule have been defined, the tracked molecule can be viewed in the original video. To do so, select a molecule of interest in **Rover** by selecting it in the 'Molecule' tab. Then open the video tool via Tools>Show Video in the top menu bar of Rover (see screenshot below). Select roi_x and roi_y in the pop-up dialogue as parameters.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img8.png' width='450'/></div>
<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img9.png' width='200'/></div>

After pressing OK this opens the video at the position of the selected molecule. Move through the different time points (frames) with the slider below to see the movement of the molecule over time. To move to a different molecule keep the video window opened and click another UUID in **Rover**.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img10.png' width='400'/></div>

### 5. Export the Video
To save the video showing the molecule of interest use the 'Export Video' tool in the 'Tools' menu in **Rover**. Make sure to have the video open on the molecule of interest, then open the 'Export Video' tool in the 'Tools' menu. Stick with the default settings for the parameters in the popped-up dialogue and press OK.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img11.png' width='450'/></div>
<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img12.png' width='200'/></div>

A new Fiji window will open with the exported video. Use the main Fiji functions to adjust f.e. the brightness of the video and save.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/bdv/img13.png' width='400'/></div>


<sup>1</sup> Pietzsch, T.; Saalfeld, S. & Preibisch, S. et al. (2015), "BigDataViewer: visualization and processing for large image data sets", _Nature Methods_ **12(6)**: 481-483, PMID 26020499
