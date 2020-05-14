---
layout: marsrover
title: Scriptable Widgets
permalink: /docs/MarsRover/ScriptableWidgets/index.html
---
<img align='center' src='{{site.baseurl}}/docs/img/Rover/img3.png' width='180' />

<img align='center' src='{{site.baseurl}}/docs/img/Rover/img6.png' width='800' />

The scriptable widgets are available on **Rover** Dashboard and also in the 'Experiments' and 'Molecules' tabs. They allow for a rapid evaluation of the archive, metadata, and molecule features. All scriptable widgets leverage the scripting capabilities of ImageJ to provide limitless customisability. Scripts can be written in Python or Groovy. In both cases, chart scripts are provided with a single input that depends on the dashboard.

At the highest level, the global dashboard receives a reference to the entire [MoleculeArchive](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/MoleculeArchive.html) as an input. This provides complete access to all molecules, metadata, and properties, so global relationships can be rapidly evaluated. Dashboards are also provided within the 'Experiments' and 'Molecules' tabs. These receive [MarsMetadata](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/MarsMetadata.html) and [Molecule](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/Molecule.html) references for detailed inspection of individual records. Once a suitable collection of widgets has been setup, simply reloading after selecting different molecule and metadata records will update all charts in reference to the current record.

### General setup of all Widgets
There are five types of widgets available in **Mars**:
* Category Chart
* Histogram
* XY Chart
* Bubble Chart
* Beaker

In all cases, the widget can be opened by clicking the corresponding icon. The script can be altered in the scripting tab (<>) and the chart can be rendered by pressing the refresh icon. The script log can be accessed by pressing the book icon. This holds all information about running the script (f.e. error types). By default, all widgets come with example code that render an example plot when pressing the refresh button. Widget-specific details are discussed in the next section.

##### Scripting
The entire scripting framework relies on the [script parameter](https://imagej.net/Script_Parameters) harvesting mechanism provided with ImageJ. This way of parameter handling ensures that multiple languages can be interpreted (in the case of **Mars**: Python & Groovy) and makes the scripts easily interchangeable between script running environments. An example Python script is discussed to highlight the main script features. More information on this specific example can be found at the histogram example in the ['How to use Scriptable Widgets'](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) tutorial


In general, a script contains the following sections:
* Global parameter declaration section: declare the input and outputs of the script using the [script parameter](https://imagej.net/Script_Parameters) syntax:
  * #@ Type variableName declares an input
  * #@OUTPUT Type variableName declares an output

* Global output settings: global settings like labels, title and axis information. Make sure that the data type provided corresponds with the expected data type in the parameter declaration.
* Series specific parameter declaration: in this script each line in the plot is defined as a series# (where # is replaced by a number). Depending on the number of series shown, more or less parameters have to be declared. In this example only one line is plotted so only series1 parameters have to be declared.
* Series specific output settings: set series specific information such as color and strokewidth.
* Scripting region: script that retrieves the data of interest from the archive (in this case the value of the 'column_msd' parameter for each molecule) and converts it into the list of values (series1_values) that are plotted in the chart.


Note that the parameters always have to be declared first, but the order of the other components of the script is up to the preferences of the user. Variables used for scripting that are not used as a plot output do not need to be declared as parameter in the script (f.e. make a list of parameter values, then calculate the mean of that list and provide it to the output parameter).

<img src='{{site.baseurl}}/docs/img/Rover/img15.png' width='800' />

To learn more about customising the scripts of the widgets please go to the [How to use Scriptable Widgets](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) tutorial.

##### Language details
The scripting environment is based on the environment provided by ImageJ. Language specific features can be found in the [ImageJ documentation](https://imagej.net/Scripting#Supported_languages). **Mars** supports scripting in Python and Groovy.

**Python**

The Python version used within ImageJ, and therefore within **Mars**, is [Jython](https://www.jython.org). Jython forms a seamless interaction between Python and Java and allows the user to mix both languages during development. The Java implementation of Python is limited to the standard library of Python 2. It is not possible to use external python modules (f.e. Numpy, Pandas), however, any Java class in the Fiji installation can be used.

**Groovy**

Groovy is a Java-based language that builds upon the strenghts of Java but has additional features inspired by languages like Python, Ruby and Smalltalk. For more information about scripting in Groovy visit the [Groovy Quick Start guide](http://groovy-lang.org/documentation.html#gettingstarted).


### Widget-specific information

##### Category Chart

- Expected Outputs
- Adding categories

##### Histogram

- Expected Outputs
- Function of Xmin and Xmax
- Adding series

##### XY Chart

- Expected outputs
  - Errors
- Adding series

##### Bubble Chart

- Expected outputs
- Adding series 





- Category Chart
- Histogram
- XY Chart
- Bubble Chart
- Beaker


To implement a fully independently written widget use the 'beaker' scriptable widget. This undefined widget space gives the user unlimited possibilities to design widgets of choice.
