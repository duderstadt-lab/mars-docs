---
layout: workingwithmars
title: "How to use Scriptable Widgets"
permalink: /tutorials/workingwithmars/scriptable-widgets/index.html
---

This tutorial focusses on working with the scriptable widgets. It highlights the functions of the 'Category Chart', 'Histogram', 'XY Chart' and 'Bubble Chart' widgets based on example data generated in the [Let's make a MoleculeArchive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/) and [Let's calculate the Mean Square Displacement (MSD)](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-msd/) tutorials. Please complete these tutorials first before continuing with this tutorial to understand the data set and use the main functions of **Mars**. Alternatively, download the 'MSD.yama' MoleculeArchive from the repository.

In the upcoming sections all four scriptable widgets are described with an example. These sections are not depending on each other and can be completed independent of each other.
Please not that the fifth scriptable widget 'Beaker' is not addressed in this tutorial. The 'Beaker' widget gives complete freedom to the user to script anything that is desired - from showing a weather prediction to creating a fully customised chart specific to a data type of interest. For more documentation on the widgets please visit the [documentation section.](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/RoverDashboard/)

<img src='{{site.baseurl}}/tutorials/img/script/img1.png' width='450' />

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
ymin = 0.0
ymax = 60.0

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
#@OUTPUT Double ymin
#@OUTPUT Double ymax

# Set global outputs
xlabel = "MSD-value"
ylabel = "Frequency"
title = "MSD-value"
bins = 10
xmin = 0.0
xmax = 150.0
ymin = 0.0
ymax = 60.0

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

Now click the refresh button again to display the histogram. In the case of this dataset, it is clear that most tracked molecules have a MSD-value below ~15.

