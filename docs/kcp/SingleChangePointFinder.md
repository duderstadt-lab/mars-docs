---
layout: kcp
title: Single Change Point Finder
permalink: /docs/kcp/SingleChangePointFinder/index.html
---

Coming soon.
- Introduction what the function does
- Position name Input; And other updates to the inputs window
- Outputs; Update after new version

---

The single change point finder tool allows for the identification of one change point in the trace in the MoleculeArchive. This can be of use if a bleaching step (a stepwise drop in the intensity vs. time plot indicating the bleaching of a fluorophore) has to be identified, or when the kinetics of the process studied only involve one transition.

<div style="text-align: center"><img src='{{site.baseurl}}/docs/kcp/img/img3.png' width="450"/></div>  


#### Inputs
* _MoleculeArchive_ - MoleculeArchive to find the single change point for.
* _X Column_ - X column input (f.e. time).
* _Y Column_ - Y column input (axis along which displacement takes place).
* _Position Name_ -
* _Include_ - Choose to include 'all', 'tagged with' or 'untagged' molecules in the analysis.
* _Tags (comma separated list)_ - List of tags to base on which traces are included in the analysis.

<img src='{{site.baseurl}}/docs/kcp/img/img6.png' width='450' />


#### Output


#### How to run this Command from a groovy script
```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run
final SingleChangePointFinder scpCalc = new SingleChangePointFinder();

//Populates @Parameters Services etc. using the current context which we get from the ImageJ Input
scpCalc.setContext(ij.getContext());

//Set all the input parameters
scpCalc.setArchive(archive);
scpCalc.setXcolumn("y");
scpCalc.setYcolumn("slice");
scpCalc.setConfidenceLevel(0.99);
scpCalc.setFitSteps(False);
scpCalc.setGlobalSigma(1.0);


//Run the Command
scpCalc.run();

```
