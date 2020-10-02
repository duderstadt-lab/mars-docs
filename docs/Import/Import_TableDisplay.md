---
layout: import
title: Import TableDisplay
permalink: /docs/import/Import_TableDisplay/index.html
---

Command to import any Scijava table (such as TableDisplay) to the MarsTable format. This allows for a smooth interaction between the Mars-related commands as well as table features (see also the [MarsTable tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/marstable/)). By default, the MarsTable also gives access to the plotter and the scriptable widgets.

#### Inputs
* *ImageJ1 table* - Table that should be converted. This needs to be an active table in the SciJava format (TableDisplay).

<img align='center' src='{{site.baseurl}}/docs/Import/img/img4.png' width='400' />
<img align='center' src='{{site.baseurl}}/docs/Import/img/img5.png' width='400' />

#### Outputs
* *MarsTable* - The generated output is a MarsTable.

<img align='center' src='{{site.baseurl}}/docs/Import/img/img6.png' width='400' />

### How to run this Command from a groovy script

```groovy
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run.
final AddTimeCommand addTime = new AddTimeCommand();

//Populates @Parameters Services etc. using the current context
addTime.setContext(ij.getContext());

//Set all the input parameters
addTime.setArchive(archive);


//Run the Command
addTime.run();

```
