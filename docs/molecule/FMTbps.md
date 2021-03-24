---
layout: molecule
title: FMT Generate bps
permalink: /docs/molecule/FMTbps/index.html
---

This tool was specifically developed to work with flow magnetic tweezer (FMT) data using the set-up as developed in our lab. More information about this set-up and an example of the data collected with it can be found on this [example page](https://duderstadt-lab.github.io/mars-docs/examples/flow-Magnetic-Tweezers/). Java information about the command can be found in the [repository](https://github.com/duderstadt-lab/mars-fmt/blob/master/src/main/java/de/mpg/biochem/mars/molecule/commands/GenerateBPSCommand.java).

[What does this tool calculate exactly? BPS what?]


#### Inputs

[Insert screenshot of the dialog]

* *Molecule Archive* - MoleculeArchive input with the FMT tracking data that has to be converted.
* *X column* - X column to use for the conversion, usually "x".
* *Y column* - Y column to use for the conversion, usually "y".
* *um per pixel* - Specifies the pixel size in um. This is specific per microscope and camera.
* *global bps per um* - Specifies how many basepairs (bps) per um are expected under the current experimental conditions.
* *Use* - Choose between "reversal" and "region". Choose "reversal" in the case the calculation has to be applied to a part of the experiment in which the flow was reversed during the recording. Supply the parameters listed below. Choose "region" if the flow remained constant during the part of the experimet of interest. See further below for "region"-specific settings.

If using reversal regions:
* *Reverse flow start* - T at which the reverse flow started.
* *Reverse flow end* - T at which the reverse flow ended.
* *Forward flow start* - T at which the forward flow started.
* *Forward flow end* - T at which the forward flow ended.
* *Flow start* - T at which the flow started.
* *Flow end* - T at which the flow ended.
* *Bead radius (um)* - Radius of the coupled magnetic bead in um.
* *Conversion type* - Select the conversion type. Choose between "Global"
 and "By molecule".
* *DNA length in bps* - Supply the DNA length of the template DNA in bps.

If using background region:
* *Start* - Starting frame (T) that defines the start of the region of interest for which the calculation has to be executed.
* *End* - Final frame (T) that defines the end of the region of interest for which the calculation has to be executed.

* *Output Column* - Name of the column that displays the output of this command. Usually this is set to 'bps'.


#### Outputs
* Single Molecule Archive - The output of this command is the same SingleMoleculeArchive as was used for the input, but with an additional column stating the bps.


### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run.
final GenerateBPSCommand fmtBPS = new GenerateBPSCommand();

//Populates @Parameters Services etc. using the current context
//which we get from the ImageJ input.
fmtBPS.setContext(ij.getContext());

//Set all the input parameters
fmtBPS.setArchive(archive)
fmtBPS.setXcolumn("x")
fmtBPS.setYColumn("y")
fmtBPS.setUmPerPixel(20)
fmtBPS.setGlobalBpsPerUm(30)
fmtBPS.setUse("reversal")
fmtBPS.setReverseFlowStart(10)
fmtBPS.setReverseFlowEnd(800)
fmtBPS.setForwardFlowStart(810)
fmtBPS.setForwardFlowEnd(900)
fmtBPS.setFlowStart(1000)
fmtBPS.setFlowEnd(1200)
fmtBPS.setBeadRadius(2)
fmtBPS.setConversionType("Global")
fmtBPS.setDNALengthBps(27000)
fmtBPS.setStart(100)
fmtBPS.setEnd(800)
fmtBPS.setOutputColumn("bps")


//Run the Command
fmtBPS.run();


```
