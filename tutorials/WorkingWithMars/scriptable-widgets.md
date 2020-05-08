---
layout: workingwithmars
title: "How to use Scriptable Widgets"
permalink: /tutorials/workingwithmars/scriptable-widgets/index.html
---


<img align='center' src='{{site.baseurl}}/tutorials/img/script/img1.png' width='450' />

On this page examples and tutorials on the different scriptable widgets are highlighted. The tutorials are based on other tutorials as listed in the table below. Please go through the respective tutorial first to understand the dataset and the structure of the MoleculeArchive. For more documentation on the widgets please visit the [documentation section.](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/RoverDashboard/)


| |Scriptable Widget    | Example/tutorial to be found at     |
| :------------- | :------------- |:---|
| 1 |Category Chart       | [Let's calculate the Mean Square Displacement (MSD) tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-msd/)      |
| 2 |Histogram       | [Let's calculate the Mean Square Displacement (MSD) tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-msd/)      |
| 3 |XY Chart       |        |
| 4 |Bubble Chart       | [Let's calculate the Mean Square Displacement (MSD) tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-msd/)        |
| 5 |Beaker       |        |


### 1. Category Chart - Plot the Mean MSD Value with Respect to Tag
To gain insight in the relationship between the mean square displacement (MSD) and the assigned tag ('Active' or not) this script first makes two categories: 'Active'-tagged molecules and molecules that are not tagged. Next, the MSD values of all molecules in these categories are collected and the mean per category is calculated. This gives a first glance at the relation between MSD value and tag category. Note that for a more thorough analysis of this relationship the reader is referred to the [Open a MoleculeArchive in Python](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/) tutorial. In Python this plot can be recreated using a very similar script as used for this widget, also plotting standard deviation.

<img align='center' src='{{site.baseurl}}/tutorials/img/script/img2.png' width='450' />

First, open the Category Chart widget in the **Rover** Dashboard toolbar. Switch to the script tab (<>) and replace the example script with the script below. Make sure to insert the correct parameter name (in this example: 'column_MSD'). Press the refresh button to load the plot. Switch back to the plot tab.

```python
#@ MoleculeArchive archive
#@OUTPUT String xlabel
#@OUTPUT String ylabel
#@OUTPUT String color
#@OUTPUT String title
#@OUTPUT String[] xvalues
#@OUTPUT Double[] yvalues
#@OUTPUT Double[] list1
#@OUTPUT Double[] list2
#@OUTPUT Double[] ymin
#@OUTPUT Double[] ymax

color = "#add8e6"
title = "Category Chart"
xlabel = "Categories"
ylabel = "Mean MSD value"

xvalues = ['Active','NotActive']
list1 = []
list2 = []

for UID in archive.getMoleculeUIDs():
    if archive.get(UID).hasTag('Active'):
        list1.append(archive.get(UID).getParameter('column_MSD'))
    else:
        list2.append(archive.get(UID).getParameter('column_MSD'))

yvalues=[sum(list1)/len(list1),sum(list2)/len(list2)]

```

<img align='center' src='{{site.baseurl}}/tutorials/img/script/img3.png' width='450' />



### 2. Histogram - Plot the MSD in the Rover Dashboard
To begin, click on the histogram icon in the widget dashboard toolbar. This will add a histogram widget to the dashboard pane. Next click the script tab <> to reveal the example script generating an example histogram with random number generators instead of MoleculeArchive data. To display this histogram, click the refresh icon in the far right.

<img src='{{site.baseurl}}/tutorials/img/TMSD/img5.png' width='450' />

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

<img src='{{site.baseurl}}/tutorials/img/TMSD/img6.png' width='450' />

Now click the refresh button again to display the histogram.

### 4. Bubble Chart - Plot the MSD vs Track Length in the Rover Dashboard
Next, to plot one feature of a molecule against another feature the bubble plot can be used. In this example the MSD value is plotted against the track length. This could answer the question "Are longer tracks associated with higher MSD values?".
To open the bubble plot widget press the icon in the **Rover** dashboard toolbar. Open the script tab (<>) and adjust the example script to match the correct parameter name (in this example: "column_MSD") and x-axis and y-axis minima and maxima. This script makes an array of x-coordinates containing the length of the traces (length of the data table with the number of slices per track in it), an array of y-coordinates containing the MSD values per molecule, and next to that, also labels the data points with the molecule UID when the mouse is placed on the data point. This feature can be used to find the molecule in the Archive.


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
