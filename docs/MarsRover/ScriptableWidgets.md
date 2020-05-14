---
layout: marsrover
title: Scriptable Widgets
permalink: /docs/MarsRover/ScriptableWidgets/index.html
---
<img align='center' src='{{site.baseurl}}/docs/img/Rover/img3.png' width='180' />

<img align='center' src='{{site.baseurl}}/docs/img/Rover/img6.png' width='800' />

The scriptable widgets are available on **Rover** Dashboard and also in the 'Experiments' and 'Molecules' tabs. They allow for a rapid evaluation of the archive, metadata, and molecule features. All scriptable widgets leverage the scripting capabilities of ImageJ to provide limitless customisability. Scripts can be written in Python or Groovy. In both cases, chart scripts are provided with a single input that depends on the dashboard.

At the highest level, the global dashboard receives a reference to the entire [MoleculeArchive](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/MoleculeArchive.html) as an input. This provides complete access to all molecules, metadata, and properties, so global relationships can be rapidly evaluated. Dashboards are also provided within the 'Experiments' and 'Molecules' tabs. These receive [MarsMetadata](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/MarsMetadata.html) and [Molecule](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/Molecule.html) references for detailed inspection of individual records. Once a suitable collection of widgets has been setup, simply reloading after selecting different molecule and metadata records will update all charts in reference to the current record.




### General setup of all Widgets
Each type of chart widgets expect a different set of outputs. Example scripts are provided for all chart types on creation to illustrate the outputs required and how they can be used. The entire framework relies on the [script parameter](https://imagej.net/Script_Parameters) harvesting mechanism provided with ImageJ.

There are four predefined scriptable widgets: a Category Chart, Histogram,
XY chart, and Bubble chart. Click on the <> tab to change the script, click on the book icon to show the script log. To show examples of these plot types load the widgets and press the refresh button.
To learn more about customising these widgets please go to the [How to use Scriptable Widgets](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) page.


### Widget specific information

- Category Chart
- Histogram
- XY Chart
- Bubble Chart
- Beaker


To implement a fully independently written widget use the 'beaker' scriptable widget. This undefined widget space gives the user unlimited possibilities to design widgets of choice.
