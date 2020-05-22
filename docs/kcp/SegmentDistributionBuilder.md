---
layout: kcp
title: Segment Distribution Builder
permalink: /docs/kcp/SegmentDistributionBuilder/index.html
---

Coming soon.
- Input: update based on changes in the software.

---

The segment distribution builder is a tool to generate XY tables from the obtained segments tables after change point analyses to allow for the easy plotting of distributions. On top of that, it gives access to a bootstrapping tool determining the error on the obtained distributions.

#### Inputs
Run the change point analysis first before using this tool.
* _MoleculeArchive_ - MoleculeArchive selection.
* _Segments_ - Select on which segment table the tool has to be applied.
* _Start_ - Minimum x-axis value, start point of the distribution described in the obtained XY table.
* _End_ - Maximum x-axis value, end point of the distribution described in the obtained XY table.
* _Bins_ - Amount of bins to use in the distribution.
* _Distribution type_ - Select the distribution to use. Options:
  * _Rate (Gaussian)_ - Plot the rates (slopes of the fitted segments) based on their Gaussian distributions. This allows for the error to be considered in the obtained distribution.
  * _Rate (Histogram)_ - Plot the rates (slopes of the fitted segments) as a histogram.
* _Only regions with rates_ - Select if only regions within a specified range of rates should be considered.
  * _from_ - Lower limit of the specified rates range.
  * _to_ - Higher limit of the specified rates range.
* _Bootstrapping_ - Select 'None', 'Segments' or 'Molecules'. Used to calculate the error on the obtained distribution based on entire Molecule records or on the observed Segments.
  * _Number of cycles_ - Number of cycles to be run by the Bootstrapping algorithm.
* _Include_ - Choose to include 'all', 'tagged with' or 'untagged' molecules in the analysis.
* _Tags (comma separated list)_ - List of tags to base on which traces are included in the analysis.


<img src='{{site.baseurl}}/docs/kcp/img/img9.png' width='450' />

#### Output
* Table with XY values to be plotted as histogram or distribution.


#### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run
final SegmentDistributionBuilderCommand SegBu = new SegmentDistributionBuilderCommand();

//Populates @Parameters Services etc. using the current context which we get from the ImageJ Input
SegBu.setContext(ij.getContext());

//Set all the input parameters
SegBu.setArchive(archive);
SegBu.setBootstrapCycles(100);
SegBu.setBootstrappingType("None");
SegBu.setDistType("Gaussian");
SegBu.setEnd(3);
SegBu.setFilter(True);
SegBu.setFilterRegionEnd(100);
SegBu.setFilterRegionStart(0);
SegBu.setInclude("All");
SegBu.setSegmentsTableName("Table");
SegBu.setStart(0);
SegBu.setTags("Active");

//Run the Command
SegBu.run();

```
