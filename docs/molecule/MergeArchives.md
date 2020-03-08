---
layout: molecule
title: Merge Archives
permalink: /docs/molecule/MergeArchives/index.html
---
This command merges multiple MoleculeArchives (yama files) into one. To ensure efficient memory usage, and allow for arbitrarily large archives to be combined, this command works in streaming mode. Given a directory, all the MoleculeArchiveProperties and ImageMetaData information is read into memory from all files. Then these records are checked to ensure no duplicate datasets exist. If duplicates exist the merge is aborted. Assuming all datasets are unique a new merged.yama file is created with new MoleculeArchiveProperties and all ImageMetaData items. Then molecules are read in and written out one by one to ensure only a single molecule record is held in memory any given time. It is assumed that all molecule UIDs are unique based on the uniqueness of the ImageMetaData UIDs.

#### Inputs

* *Directory* - A directory containing multiple .yama files you would like to merge (They can be either txt or smile encoded or a combination of the two). Can be as many as you like but they must all contain unique datasets (unique ImageMetaData UIDs). If two archives have ImageMetaData items with the same UIDs the merge process will be aborted until the conflict is resolved.
* *Use smile encoding* - The encoding of the output yama file. Usually smile encoding is preferred because it will results in smaller file sizes. However, if export to another software is required plain txt JSON is preferred.

<img align='center' src='{{site.baseurl}}/docs/molecule/img/Merge Archives.png' height='250'/>

#### Output

* A merged.yama file will be created in the given directory containing all the ImageMataData and molecules from all .yama files in the directory.

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
merger.setSmileEncoding(true)

//Run the Command
merger.run()

//Output is a merged.yama file in the directory given
```
