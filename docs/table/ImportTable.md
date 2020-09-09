---
layout: table
title: Import Table
permalink: /docs/table/ImportTable/index.html
---
This command opens a file dialog in which you can select the comma or tab delimited table you would like to open. This can be a table saved previously using Mars or a table generated elsewhere. In either case, all columns need headers and only comma and tab delimited table files are supported.

#### Inputs

   * "File to open" - The file with the table data you would like to open.

#### Output

   * A MarsTable will be opened in a new window.

For general information about how to work with tables take a look at the [MarsTables tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/marstable/).

An example of an imported table is shown in the image below.  
<img align='center' src='{{site.baseurl}}/docs/table/img/img1.png' width='400' />

#### How to open tables in a groovy script

```groovy
#@ File file
#@ ImageJ ij
#@output MarsTable table

import de.mpg.biochem.mars.table.*

//Make an instance of the Command you want to run...
final ResultsTableOpenerCommand tableOpener = new ResultsTableOpenerCommand()

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
tableOpener.setContext(ij.getContext())

//Set all the input parameters
tableOpener.setFile(file)

//Run the Command
tableOpener.run()

//Retrieve output from the command
table = tableOpener.getTable()
```
OR if you have a json table file you can use below.
```groovy
#@ File file
#@output MarsTable table

import de.mpg.biochem.mars.table.*

//Expects a json formatted file.
table = new MarsTable(file)

```
