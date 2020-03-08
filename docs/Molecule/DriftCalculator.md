---
layout: molecule
title: Drift Calculator
permalink: /docs/molecule/DriftCalculator/index.html
---
This command calculates the sample drift given a MoleculeArchive and a tag corresponding to all immobile molecules. Then calculates the mean x and y position for each slice from those immobile molecules and places the results into each ImageMetaData record's datatable with the column headings x_drift and y_drift:
<img align='center' src='{{site.baseurl}}/docs/molecule/img/ImageMetaData Drift.png' width='750' />

This command can also be used to calculate other kinds of background besides drift. For example, if you have changes in flow, you could add that in a second set of columns to the ImageMetaData table and correct for it.

The command is intended to be used together with the [[DriftCorrector]]. First, you calculate the drift with this command and then you correct for it using the [DriftCorrector](../DriftCorrector).

#### Inputs

* *MoleculeArchive* - MoleculeArchive to calculate drift for.
* *Background Tag* - The tag added to all molecules that should be used to calculate background. For a bead experiment, this tag can be added to all beads that don't reverse beyond a certain point and have an MSD below a certain threshold. Typically for bead experiments the tags are added using custom groovy scripts.
* *Input X (x)* - The input x column to use the build the background trace. Usually this should just be x.
* *Input Y (y)* - The input y column to use the build the background trace. Usually this should just be y.
* *Output X (x_drift)* - The output column name used when the x background is added to the ImageMetaData table. Usually this should just be x_drift.
* *Output Y (y_drift)* - The output column name used when the y background is added to the ImageMetaData table. Usually this should just be y_drift.
* *Use incomplete traces* - Uses all traces for the drift calculation including those that do not have all slices, which sometimes happens when peak fitting fails for a certain frame. If there are no observations for a certain slice, then NaN is used.

<img align='center' src='{{site.baseurl}}/docs/molecule/img/Drift Calculator.png' width='400' />

#### Output

* The MoleculeArchive provided as Input is modified. The drift information is added to the ImageMetaData table since it applies globally to all molecules within the given dataset.

### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run...
final DriftCalculatorCommand driftCalc = new DriftCalculatorCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
driftCalc.setContext(ij.getContext());

//Set all the input parameters
driftCalc.setArchive(archive);
driftCalc.setBackgroundTag("background");
driftCalc.setInputX("x");
driftCalc.setInputY("y");
driftCalc.setOutputX("x_drift");
driftCalc.setOutputY("y_drift");
driftCalc.setUseIncompleteTraces(false);

//Run the Command
driftCalc.run();

//Retrieve output from the command
//In this case, the archive is just modified
//so no output is needed and the line below is not needed
//archive = driftCalc.getArchive();
```
