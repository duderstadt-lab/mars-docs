---
layout: molecule
title: Region difference calculator
permalink: /docs/molecule/RegionDifferenceCalculator/index.html
---

#### Inputs

   * *MoleculeArchive* - MoleculeArchive input to use for the difference calculation. This command will calculate the difference between the regions specified for all molecules in the archive and add the values for each molecule as the parameter specified below.
   * *X Column* - X column for the purposes of specifying the reversal regions.
   * *Y Column* - Y column used for calculating the difference.
   * *Region 1 start* - Beginning of the first region.
   * *Region 1 end* - End of the first region.
   * *Region 2 start* - Beginning of the second region.
   * *Region 2 end* - End of the second region.
   * *Parameter Name* - Name of the Parameter added to each molecule entry in the archive with the calculated difference.
<img align='center' src='{{site.baseurl}}/docs/Molecule/img/Region Difference Calculator.png' width='600' />

#### Output

   * The MoleculeArchive provided as Input is modified.

#### Example Molecule for inputs above

<img align='center' src='{{site.baseurl}}/docs/Molecule/img/regionDifferencePlot.png' width='400' />

Running the command on an archive with this molecule results in the Parameter "first_reversal" with value 4.43... being added for this molecule.

<img align='center' src='{{site.baseurl}}/docs/Molecule/img/Added Parameter.png' width='400' />

### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;
import de.mpg.biochem.mars.table.*;

//Make an instance of the Command you want to run...
final RegionDifferenceCalculatorCommand regionDifference = new RegionDifferenceCalculatorCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
regionDifference.setContext(ij.getContext());

//Set all the input parameters
regionDifference.setArchive(archive);
regionDifference.setXcolumn("slice");
regionDifference.setYcolumn("x");
regionDifference.setRegion1Start(40);
regionDifference.setRegion1End(60);
regionDifference.setRegion2Start(80);
regionDifference.setRegion2End(120);
regionDifference.setParameterName("first_reversal");

//Run the Command
regionDifference.run();

//Retrieve output from the command
//In this case, the archive is just modified
//so no output is needed and the line below is not needed
//archive = regionDifference.getArchive();
```
