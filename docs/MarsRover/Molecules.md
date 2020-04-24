---
layout: docs
title: Molecules
permalink: /docs/MarsRover/Molecules/index.html
---
The Molecules tab has three main sections:


<img align='center' src='{{site.baseurl}}/docs/img/Rover/img8.png' width='600' />


**1. List of molecules in the Archive.**

| :------------- | :------------- |
| Index       | Index of the molecule.       |
| UID       | UID of the molecule.       |
| Tags       | Tags given to the molecule.       |
| metaUID       | UID of the experiment.       |

**2. Data section.**

| :------------- | :------------- |
| DataTable      | Molecule specific position information (x, y, intensity, slice).      |
| Plot | Plotting widget for easy in-GUI analysis of the data |
| Scriptable Widgets      | Scriptable Widgets to visualise data per molecule.       |

**Plotting:**
To make a plot press the 'Add Plot' button. A new window will appear at the bottom of the screen to provide title, axis labels, indicators, x-value, y-value, style, color and stroke.
Add an additional line by pressing the + in the left corner, update the graph with the refresh button in the right corner.

The plotting window itself can be formatted using the tools above the plot. From left to right: track tool (move over the graph to read the corresponding data values), select XY region, select X region, select Y region, move graph, set region, set position, reset zoom, reload graph, save image as .png, and settings (fix X and Y bounds, turn gridlines on/off).


<img align='centre' src='{{site.baseurl}}/docs/img/Rover/img9.png' width='450' />

**3. Notes, parameters, and information per molecule.**

| :------------- | :------------- |
| Information       | UID (copy to clipboard with icon), metaUID, Tags and Notes.      |
| Parameters       | List of parameters.       |
| Regions       | List of assigned regions.      |
| Positions       | List of assigned positions.      |
