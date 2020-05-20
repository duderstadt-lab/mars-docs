---
layout: workingwithmars
title: "Let's Calculate the Variance - Tutorial"
permalink: /tutorials/workingwithmars/calculate-var/index.html
---

_level: intermediate, duration: 5-10 min_

This tutorial focuses on the calculation of the variance (var) in the traces obtained after running the "Peak Tracker" tool. To do so, a MoleculeArchive .yama file with the data of the traces of interest is needed. To create such an archive, look at the tutorial [Let's Make a MoleculeArchive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/). Alternatively, one can also use the tutorial file from the repository (Testvideoarchive.yama)

### 1. Open the MoleculeArchive (.yama)
First, open the archive using the "Molecule" tool. The **Mars Rover** GUI should show up.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img1.png' width='450' />

### 2. Run the Variance Calculator
Now go back to the main Fiji environment and select the "Mean Square Displacement Calculator" from the **Mars** plugins.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img2.jpg' width='450' />

The following window will show up. Provide the settings as shown.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img3.png' width='200' />

In the case of this example data set, the variance is calculated for the movement in the direction of the y-axis. Provide a parameter name and press OK.
For more information about the experimental background of this example dataset go to the [Let's Make a MoleculeArchive Tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/)



### 3. Show the Results in Mars Rover
After the calculator has run go back to **Rover** and open any of the molecule UIDs to show the plot. Next to the plot window a window with more options is found (f.e. for molecule tagging). Click on the parameters tab (next to the information icon on the top). This should show a list of obtained parameters - at this moment only showing the var parameter just calculated. Since the variance is a property of a trace one value is calculated per molecule (UID).

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img4.png' width='450' />

When exploring the molecules two different populations are found: the population tagged "Active" with a high variance & the untagged population with a low variance. This is to be expected since active polymerase molecules are expected to travel a longer distance on the DNA than the molecules that are not active. In section 4 it is discussed how these two populations can be visualised in a histogram on the **Rover** dashboard.

Now save the archive again to retain the calculated values.


To further analyse the variance values f.e. by means of plotting, have a look at the [How to open a MoleculeArchive in python](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/) tutorial. Also have a look at the [How to use Scriptable Widgets](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) tutorial to see how the variance parameter can be used to understand the data in more depth using the scriptable widgets on **Rover** dashboard.
