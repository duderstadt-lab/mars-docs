---
layout: molecule
title: Merge Archives
permalink: /docs/molecule/MergeArchives/index.html
---
This command merges multiple Molecule Archives (yama files) into one. To ensure efficient memory usage, and allow for arbitrarily large archives to be combined, this command works in streaming mode. Given a directory, all the MoleculeArchiveProperties and MetaData information is read into memory from all files. Then these records are checked to ensure no duplicate datasets exist. If duplicates exist the merge is aborted. Assuming all datasets are unique a new merged.yama file is created with new MoleculeArchiveProperties and all MetaData items. Then molecules are read in and written out one by one to ensure only a single molecule record is held in memory any given time. It is assumed that all molecule UIDs are unique based on the uniqueness of the MetaData UIDs.

#### Inputs

* The input of this command is the folder containing the archives that should be merged. When running the command a file explorer window is opened where this directory can be selected.

#### Output

* A merged.yama file will be created in the given directory containing all the MataData and molecule records from all .yama files in the directory.

### How to run this Command from a groovy script

```groovy
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*

//Make an instance of the Command you want to run...
final MergeCommand merger = new MergeCommand()

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
merger.setContext(ij.getContext())

//Set all the input parameters
merger.setDirectory("/Users/me/data")

//Run the Command
merger.run()

//Output is a merged.yama file in the directory given
```
