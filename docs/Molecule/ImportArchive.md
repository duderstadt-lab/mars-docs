---
layout: molecule
title: Import Archive
permalink: /docs/Molecule/ImportArchive/index.html
---

#### Inputs
   * "MoleculeArchive (.yama)" - The full path to the file that contains the archive you want to open. This must be a yama formated json file. This should should then have the extension .yama but if the correct format it will still open without that. Also, the smile encoded and/or text format files are both ok as input. Usually .yama files are store smile encoded to reduce the file size.
   * "Use virtual memory" - Will load the MoleculeArchive into a virtual store in the same folder as the .yama file but with the additional extension .store. This means all read and write operations will work from this file. This option offers fast recovery from an Fiji (ImageJ) or system crash and also offers the ability to work with really big files only using a small amounts of ram. See the ----TODO MoleculeArchive general introduction for more details.
   * If you previously opened this archive virtually, aftering hitting OK, a second dialog will appear:
   * This offer the option of recovering the archive from the store and continuing to work from the same store. This can be very helpful in a case of a system or ImageJ crash preventing lost work. However, this option triggers a review of the store to determine if any records were corrupted and can take a little while for very large files. When the store if made the first time the available size is based on the yama file given. There is room for growth beyond that size, but if you increased the size more than 10% it is best to save the archive and rebuild the store. If you continually reopen the same store and dramatically increase its size, at some point records may be lost during the recovery process and performance will not be optimal.
#### Output
   * A MoleculeArchive loaded for the yama file given.
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
