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

The predefined widgets give a first glance at the data. The
information widget shows the name of the archive, the archive type, the number of molecules in the archive, the number of metadata files and which type of memory is used for storage ('Normal' or 'Virtual Mode'). The tag widget shows a bar plot of the number of molecules that are categorised by a certain tag. Update the widgets after a change in the archive by pressing the refresh button.

<img align='center' src='{{site.baseurl}}/docs/img/Rover/img4.png' width='450' />


#### Scriptable widgets
<img align='center' src='{{site.baseurl}}/docs/img/Rover/img3.png' width='180' />

The scriptable widgets are available on **Rover** Dashboard and also in the 'Experiments' and 'Molecules' tabs. They allow for a rapid evaluation of the archive, metadata, and molecule features. All scriptable widgets ('Category Chart', 'Histogram', 'XY Chart', 'Bubble Chart', and 'Beaker') leverage the scripting capabilities of ImageJ to provide limitless customisability. Scripts can be written in Python or Groovy.

Extended [documentation](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/ScriptableWidgets/) on the scripable widgets is provided as well as a tutorial with examples on [the scriptable widgets](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/).


<img align='center' src='{{site.baseurl}}/docs/img/Rover/img6.png' width='800' />
