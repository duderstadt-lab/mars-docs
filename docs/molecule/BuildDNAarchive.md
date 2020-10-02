---
layout: molecule
title: Build DNA archive
permalink: /docs/molecule/BuildDNAarchive/index.html
---


#### Inputs



#### Outputs



### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run.
final AddTimeCommand addTime = new AddTimeCommand();

//Populates @Parameters Services etc. using the current context
//which we get from the ImageJ input.
addTime.setContext(ij.getContext());

//Set all the input parameters
addTime.setArchive(archive);
addTime.setSource("dt");
addTime.setTimeIncrement(2);

//Run the Command
addTime.run();

//Retrieve output from the command
//In this case, the archive is just modified
//so no output is needed and the line below is not needed
//archive = regionDifference.getArchive();
```
