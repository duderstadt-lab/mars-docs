---
layout: molecule
title: Build archive from table
permalink: /docs/Molecule/BuildArchiveFromTable/index.html
---

#### Inputs

   * "Table with molecule column" - MarsTable with a molecule column. The molecule column will be used as a definition for each molecule entry in the MoleculeArchive.
<img align='center' src='{{site.baseurl}}/docs/Molecule/img/Input table.png' width='400' />
<img align='center' src='{{site.baseurl}}/docs/Molecule/img/Build archive from table.png' width='400' />

#### Output

   * A MoleculeArchive in which each molecule entry corresponds to a unique molecule number in the input table.
   
### How to run this Command from a groovy script

```groovy
#@ MarsTable table
#@ ImageJ ij
#@output MoleculeArchive archive

import de.mpg.biochem.mars.molecule.*;
import de.mpg.biochem.mars.table.*;

//Make an instance of the Command you want to run...
final BuildArchiveFromTableCommand archiveFromTable = new BuildArchiveFromTableCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
archiveFromTable.setContext(ij.getContext());

//Set all the input parameters
archiveFromTable.setTable(table);

//Run the Command
archiveFromTable.run();

//Retrieve output from the command
archive = archiveFromTable.getArchive();
```

Alternatively, you can just use the MoleculeArchive constructer for creating an archive from a MarsTable:
```groovy
#@ MarsTable table
#@ MoleculeArchiveService moleculeArchiveService
#@output MoleculeArchive archive

import de.mpg.biochem.mars.molecule.*;
import de.mpg.biochem.mars.table.*;

archive = new MoleculeArchive("new archive", table, moleculeArchiveService)
//OR
//archive = new MoleculeArchive("new archive", table)
//With no status updates...
```
