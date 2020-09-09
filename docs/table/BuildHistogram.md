---
layout: table
title: Build histogram
permalink: /docs/table/BuildHistogram/index.html
---
This command allows for fast generation of simple histograms. The purpose is to allow for rapid examination of the results of filtering/processing. Rate/Duration distributions can be generated using the [Segment Distribution Builder](../../kcp/SegmentDistributionBuilder) given a MoleculeArchive containing KCP segments. This simple histogram builder doesn't allow you to specify the starting and ending values. Therefore, if you have large outliers that don't cluster with the other values, these may throw off the histogram binning, putting most of the values in to a single bin. You can get around this issue by prefiltering the table by the desired start and end values using the [Filter](../Filter).


#### Inputs
* *Table* - The open MarsTable that contains the column for building the histogram.
* *Column* - The column to use for building the histogram.
* *bins* - Number of bins in the histogram.

<img align='center' src='{{site.baseurl}}/docs/table/img/img3.png' width='450' />

#### Output

* A new MarsTable with the histogram specified as the selected column (x-axis) and Events (y-axis).

### How to run this command in a groovy script

The table used to generate the above histogram is attached. First datable.csv and then histTable.csv as the command output used to make the histogram.

```groovy
#@ ImageJ ij
#@ MARSResultsTable inputTable
#@ String column
#@ Integer bins
#@output MARSResultsTable histTable
import de.mpg.biochem.mars.table.*

//Make an instance of the Command you want to run...
final HistogramBuilderCommand histBuilder = new HistogramBuilderCommand()

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
histBuilder.setContext(ij.getContext())

//Set all the input parameters
histBuilder.setInputTable(inputTable)
histBuilder.setColumnName(column)
histBuilder.setBins(bins)

//Run the Command
histBuilder.run()

//Retrieve output from the command
histTable = histBuilder.getHistogramTable()
```
