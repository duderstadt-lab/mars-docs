---
layout: kcp
title: Segment Distribution Builder
permalink: /docs/kcp/SegmentDistributionBuilder/index.html
---

Coming soon.
- Introduction
- Input
  - Segments, Start, End, Bins
  - Number of cycles 
- Output


#### Inputs
* _MoleculeArchive_ - MoleculeArchive selection.
* _Segments_ -
  * _Start_ -
  * _End_ -
  * _Bins_ -
* _Distribution type_ - Select the distribution to use. Options: Rate (Gaussian), Rate (Histogram), Duration, Processivity (molecule), and Processivity (region).
* _Only regions with rates_ - Select if only regions within a specified range of rates should be considered.
  * _from_ - Lower limit of the specified rates range.
  * _to_ - Higher limit of the specified rates range.
* _Bootstrapping_ - Select 'None', 'Segments' or 'Molecules'.
* _Number of cycles_ -
* _Include_ - Choose to include 'all', 'tagged with' or 'untagged' molecules in the analysis.
* _Tags (comma separated list)_ - List of tags to base on which traces are included in the analysis.


<img src='{{site.baseurl}}/docs/kcp/img/img9.png' width='450' />

#### Output



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
