---
layout: tutorials
title: "Let's Calculate the Mean Square Displacement"
permalink: /tutorials/calculate-msd/index.html
---

_level: intermediate, duration: 5-10 min_

In this tutorial we will calculate the mean square displacement in the traces
obtained after running the peak tracker. To do so, we need a MoleculeArchive .yama file with the data of the traces of interest. If you do not know yet how to create a MoleculeArchive from your data please go back to the tutorial **[Let's Create a MoleculeArchive](../tutorials/create-a-Molecule-Archive)**. In case you do not want to do that tutorial first go ahead and use tutorial file from our repository. (TestVideoarchive.yama)


### 1. Open the MoleculeArchive (.yama)
First, open the archive using the molecule tool. The MARS GUI should show up.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img1.png' width='450' />

### 2. Run the Mean Square Displacement DriftCalculator
Now go back to the main Fiji environment and select the Mean Square Displacement Calculator from the MARS plugins.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img2.jpg' width='450' />

This should open a window in which you can put in settings.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img3.png' width='200' />

In the case of this experiment we want to use the archive we just opened ('TestVideoarchive.yama') and calculate the MSD for the movement in the direction of the y-axis. You can put in a parameter name you like. Now press OK.

### 3. Show the results in the MARS GUI
After the calculator has run go back to the MARS GUI and open any of the molecule UIDs to show the plot. Next to the plot window you find a window with more options (f.e. molecule tagging). Next to the information icon you find an icon to move to the parameters tab. Click this tab. This should show a list of obtained parameters - at this moment only showing the MSD parameter we just calculated. Since the MSD is a property of a trace one value is calculated per molecule (UID).

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img4.png' width='450' />

When you now go through the different molecule UIDs you can clearly see that, as expected, for active molecules (molecules that we marked with the tag 'Active' in the 'Let's create a MoleculeArchive' tutorial) the MSD value is way higher than for traces representing non-Active molecules.
Now safe the archive again to retain the calculated values.

If you want to analyze these values further f.e. by means of plotting, have a look at the [How to open a MoleculeArchive in python](open-a-Molecule-Archive-in-Python) tutorial.
