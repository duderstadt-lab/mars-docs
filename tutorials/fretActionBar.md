---
layout: actionBar
title: "FRET Power Tools ActionBar"
permalink: /tutorials/fretActionBar/index.html
---

The Fiji [ActionBar plugin](https://figshare.com/articles/dataset/Custom_toolbars_and_mini_applications_with_Action_Bar/3397603) offers many options to build custom button collections for easy access to commands and scripts. These can be used to speed up Mars workflows. In this tutorial, we will install a custom ActionBar for the [example FRET workflows](../../examples/) to illustrate how custom ActionBar button panels can help to streamline Mars workflows and reduce mistakes.

### 1. Install the ActionBar plugin
The [ActionBar plugin](https://figshare.com/articles/dataset/Custom_toolbars_and_mini_applications_with_Action_Bar/3397603) plugin has to be installed to take advantage of custom button collections. Download the jar file found through the link and copy it into the jars folder of Fiji or activate the IBMP-CNRS Fiji update site. The ActionBar script we will install was [tested with version 2.15](https://forum.image.sc/t/actionbar-switches-look-and-feel-to-metal/67838).

### 2. Install the FRET Power Tools button layout and scripts
Copy the [FRET_Power_Tools.ijm and FRET_Power_Tools.txt](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/plugins/ActionBar) files into your local Fiji plugins folder into the ActionBar subfolder. The macro file will allow you to launch the custom button window from the menu and the text file contains the layout as well as the command and script names.

Next, create a FRET subfolder in the scripts folder of your local Fiji and copy all of the [example FRET workflow scripts](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Example_workflows/FRET/scripts) into that subfolder. This should create a new FRET menu with all the scripts as menu items and allow the buttons to activate the scripts.

The easiest way to download all the required files and easily copy them to the correct folders in your local Fiji, is to first clone the [mars-tutorials](https://github.com/duderstadt-lab/mars-tutorials) repository to a local location using `git clone https://github.com/duderstadt-lab/mars-tutorials.git` command.

### 3. Have fun using your new FRET Power Tools button window
To open the FRET Power Tools button window go under Plugins>ActionBar>FRET Power Tools. This should open the multicolor button window shown on the left below.

<div style="text-align: center">
<img align='center' src='{{site.baseurl}}/tutorials/img/ActionBar_FRET_Power_Tools_Menu.png' width='450'>
<img align='center' src='{{site.baseurl}}/tutorials/img/ActionBar_FRET_Power_Tools.png' width='350'></div>

You will notice in addition to all the FRET scripts we installed, there are also several Mars commands added at the top that are frequently used in the [example FRET workflows](../../examples/). If you have trouble getting this tutorial to work get in touch by making a post on the [scientific image community forum](https://forum.image.sc/tag/mars) with the mars tag.
