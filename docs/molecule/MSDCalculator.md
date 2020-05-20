---
layout: molecule
title: Variance Calculator
permalink: /docs/molecule/varCalculator/index.html
---

#### Inputs

 * _MoleculeArchive_ - MoleculeArchive to operate on.
 * _Column_ - Column of each molecule datatable to calculate the variance(var) for.
 * _Parameter Name_ - The name of the Parameter that will be added to each molecule record.

<img align='center' src='{{site.baseurl}}/docs/molecule/img/img1.png' width='400' />

#### Output

* The calculated value is added as parameter to each molecule record in the MoleculeArchive.


### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run...
final MSDCalculatorCommand varCalc = new MSDCalculatorCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
varCalc.setContext(ij.getContext());

//Set all the input parameters
varCalc.setArchive(archive);
varCalc.setColumn("x");
varCalc.setParameterName("MSD_x");

//Run the Command
varCalc.run();
```
