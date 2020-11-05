---
layout: molecule
title: Build DNA archive
permalink: /docs/molecule/BuildDNAarchive/index.html
---

This command converts ???

#### Inputs
* *Search radius around DNA* -
* *DNA length in bps* - Enter the length of the imaged DNA in bps.
* *x column* -
* *y column* -
* *SingleMoleculeArchive 1* - Select the first SingleMoleculeArchive to be matched to these DNA molecules.
* *SingleMoleculeArchive 1 Name* - Enter the name of the MoleculeArchive (f.e. the name of the measured protein that is tracked in this archive).
* *SingleMoleculeArchive 2* - Select the first SingleMoleculeArchive to be matched to these DNA molecules.
* *SingleMoleculeArchive 2 Name* - Enter the name of the MoleculeArchive (f.e. the name of the measured protein that is tracked in this archive).
* *SingleMoleculeArchive 3* - Select the first SingleMoleculeArchive to be matched to these DNA molecules.
* *SingleMoleculeArchive 3 Name* - Enter the name of the MoleculeArchive (f.e. the name of the measured protein that is tracked in this archive).


<div style="text-align: center"><img  src='{{site.baseurl}}/docs/molecule/img/img6.png' width='400'/></div>

#### Outputs
* *DNAArchive* -  ??


### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run.
final BuildDnaArchiveCommand dnaArchive = new BuildDnaArchiveCommand();

//Populates @Parameters Services etc. using the current context
//which we get from the ImageJ input.
addTime.setContext(ij.getContext());

//Set all the input parameters
dnaArchive.setDNASearchRadius(0)
dnaArchive.setDNALength(21236)
dnaArchive.setXColumn("x_drift_corr")
dnaArchive.setYColumn("y_drift_corr")
dnaArchive.setArchive1(archive1.yama)
dnaArchive.setArchive1Name("mol1")
dnaArchive.setArchive2(archive2.yama)
dnaArchive.setArchive2Name("mol2")
dnaArchive.setArchive3(archive3.yama)
dnaArchive.setArchive3Name("mol3")

//Run the Command
dnaArchive.run();


```
