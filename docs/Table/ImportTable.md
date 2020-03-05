---
layout: table
title: Import Table
permalink: /docs/Table/ImportTable/index.html
---
This command opens a file dialog in which you can select the comma or tab delimited ResultsTable you would like to open. This can be a ResultsTable saved previously using Mars or a table generated elsewhere. In either case, all columns need headers and only comma and tab delimited table files are supported.
#### Inputs
   * "File to open" - The file with the table data you would like to open.
#### Output
   * A MarsTable will be opened in a new window. For general information about how to work with this table window and links to there useful docs see xxx.
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
