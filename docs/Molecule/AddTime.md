---
layout: molecule
title: Add Time
permalink: /docs/Molecule/AddTime/index.html
---
This command makes a map between the slice and Time columns in all ImageMetaData records and uses that map to add a Time column to all molecule DataTables.

The ImageMetaData record from a Bead experiment collected using the Norpix software look like:
#### <img align='center' src='{{site.baseurl}}/docs/Molecule/img/ImageMetaData table.png' width='750' />

Once a map between slice and Time (s) is established using the table above, it is used to add a Time (s) column below for each molecule record:
#### <img align='center' src='{{site.baseurl}}/docs/Molecule/img/Molecule Time column-01.png' width='750' />

Since a mapping is used, missed slices are not a problem. Also, if multiple dataset are combined in the archive. The slice and Time will only be applied based on the corresponding ImageDataRecord for each molecule. This ensures that molecules from different datasets get the correct time information, and that this information can be completely different between molecule sets and ImageMetaData records inside the same merged archive.

#### Inputs
   * *MoleculeArchive* - MoleculeArchive for which to add a Time (s) column to all molecule records.
<img align='center' src='{{site.baseurl}}/docs/Molecule/img/Add Time.png' width='250' />
#### Output
   * The MoleculeArchive provided as Input is modified.
### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run...
final AddTimeCommand addTime = new AddTimeCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
addTime.setContext(ij.getContext());

//Set all the input parameters
addTime.setArchive(archive);

//Run the Command
addTime.run();

//Retrieve output from the command
//In this case, the archive is just modified
//so no output is needed and the line below is not needed
//archive = regionDifference.getArchive();
```