### 3. XY Chart - Plot all 'Active' Traces in one Figure
As an example to show the applicability of the XY Chart widget, a plot is made containing all traces marked in the MoleculeArchive as 'Active'. In the archive made in the [Let's calculate the Mean Square Displacement](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-msd/) tutorial there were 10 molecules tagged 'Active'. The traces (y vs slice) of all molecules will be plotted in one figure to inspect whether their overall shape and rate are similar.

<img src='{{site.baseurl}}/tutorials/img/script/img4.png' width='450' />

Open the XY Chart widget and switch to the scripting tab <> and remove the example script. The next step is to write a script that retrieves and prepares the data for plotting. Data preparation is important since not all traces start at the same y-coordinate and slice number. Therefore, their values have to be corrected to show the traces in such a way they all start at y=0 and slice=0 to enable a good comparison between all of them. To do so, the start y-coordinate (minimum of y coordinates) and start slice number (minimum of slice numbers) is subtracted of every y-coordinate and slice number respectively.

Below, a script that retrieves the y-coordinates and slice numbers, does the described corrections and plots the 10 'Active'-tagged traces can be found. Copy this into the scripting tab.
Please note that since all output parameters needs to be explicitly assigned their values also need to be explicitly assigned. In this scripting environment it is impossible to use a script that assigns these values automatically using matrices.
In this example all error values are set to 0 since no error information is available.


```Python
### @ MoleculeArchive archive
#@OUTPUT String xlabel
#@OUTPUT String ylabel
#@OUTPUT String title
#@OUTPUT Double xmin
#@OUTPUT Double xmax
#@OUTPUT Double ymin
#@OUTPUT Double ymax

#@OUTPUT Double[] series1_xvalues
#@OUTPUT Double[] series1_yvalues
#@OUTPUT Double[] series2_xvalues
#@OUTPUT Double[] series2_yvalues
#@OUTPUT Double[] series3_xvalues
#@OUTPUT Double[] series3_yvalues
#@OUTPUT Double[] series4_xvalues
#@OUTPUT Double[] series4_yvalues
#@OUTPUT Double[] series5_xvalues
#@OUTPUT Double[] series5_yvalues
#@OUTPUT Double[] series6_xvalues
#@OUTPUT Double[] series6_yvalues
#@OUTPUT Double[] series7_xvalues
#@OUTPUT Double[] series7_yvalues
#@OUTPUT Double[] series8_xvalues
#@OUTPUT Double[] series8_yvalues
#@OUTPUT Double[] series9_xvalues
#@OUTPUT Double[] series9_yvalues
#@OUTPUT Double[] series0_xvalues
#@OUTPUT Double[] series0_yvalues

#@OUTPUT String series1_fillColor
#@OUTPUT String series1_strokeColor
#@OUTPUT Integer series1_strokeWidth
#@OUTPUT String series2_fillColor
#@OUTPUT String series2_strokeColor
#@OUTPUT Integer series2_strokeWidth
#@OUTPUT String series3_fillColor
#@OUTPUT String series3_strokeColor
#@OUTPUT Integer series3_strokeWidth
#@OUTPUT String series4_fillColor
#@OUTPUT String series4_strokeColor
#@OUTPUT Integer series4_strokeWidth
#@OUTPUT String series5_fillColor
#@OUTPUT String series5_strokeColor
#@OUTPUT Integer series5_strokeWidth
#@OUTPUT String series6_fillColor
#@OUTPUT String series6_strokeColor
#@OUTPUT Integer series6_strokeWidth
#@OUTPUT String series7_fillColor
#@OUTPUT String series7_strokeColor
#@OUTPUT Integer series7_strokeWidth
#@OUTPUT String series8_fillColor
#@OUTPUT String series8_strokeColor
#@OUTPUT Integer series8_strokeWidth
#@OUTPUT String series9_fillColor
#@OUTPUT String series9_strokeColor
#@OUTPUT Integer series9_strokeWidth
#@OUTPUT String series0_fillColor
#@OUTPUT String series0_strokeColor
#@OUTPUT Integer series0_strokeWidth

# Set global outputs
xlabel = "Slice"
ylabel = "Displacement (Y)"
title = "XY Chart"
xmin = 0.0
xmax = 150
ymin = 0.0
ymax = 35

# Make two lists containing the lists of x and y coordinates of the molecules tagged 'Active'
list1_x=[]
list1_y=[]

for UID in archive.getMoleculeUIDs():
    if archive.get(UID).hasTag('Active'):
        list1_x.append(archive.get(UID).getDataTable().getColumnAsDoubles("slice"))
        list1_y.append(archive.get(UID).getDataTable().getColumnAsDoubles("y"))

# Since all variables have to be explicitly defined, always adjust this part based on the amount of tagged molecules.
series1_xvalues = []
series1_yvalues = []
series2_xvalues = []
series2_yvalues = []
series3_xvalues = []
series3_yvalues = []
series4_xvalues = []
series4_yvalues = []
series5_xvalues = []
series5_yvalues = []
series6_xvalues = []
series6_yvalues = []
series7_xvalues = []
series7_yvalues = []
series8_xvalues = []
series8_yvalues = []
series9_xvalues = []
series9_yvalues = []
series0_xvalues = []
series0_yvalues = []

# Define the colors of the stroke and fill and strokewidth
[series1_strokeColor,series2_strokeColor,series3_strokeColor,series4_strokeColor,series5_strokeColor,
 series6_strokeColor,series7_strokeColor,series8_strokeColor,series9_strokeColor,series0_strokeColor]=["black","blue","red","green","yellow","cyan","orange","violet","pink","gray"]

[series1_fillColor,series2_fillColor,series3_fillColor,series4_fillColor,series5_fillColor,series6_fillColor,
series7_fillColor,series8_fillColor,series9_fillColor,series0_fillColor]= [series1_strokeColor,series2_strokeColor,series3_strokeColor,series4_strokeColor,series5_strokeColor,
 series6_strokeColor,series7_strokeColor,series8_strokeColor,series9_strokeColor,series0_strokeColor]

[series1_strokeWidth,series2_strokeWidth,series3_strokeWidth,series4_strokeWidth,series5_strokeWidth,series6_strokeWidth,
series7_strokeWidth,series8_strokeWidth,series9_strokeWidth,series0_strokeWidth]=[1,1,1,1,1,1,1,1,1,1]

# Assign the x and y values for each trace
for i in list1_x[0]:
    series1_xvalues.append(i-min(list1_x[0]))
for i in list1_y[0]:
    series1_yvalues.append(i-min(list1_y[0]))
series1_error = [0]*len(series1_yvalues)

for i in list1_x[1]:
    series2_xvalues.append(i-min(list1_x[1]))
for i in list1_y[1]:
    series2_yvalues.append(i-min(list1_y[1]))
series2_error = [0]*len(series2_yvalues)

for i in list1_x[2]:
    series3_xvalues.append(i-min(list1_x[2]))
for i in list1_y[2]:
    series3_yvalues.append(i-min(list1_y[2]))
series3_error = [0]*len(series3_yvalues)

for i in list1_x[3]:
    series4_xvalues.append(i-min(list1_x[3]))
for i in list1_y[3]:
    series4_yvalues.append(i-min(list1_y[3]))
series4_error = [0]*len(series4_yvalues)

for i in list1_x[4]:
    series5_xvalues.append(i-min(list1_x[4]))
for i in list1_y[4]:
    series5_yvalues.append(i-min(list1_y[4]))
series5_error = [0]*len(series5_yvalues)

for i in list1_x[5]:
    series6_xvalues.append(i-min(list1_x[5]))
for i in list1_y[5]:
    series6_yvalues.append(i-min(list1_y[5]))
series6_error = [0]*len(series6_yvalues)

for i in list1_x[6]:
    series7_xvalues.append(i-min(list1_x[6]))
for i in list1_y[6]:
    series7_yvalues.append(i-min(list1_y[6]))
series7_error = [0]*len(series7_yvalues)

for i in list1_x[7]:
    series8_xvalues.append(i-min(list1_x[7]))
for i in list1_y[7]:
    series8_yvalues.append(i-min(list1_y[7]))
series8_error = [0]*len(series8_yvalues)

for i in list1_x[8]:
    series9_xvalues.append(i-min(list1_x[8]))
for i in list1_y[8]:
    series9_yvalues.append(i-min(list1_y[8]))
series9_error = [0]*len(series9_yvalues)

for i in list1_x[9]:
    series0_xvalues.append(i-min(list1_x[9]))
for i in list1_y[9]:
    series0_yvalues.append(i-min(list1_y[9]))
series0_error = [0]*len(series0_yvalues)

```
Press the refresh button to render the plot. This plot shows that all traces have a similar slope but vary in their tracked length.

<img src='{{site.baseurl}}/tutorials/img/script/img5.png' width='650' />


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
