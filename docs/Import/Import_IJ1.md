---
layout: import
title: Import IJ1 table
permalink: /docs/import/Import_IJ1/index.html
---

Command to import any ImageJ1 table (such as ResultsTable) to the MarsTable format. This allows for a smooth interaction between the Mars-related commands as well as table features (see also the [MarsTable tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/marstable/)). By default, the MarsTable also gives access to the plotter and the scriptable widgets.

#### Inputs
* *ImageJ1 table* - Table that should be converted. This needs to be an active table in the ImageJ1 format. In the example below a ResultsTable generated with the 'Measure' tool is used to show the conversion.

<img align='center' src='{{site.baseurl}}/docs/Import/img/img2.png' width='400' />
<img align='center' src='{{site.baseurl}}/docs/Import/img/img1.png' width='400' />

#### Outputs
* *MarsTable* - The generated output is a MarsTable.

<img align='center' src='{{site.baseurl}}/docs/Import/img/img3.png' width='400' />

### How to run this Command from a groovy script

```groovy
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run.
final AddTimeCommand addTime = new AddTimeCommand();

//Populates @Parameters Services etc. using the current context
//which we get from the ImageJ input.
addTime.setContext(ij.getContext());

//Set all the input parameters
addTime.setArchive(archive);

//Run the Command
addTime.run();

```
