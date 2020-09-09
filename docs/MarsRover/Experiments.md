---
layout: marsrover
title: Metadata
permalink: /docs/MarsRover/Metadata/index.html
---

The Metadata tab has three main sections:


**1. List of Metadata entries in the Archive.**

| :------------- | :------------- |
| Index       | Index of the Metadata entry.       |
| UID       | UID of the Metadata entry.       |
| Tags       | Tags given to the Metadata entry.       |

**2. Data section.**

| :------------- | :------------- |
| OME      | All Metadata records are listed here. Select the image from the plane on the left to show all 'Image' and 'Plane' information in the middle two segments.      |
| Log       | Log of parameters set in image processing and tracking.       |
| Scriptable widgets      | [Scriptable Widgets](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/ScriptableWidgets/) to visualise data per Metadata entry.       |
| Bdv Views       | BigDataViewer parameters to be used for video visualisations. See [tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/bdv/).       |

**3. Notes, parameters, and information valid for all molecules within the metadata entry.**

| :------------- | :------------- |
| Information       | UID (copy to clipboard with icon), Tags and Notes.      |
| Parameters       | List of Metadata record-wide parameters.       |
| Regions       | List of assigned regions (f.e. to denote the change of a parameter at a certain time point in the experiment).       |
| Positions       | List of globally assigned positions.      |

Regions and Positions are globally defined in the Metadata tab and if enabled will show up in the plots made under the 'molecules' tab where relevant.

<img align='center' src='{{site.baseurl}}/docs/img/Rover/img7.png' width='600' />
