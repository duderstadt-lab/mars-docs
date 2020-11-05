---
layout: molecule
title: Drift Calculator
permalink: /docs/molecule/DriftCalculator/index.html
---
This command calculates the sample drift given a MoleculeArchive and a tag corresponding to all immobile molecules. Then calculates the mean or median x and y position for each slice from those immobile molecules and places the results into the metadata specified per plane as xDrift, yDrift and zDrift.

The command is intended to be used together with the [DriftCorrector](../DriftCorrector). First, you calculate the drift with this command and then you correct for it using the [DriftCorrector](../DriftCorrector).

#### Inputs

* *MoleculeArchive* - MoleculeArchive to calculate drift for.
* *Background Tag* - The tag added to all molecules that should be used to calculate background.
* *Input X (x)* - The input x column to use the build the background trace. Usually this should just be x.
* *Input Y (y)* - The input y column to use the build the background trace. Usually this should just be y.
* *Use incomplete traces* - Uses all traces for the drift calculation including those that do not have all slices, which sometimes happens when peak fitting fails for a certain frame. If there are no observations for a certain slice, then NaN is used.
* *mode* - Select between using the 'mean' or 'median' x and y position.
* *Zero* - Select between treating the starting point or then end point as zero.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/molecule/img/Drift Calculator.png' width='400'/></div>

#### Output

* The MoleculeArchive provided as Input is modified. The drift information is added to the metadata table 'Plane' since it applies globally to all molecules within the given dataset. In the example below the drift in all directions is 0 for the plane selected.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/molecule/img/img4.png' width='650'/></div>

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
driftCalc.setUseIncompleteTraces(false);
driftCalc.setMode("mean");
driftCalc.setZeroPoint("end");

//Run the Command
driftCalc.run();

//Retrieve output from the command
//In this case, the archive is just modified
//so no output is needed and the line below is not needed
//archive = driftCalc.getArchive();
```
