---
layout: kcp
title: Single Change Point Finder
permalink: /docs/kcp/SingleChangePointFinder/index.html
---

The single change point finder tool allows for the identification of one change point in the trace in the MoleculeArchive. This can be of use if a bleaching step (a stepwise drop in the intensity vs. time plot indicating the bleaching of a fluorophore) has to be identified, or when the kinetics of the process studied only involve one transition.

<div style="text-align: center"><img src='{{site.baseurl}}/docs/kcp/img/img3.png' width="450"/></div>  


#### Inputs
* _MoleculeArchive_ - MoleculeArchive to find the single change point for.
* _X Column_ - X column input (f.e. time).
* _Y Column_ - Y column input (axis along which displacement takes place).
* _Analyze region_ - Tick this box if only a specified region of the trace should be analyzed.
* _Region source_ - Location of the defined region to use: Molecules or Metadata.
* _Region_ - Name of the region to use.
* _Fit steps (zero slope)_ - Tick box in case only lines with slope = 0 should be fitted. Use this option to for example investigate step-wise bleaching traces.
* _Add segments table_ - Generate a segments table as output of this command. This table contains the coordinates of the change point, the slopes and corresponding errors.
* _Add position_ - Show the position of the identified change point with an indicator in the plotted trace.
* _Position_ - Name for the position.
* _Include_ - Choose to include 'all', 'tagged with' or 'untagged' molecules in the analysis.
* _Tags (comma separated list)_ - List of tags to base on which traces are included in the analysis.

<img src='{{site.baseurl}}/docs/kcp/img/img6.png' width='450' />


#### Output
* _Segments table_ - If the 'Add segments table' input is enabled, a segments table is generated for each analyzed molecule containing the coordinates of the identified change point, the calculated slopes, intercepts and corresponding error values.
* _Position_ - If the 'Add position' input is enabled the position of the identified change point is shown with a position marker in the plotted trace.


#### How to run this Command from a groovy script
```Groovy
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
scpCalc.setAnalyzeRegion(False);
scpCalc.setRegionSource("Molecules");
scpCalc.setRegion("name");
scpCalc.setFitSteps(False);
scpCalc.setAddSegmentsTable(True);
scpCalc.setAddPosition(True);
scpCalc.setPosition("test_name");
scpCalc.setIncludeTags("Active");

//Run the Command
scpCalc.run();

```
