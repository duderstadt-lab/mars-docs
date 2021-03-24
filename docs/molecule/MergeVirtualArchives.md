---
layout: molecule
title: Merge Virtual Archives
permalink: /docs/molecule/MergeVirtualArchives/index.html
---
This command merges multiple virtual Molecule Archives (.yama.store directories) into one. Records are checked to ensure no duplicate datasets exist. If duplicates exist the merge is aborted. Assuming all datasets are unique a new merged.yama.store directory is created with new MoleculeArchiveProperties and all MetaData items. It is assumed that all molecule UIDs are unique based on the uniqueness of the MetaData UIDs.

#### Inputs

* *Directory* - A directory containing multiple .yama.store directories you would like to merge (They can be either json or smile encoded or a combination of the two). Can be as many as you like but they must all contain unique datasets (unique MetaData UIDs). If two archives have MetaData items with the same UIDs the merge process will be aborted until the conflict is resolved.
* *Thread count* - Determines how much computing power of your computer will be devoted to this calculation. A higher thread count decreases computing time.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/molecule/img/merge.png' width='400'/></div>

#### Output

* A merged.yama.store directory will be created containing all the Metadata and Molecule records from all .yama.store directories.

### How to run this Command from a groovy script

```groovy
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*

//Make an instance of the Command you want to run...
final MergeVirtualStoresCommand merger = new MergeVirtualStoresCommand()

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
merger.setContext(ij.getContext())

//Set all the input parameters
merger.setDirectory("/Users/me/data")

//Run the Command
merger.run()

//Output is a merged.yama.store directory
```
