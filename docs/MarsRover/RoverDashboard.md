---
layout: marsrover
title: Rover Dashboard
permalink: /docs/MarsRover/RoverDashboard/index.html
---

The Rover Dashboard gives an overview of the data collected in the Molecule Archive.
Widgets can be selected from the toolbar with a click. The scripting language can be switched between Groovy or Python in the drop down menu and all widgets can either be cleared from the desktop (bomb icon), stopped loading (square) or refreshed (refresh button) simultaneously.

<img align='center' src='{{site.baseurl}}/docs/img/Rover/img5.png' width='450' />


#### Predefined widgets
<img align='center' src='{{site.baseurl}}/docs/img/Rover/img2.png' width='80' />

The predefined widgets give a first glance at the data. As shown in the example, the
information widget shows the name of the archive, the archive type, the amount of molecules in the archive, the amount of metadata files and which type of memory is used for storage ('Normal' or 'Virtual Mode'). The tag widget shows a bar plot of the amount of molecules that are categorised by a certain tag.
Update the widgets after a change in the archive by pressing the refresh button.

<img align='center' src='{{site.baseurl}}/docs/img/Rover/img4.png' width='450' />


#### Scriptable widgets
<img align='center' src='{{site.baseurl}}/docs/img/Rover/img3.png' width='180' />

The scriptable widgets are available on **Rover** Dashboard and also in the 'Experiments' and 'Molecules' tabs. They allow for a rapid evaluation of the archive, metadata, and molecule features. All scriptable widgets leverage the scripting capabilities of ImageJ to provide limitless customisability. Scripts can be written in Python or Groovy. In both cases, chart scripts are provided with a single input that depends on the dashboard.

At the highest level, the global dashboard receives a reference to the entire [MoleculeArchive](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/MoleculeArchive.html) as an input. This provides complete access to all molecules, metadata, and properties, so global relationships can be rapidly evaluated. Dashboards are also provided within the metadata and molecules tabs. These receive [MarsMetadata](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/MarsMetadata.html) and [Molecule](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/Molecule.html) references for detailed inspection of individual records. Once a suitable collection of widgets has been setup, simply reloading after selecting different molecule and metadata records will update all charts in reference to the current record.

Each type of chart widgets expect a different set of outputs. Example scripts are provided for all chart types on creation to illustrate the outputs required and how they can be used. The entire framework relies on the [script parameter](https://imagej.net/Script_Parameters) harvesting mechanism provided with ImageJ.

There are four predefined scriptable widgets: a Category Chart, Histogram,
XY chart, and Bubble chart. Click on the <> tab to change the script, click on the book icon to show the script log. To show examples of these plot types load the widgets and press the refresh button.
To learn more about customising these widgets please go to the [How to use Scriptable Widgets](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) page.


<img align='center' src='{{site.baseurl}}/docs/img/Rover/img6.png' width='800' />
