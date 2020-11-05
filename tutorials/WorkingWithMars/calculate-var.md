---
layout: workingwithmars
title: "Let's Calculate the Variance - Tutorial"
permalink: /tutorials/workingwithmars/calculate-var/index.html
---

_level: intermediate, duration: 5-10 min_

This tutorial focuses on the calculation of the variance (var) in the traces obtained after running the "Peak Tracker" tool. To do so, a Molecule Archive .yama file with the data of the traces of interest is needed. To create such an archive, look at the tutorial [Let's Make a Molecule Archive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/). Alternatively, one can also use the tutorial file from the repository [Testvideo_archive.yama](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files/Working%20with%20Mars).

### 1. Open the Molecule Archive (.yama)
First, open the archive using the "Molecule" tool. The **Mars Rover** GUI should show up.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/Tvar/img1.png' width='450'/></div>

### 2. Run the Variance Calculator
Now go back to the main Fiji environment and select the "Variance Calculator" from the **Mars** plugins.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/Tvar/img2.png' width='450'/></div>

The following window will show up. Provide the settings as shown.

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/Tvar/img3.png' width='450'/></div>

In the case of this example data set, the variance is calculated for the movement in the direction of the y-axis. Provide a parameter name and press OK.
For more information about the experimental background of this example dataset go to the [Let's Make a Molecule Archive Tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/).



### 3. Show the Results in Mars Rover
After the calculator has run go back to **Rover** and open any of the molecule UUIDs to show the plot. Next to the plot window a window with more options is found (f.e. for molecule tagging). Click on the parameters tab (next to the information icon on the top). This should show a list of obtained parameters - at this moment only showing the var parameter just calculated. Since the variance is a property of a trace one value is calculated per molecule (UUID).

<div style="text-align: center"><img  src='{{site.baseurl}}/tutorials/img/Tvar/img4.png' width='650'/></div>

When exploring the molecules two different populations are found: the population tagged "Active" with a high variance & the untagged population with a low variance. This is to be expected since active polymerase molecules are expected to travel a longer distance on the DNA than the molecules that are not active. In section 2 of the [How to use Scriptable Widgets tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) an explanation can be found to visualise these two populations in a histogram on the Rover dashboard.  

Now save the archive again to retain the calculated values.


To further analyse the variance values f.e. by means of plotting, have a look at the [How to open a Molecule Archive in python](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/) and the [How to use Scriptable Widgets](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) tutorials.
