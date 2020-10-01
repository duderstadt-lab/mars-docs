---
layout: molecule
title: Open Virtual Archive
permalink: /docs/molecule/ImportVirtualArchive/index.html
---

#### Inputs

* "Virtual MoleculeArchive (.yama.store)" - The full path to the file or directory that contains the archive you want to open. Mars files are usually smile encoded to reduce the file size, but can also be plain json.

#### Output

* A MoleculeArchive Window will open.

#### How to run this Command from a groovy script

```groovy
#@ File file
#@ ImageJ ij
#@output MoleculeArchive archive

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run...
final OpenMoleculeArchiveCommand archiveOpener = new OpenMoleculeArchiveCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
archiveOpener.setContext(ij.getContext());

//Set all the input parameters
archiveOpener.setFile(file);
archiveOpener.setVirtual(false);

//Run the Command
archiveOpener.run();

//Retrieve output from the command
archive = archiveOpener.getArchive();
```
