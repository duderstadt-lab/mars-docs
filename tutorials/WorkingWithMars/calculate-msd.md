---
layout: workingwithmars
title: "Let's Calculate the Mean Square Displacement - Tutorial"
permalink: /tutorials/workingwithmars/calculate-msd/index.html
---

_level: intermediate, duration: 5-10 min_

This tutorial focuses on the calculation of the mean square displacement (MSD) in the traces obtained after running the "Peak Tracker" tool. To do so, a MoleculeArchive .yama file with the data of the traces of interest is needed. To create such an archive, look at the tutorial [Let's Make a MoleculeArchive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/). Alternatively, one can also use the tutorial file from the repository (Testvideoarchive.yama)

### 1. Open the MoleculeArchive (.yama)
First, open the archive using the "Molecule" tool. The **Mars Rover** GUI should show up.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img1.png' width='450' />

### 2. Run the Mean Square Displacement DriftCalculator
Now go back to the main Fiji environment and select the "Mean Square Displacement Calculator" from the **Mars** plugins.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img2.jpg' width='450' />

The following window will show up. Provide the settings as shown.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img3.png' width='200' />

In the case of this example data set, the MSD is calculated for the movement in the direction of the y-axis. Provide a parameter name and press OK.
For more information about the experimental background of this example dataset go to the [Let's Make a MoleculeArchive Tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/)



### 3. Show the Results in Mars Rover
After the calculator has run go back to **Rover** and open any of the molecule UIDs to show the plot. Next to the plot window a window with more options is found (f.e. for molecule tagging). Click on the parameters tab (next to the information icon on the top). This should show a list of obtained parameters - at this moment only showing the MSD parameter just calculated. Since the MSD is a property of a trace one value is calculated per molecule (UID).

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img4.png' width='450' />

When exploring the molecules two different populations are found: the population tagged "Active" with a high MSD & the untagged population with a low MSD value. This is to be expected since active polymerase molecules are expected to travel a longer distance on the DNA than the molecules that are not active. In section 4 it is discussed how these two populations can be visualised in a histogram on the **Rover** dashboard.

Now save the archive again to retain the calculated values.

### 4. (Optional) Plot the MSD in a Dashboard Scriptable Widget Histogram
For more background information about scriptable widgets please go to the [scriptable widgets documentation](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/RoverDashboard/).

To begin, click on the histogram icon in the widget dashboard toolbar. This will add a histogram widget to the dashboard pane. Next click the script tab <> to reveal the example script generating an example histogram with random number generators instead of MoleculeArchive data. To display this histogram, click the refresh icon in the far right.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img5.png' width='450' />

In the next step, alter the example code to match the code below. In this way it will display the frequency of calculated MSD values. For convenience, the window dimensions can be changed to a bigger field for easier scripting. Make sure to adapt the script below to match the correct parameter name (in this example: 'column_MSD') and the desired xmin, xmax, ymin, ymax values to display the axes. As a starting point, one can always use the xmin and xmax values of the series1_values array. This is left as an exercise for the reader to complete.


```python
#@ MoleculeArchive archive
#@OUTPUT String xlabel
#@OUTPUT String ylabel
#@OUTPUT String title
#@OUTPUT Integer bins
#@OUTPUT Double xmin
#@OUTPUT Double xmax

# Set global outputs
xlabel = "MSD-value"
ylabel = "Frequency"
title = "MSD-value"
bins = 10
xmin = 0.0
xmax = 150.0
ymin = 0.0
ymax = 40.0

# Series 1 Outputs
#@OUTPUT Double[] series1_values
#@OUTPUT String series1_strokeColor
#@OUTPUT Integer series1_strokeWidth

series1_strokeColor = "black"
series1_strokeWidth = 2

series1_values = []

for molecule in archive.molecules().iterator():
    series1_values.append(molecule.getParameter("column_MSD"))
```

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img6.png' width='450' />

Now click the refresh button again to display the histogram.

### 5. (Optional) Plot the MSD vs Track Length in a Dashboard Scriptable Widget Bubble Plot
Next, to plot one feature of a molecule against another feature the bubble plot can be used. In this example the MSD value is plotted against the track length. This could answer the question "Are longer tracks associated with higher MSD values?".
To open the bubble plot widget press the icon in the **Rover** dashboard toolbar. Open the script tab (<>) and adjust the example script to match the correct parameter name (in this example: "column_MSD") and x-axis and y-axis minima and maxima. This script makes an array of x-coordinates containing the length of the traces (length of the data table with the number of slices per track in it), an array of y-coordinates containing the MSD values per molecule, and next to that, also labels the data points with the molecule UID when the mouse is placed on the data point. This feature can be used to find the molecule in the Archive. Extended background information about scriptable widgets can be found in the [scriptable widgets documentation](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/RoverDashboard/).


<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img7.png' width='450' />

```python
#@ MoleculeArchive archive
#@OUTPUT String xlabel
#@OUTPUT String ylabel
#@OUTPUT String title
#@OUTPUT Double xmin
#@OUTPUT Double xmax
#@OUTPUT Double ymin
#@OUTPUT Double ymax

# Set global outputs
xlabel = "Track length (slices)"
ylabel = "MSD-value"
title = "Bubble chart"

xmin = 0.0
xmax = 160
ymin = 0.0
ymax = 125

# Series 1 Outputs
#@OUTPUT Double[] series1_xvalues
#@OUTPUT Double[] series1_yvalues
#@OUTPUT Double[] series1_size
#@OUTPUT String[] series1_label
#@OUTPUT String[] series1_color
#@OUTPUT String series1_markerColor

series1_markerColor = "lightgreen"

series1_xvalues = []
series1_yvalues = []
series1_size = []
series1_color = []
series1_label = []

for molecule in archive.molecules().iterator():
	series1_xvalues.append(molecule.getDataTable().getRowCount())
	series1_yvalues.append(molecule.getParameter("column_MSD"))
	series1_size.append(4.0)
	series1_color.append("blue")
	series1_label.append(molecule.getUID())
```

Now press the refresh button and display the bubble chart.

<img align='center' src='{{site.baseurl}}/tutorials/img/TMSD/img8.png' width='350' />



To further analyse the MSD values f.e. by means of plotting, have a look at the [How to open a MoleculeArchive in python](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/) tutorial.
