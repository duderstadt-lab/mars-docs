---
layout: workingwithmars
title: "How to Find an Archive Entry back in the Video"
permalink: /tutorials/workingwithmars/bdv/index.html
---

_level: advanced, duration: 10 min_

This tutorial explores the **Rover** option to show a tracked molecule in the original video. This eminently useful function can be used for visual examination of the molecule after analysis for validation of the observed behavior. This would rule out flaws in tracking having a major impact on the drawn conclusion from the data.

The video viewer is based on [BigDataViewer](https://imagej.net/BigDataViewer#Description)<sup>1</sup> and as such requires converting the video of interest to the BigDataViewer file format XML/HDF5. To do so the [instructions](https://imagej.net/BigDataViewer#Exporting_Datasets_for_the_BigDataViewer) supplied in their documentation is used.

Steps 1-3 are required to set up the connection between the MoleculeArchive and the XML video. These only have to be executed once per MoleculeArchive and can then be saved to the MoleculeArchive. If the connection has been established and saved, step 4 and 5 can be executed independently.

### 1. Converting the Video to XML/H5 Format
First open the video of interest in Fiji as a stack. Next select the BigDataViewer plugin and select the function "Export Current Image as XML/HDF5". This will open a dialogue in which the export path has to be set. Press ok and wait until the export process has finished. Find the exported video at the specified file path.

<img src='{{site.baseurl}}/tutorials/img/bdv/img1.png' width='450' />
<img src='{{site.baseurl}}/tutorials/img/bdv/img2.png' width='350' />


For further information and in-depth documentation about this software and file format please have a look in the [BigDataViewer documentation](https://imagej.net/BigDataViewer#Exporting_Datasets_for_the_BigDataViewer). The XML and H5 files for TestVideo.tif can also be found in the tutorial repository.

### 2. Coupling the XML File to the MoleculeArchive
The next step is to couple the generated XML file to the corresponding MoleculeArchive. Open the MoleculeArchive (Plugins>MoleculeArchive Suite>Molecule>Import Archive) and go to the 'Experiments' tab. Move to the 'Bdv Views' tab in the middle window and move to text input field. (see green circles on the image) Provide the desired name tag and press the + button.

<img src='{{site.baseurl}}/tutorials/img/bdv/img3.png' width='450' />

This will add the link to the XML file to the MoleculeArchive. A row should appear in the 'Bdv Views' tab showing the provided name tag, some parameter columns and the file path to the XML file.

<img src='{{site.baseurl}}/tutorials/img/bdv/img4.png' width='600' />

Note that during this coupling step the path to the file is specified. When changing the location of the video on the computer, a new coupling to the new location of the file has to be set up.

### 3. Setting roi Parameters for each Tracked Molecule
The next step is to assign roi parameters (roi_x and roi_y) that refer to the average position of the tracked molecule (in case of a linear movement this is equal to the middle of the trace). These roi parameters are later provided to the video viewer to find the molecule of interest in the video. To retrieve these parameters for each molecules run the following script.

```groovy
#@ MoleculeArchive archive

archive.molecules().forEach{molecule ->
     molecule.setParameter("roi_x", molecule.getDataTable().mean("x"))
     molecule.setParameter("roi_y", molecule.getDataTable().mean("y"))
}

```

To run the script go to file>new>script in the main Fiji menu, paste the script and save it as .Groovy file to set the language. Then press run to execute it. This will open a dialogue in which the MoleculeArchive has to be selected.

<img src='{{site.baseurl}}/tutorials/img/bdv/img5.png' width='400' />
<img src='{{site.baseurl}}/tutorials/img/bdv/img6.png' width='300' />

Check if the roi_x and roi_y parameters are added to the molecules by moving to the Molecule tab in **Rover** and pressing the 'parameter' tab. Save the MoleculeArchive (File>Save) to keep the established connection with the video for future times the Archive is opened.

<img src='{{site.baseurl}}/tutorials/img/bdv/img7.png' width='600' />

### 4. Show the Tracked Molecule in the Video
Now a connection between the MoleculeArchive and the XML file has been established the tracked molecule can be viewed in the video. Select a molecule of interest in **Rover** by selecting it in the 'Molecule' tab. Then open the video tool via Tools>Show Video. Select roi_x and roi_y in the pop-up dialogue as parameters.

<img src='{{site.baseurl}}/tutorials/img/bdv/img8.png' width='450' />
<img src='{{site.baseurl}}/tutorials/img/bdv/img9.png' width='200' />

After pressing OK this opens the video at the position of the selected molecule. Move through the different time points (frames) with the slider below to see the movement of the molecule over time. To move to a different molecule keep the video window opened and click another UID in **Rover**.

<img src='{{site.baseurl}}/tutorials/img/bdv/img10.png' width='400' />

### 5. Export the Video
To save the video showing the molecule of interest use the 'Export Video' tool in the 'Tools' menu in **Rover**. Make sure to have the video open on the molecule of interest, then open the 'Export Video' tool in the 'Tools' menu. Stick with the default settings for the parameters in the popped-up dialogue and press OK.

<img src='{{site.baseurl}}/tutorials/img/bdv/img11.png' width='450' />
<img src='{{site.baseurl}}/tutorials/img/bdv/img12.png' width='200' />

A new Fiji window will open with the exported video. Use the main Fiji functions to adjust f.e. the brightness of the video and save.

<img src='{{site.baseurl}}/tutorials/img/bdv/img13.png' width='400' />


<sup>1</sup> Pietzsch, T.; Saalfeld, S. & Preibisch, S. et al. (2015), "BigDataViewer: visualization and processing for large image data sets", _Nature Methods_ **12(6)**: 481-483, PMID 26020499
