---
layout: molecule
title: Import Archive
permalink: /docs/molecule/ImportArchive/index.html
---

#### Inputs

* "MoleculeArchive (.yama or .yama.store)" - The full path to the file or directory that contains the archive you want to open. Mars files are usually smile encoded to reduce the file size, but can also be plain json.

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
 * Depending on the environment you are using - Headless or Jupiter Notebook etc. - this might not be the preferred way to open an archive. One alternative is to just initialise a new MoleculeArchive object using one of the constructors for which you can pass the file path as seen in the javadoc. One option of the following constructor:
 * MoleculeArchive(String name, File file, MoleculeArchiveService moleculeArchiveService, boolean virtual)
 * In this case you would need to get the MoleculeArchiveService as an input and pass it to the archive...
