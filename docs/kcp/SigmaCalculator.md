---
layout: kcp
title: Sigma Calculator
permalink: /docs/kcp/SigmaCalculator/index.html
---

The 'Sigma Calculator' tool calculates the error value in a specific region of all traces in the MoleculeArchive. By either specifying a region of interest manually or selecting it in the plot of a trace the sigma value for a region of interest is calculated. This error value can be used as an input in the 'Change Point Finder' tool.


#### Inputs
* _MoleculeArchive_ - Select the MoleculeArchive to apply the Sigma Calculator to.
* _X Column_ - X-coordinates (f.e. time or slice).
* _Y Column_ - Y-coordinates (axis of movement).
* _Region_ - Select which region of the trace to use in the calculation: all slices, defined below, defined in Molecules or defined in Metadata.  
When selecting the option 'Defined below':
  * _from_ - Start slice number.
  * _to_ - End slice number.
  * _Region from MarsRecord_ -


<img src='{{site.baseurl}}/docs/kcp/img/img7.png' width='450' />


#### Output
* _y_sigma_ - The value of the calculated error at the specified settings as a parameter to each Molecule entry.


<img src='{{site.baseurl}}/docs/kcp/img/img8.png' width='250' />


#### How to run this Command from a groovy script
```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run
final SigmaCalculatorCommand sigCalc = new SigmaCalculatorCommand();

//Populates @Parameters Services etc. using the current context which we get from the ImageJ Input
sigCalc.setContext(ij.getContext());

//Run the Command
sigCalc.run();

```
