---
layout: scripting
title: "Let's Learn Groovy Scripting Tutorial"
permalink: /tutorials/scripting/introduction-to-groovy-scripting/index.html
---

This tutorial will introduce the basics of Mars scripting. Scripting gives the opportunity of (partly) automating data analysis. In this first tutorial the key aspects of scripting will be discussed: how to open the Fiji script editor, how to execute a script, access data from the archive from within a script, examples for simple calculations, and coding in single lines: streams. The [advanced scripting tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/advanced-groovy-scripting/) will go into more detail on less straight-forward Groovy scripting in **Mars**. For more information on the Groovy scripting language the reader is referred to the [Groovy documentation](https://groovy-lang.org/learn.html).


### 1. How to open the script editor in Fiji
To follow along with this tutorial open the 'TestVideo_archive.yama' archive in **Mars** that was created in the [Let's Make a MoleculeArchive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/) tutorial. Alternatively, download this archive from the [mars tutorials repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files).

<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img1.png' width='350' />
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img2.png' width='450' />

To open the script editor press File > New > Script in the Fiji menu as displayed in the image. This will open a new window to be used for scripting. Select the language of choice using the Language menu in the Fiji toolbar. In this case, set the language to 'Groovy'. Notice that the script name changes to the .groovy extension after the Groovy language is selected.

<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img3.png' width='450' />

### 2. How to execute a script
Next, copy the short example script below and paste it into the scripting window. If this is done correctly, a grey 1 and 2 should show up in front of the lines annotating the line numbers. Run the script by pressing the run button. A dialogue window will pop up in which the archive of interest has to be selected. In this case choose 'TestVideo_archive.yama' and press OK. The script will now be run on the archive.

```Groovy
#@ MoleculeArchive archive
sleep(10000)
```
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img4.png' width='450' />
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img5.png' width='250' />
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img6.png' width='450' />

The example script used in this tutorial is a rather illustrative one: it locks the archive (i.e. disables editing functions temporarily) and displays the rotating mars icon to show the user that the archive has been locked. The archive unlocks automatically as soon as the sleep timer has ended.


### 3. The general outline of a script
To learn how write useful scripts, first the general contents of a script are discussed. To make the platform as versatile as possible, Fiji (and therefore **Mars**) utilises [script parameter harvesting](https://imagej.net/Script_Parameters). This universal way of denoting input and output parameters allow for an easy interpretation of multiple languages in the software. On top of that, it makes the scripts interpretable in other platforms and script running environments. The syntax uses #@ to denote new parameters and explicitly specifies the parameter type (f.e. string, integer, double, MoleculeArchive) and the name of that parameter.

**Script Parameter Syntax**  
```Groovy
#@ Parametertype Name          #define an input parameter
#@ OUTPUT Parametertype Name   #define an output parameter
```

In the example script used before, the parameter 'archive' is defined as an input of parameter type 'MoleculeArchive'. In this way, it is clear to which object the script has to be applied. No output is declared: no output parameters are defined by the script.


### 4. Access data from the archive with a script
All functions that can be called on with accompanying documentation can be found in the [javadocs](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/MoleculeArchive.html). The expected arguments of each function are listed between the brackets of each defined function. Frequently used commands are discussed in this section of the tutorial.

#### 4.1 'get' functions: get a parameter from the MoleculeArchive

**1** Get the number of molecules in the archive  
```Groovy
#@ MoleculeArchive archive
archive.getNumberOfMolecules()
```

**2** Get the list of molecule UIDs  
```Groovy
#@ MoleculeArchive archive
println(archive.getMoleculeUIDs())
```
This prints a comma separated list of the UIDs in the output window in the lower half of the scripting window. For a neater looking table display of the UIDs use a MarsTable.

```Groovy
#@ MoleculeArchive archive
#@OUTPUT MarsTable table

import de.mpg.biochem.mars.table.*
import org.scijava.table.*

table = new MarsTable("UIDs")
GenericColumn col = new GenericColumn ("UID")
archive.getMoleculeUIDs().forEach{UID -> col.add(UID)}

table.add(col)
```

<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img7.png' width='650' />


**3** Get the list of tags of a certain molecule
```Groovy
#@ MoleculeArchive archive
archive.getTagList('UID') #Replace 'UID' by the actual UID of the molecule of interest.
```

**4** Get the value of a parameter. For example the 'var' parameter   calculated for each molecule entry in the MoleculeArchive.
```Groovy
#@ MoleculeArchive archive
archive.get('UID').getParameter('var')
```

**5** Get the data table for a molecule entry
```Groovy
#@ MoleculeArchive archive
archive.get('UID').getDataTable()
```

**6** Get the segment table for a molecule  
```Groovy
#@ MoleculeArchive archive
archive.get('UID').getSegmentsTable("xcolumn_name","ycolumn_name")
```

#### 4.2 'has' functions: functions used to check if an entry has something

**7** Find out if a molecule has a tag
```Groovy
#@ MoleculeArchive archive
archive.moleculeHasTag('UID','tag') #Replace 'UID' by the actual UID of the molecule of interest and 'tag' by the name of the tag.
```
Returns a boolean indicating whether this molecule has this tag (True) or not (False). Provide only the 'UID' as input to find out if a molecule has any tag.

**8** Find out if a molecule has a segments table
```Groovy
#@ MoleculeArchive archive
archive.get('UID').hasSegmentsTable("xcolumn_name","ycolumn_name")
```
Returns a boolean indicating whether this molecule has a segments table (True) or not (False).


#### 4.3 general functions: save, store location

**9** Save the archive  
```Groovy
#@ MoleculeArchive archive
archive.save()
```  
Alternatively, use the saveAs() or saveAsVirtualStore() commands to save the archive with a different name or in virtual storage.

**10** Get the name of the archive  
```Groovy
#@ MoleculeArchive archive
archive.getName())
```

**11** Retrieve the archive storage location  
```Groovy
#@ MoleculeArchive archive
archive.getStoreLocation()
```

#### 4.4 'set' functions: set certain values of the MoleculeArchive

**12** Define a new parameter in the MoleculeArchive  
```Groovy
#@ MoleculeArchive archive
archive.get('UID').setParameter('parameter_name',value)
```

**13** Set the name the archive should be saved to
```Groovy
#@ MoleculeArchive archive
archive.setName('name')
```


### 5. Examples for simple calculations
#### 5.1 Calculating the total travelled distance in the y direction
In this example the total travelled distance of each molecule in the y direction is calculated and assigned to the parameter 'dist_y' declared for all molecule entries. Defining such a parameter might be of use for investigating the heterogeneity in the total distance travelled within the sample and can subsequently be plotted. The following definition is used:

dist_y = max(y) - min(y)

First, the input and output parameters have to be declared: the archive is declared as MoleculeArchive type. Since the parameter is assigned directly to the archive as well it does not need to be declared as a separate output. Besides this, the molecule, table, util and scijava table classes are imported.

Next, a loop is defined in which for each UID the molecule entry is retrieved as well as the DataTable. The ymin and ymax values are calculated by applying the min() and max() functions respectively to the DataTable on the column named "y". The distance is calculated as the difference between ymax and ymin and subsequently assigned to the parameter "dist_y".

```Groovy
#@ MoleculeArchive archive

import de.mpg.biochem.mars.molecule.*
import de.mpg.biochem.mars.table.*
import de.mpg.biochem.mars.util.*
import org.scijava.table.*

archive.getMoleculeUIDs().stream().forEach({UID ->
  Molecule molecule = archive.get(UID)
  MarsTable table = molecule.getDataTable()
  double ymin = table.min("y")
  double ymax = table.max("y")
  double dist = ymax - ymin
  molecule.setParameter("dist_y",dist)
 })

```

An inspection on the archive shows that indeed "dist_y" is added as a parameter to the parameters tab for each molecule.

<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img8.png' width='650' />

To plot the mean distance travelled in the y direction with respect to tag the category chart widget script in the [how to use scriptable widgets](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/scriptable-widgets/) tutorial can be adjusted slightly. Pasting the script into the [category chart scriptable widget](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/ScriptableWidgets/) on [**Rover** dashboard](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/RoverDashboard/) yields the chart displayed below.

```Groovy
#@ MoleculeArchive archive
#@OUTPUT String xlabel
#@OUTPUT String ylabel
#@OUTPUT String color
#@OUTPUT String title
#@OUTPUT String[] xvalues
#@OUTPUT Double[] yvalues
#@OUTPUT Double ymin
#@OUTPUT Double ymax

color = "#add8e6"
title = "Dist_y vs. Category"
xlabel = "Categories"
ylabel = "Mean dist_y value"
ymin = 0.0
ymax = 30.0

xvalues = ['Active','NotActive'] #Name the categories to be displayed
list1 = []
list2 = []

for UID in archive.getMoleculeUIDs():
    if archive.get(UID).hasTag('Active'): #Check if an entry is tagged 'Active'
        list1.append(archive.get(UID).getParameter('dist_y'))
    else:
        list2.append(archive.get(UID).getParameter('dist_y'))
yvalues=[sum(list1)/len(list1),sum(list2)/len(list2)]
```

The chart shows that there is a clear difference in the travelled distance with respect to the y-axis between molecules tagged 'Active' compared to non-tagged molecules.

<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img9.png' width='450' />

The archive generated in this tutorial can also be found in the [tutorial files repository](https://github.com/duderstadt-lab/mars-tutorials) on GitHub.








### 6. Streams (one-line coding) -> move to advanced tutorial?
