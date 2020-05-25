---
layout: scripting
title: "Let's Learn Groovy Scripting Tutorial"
permalink: /tutorials/scripting/introduction-to-groovy-scripting/index.html
---

This tutorial will introduce the basics of Mars scripting. Scripting gives the opportunity of (partly) automating data analysis. In this first tutorial the key aspects of scripting will be discussed: how to open the Fiji script editor, how to execute a script, access data from the archive from within a script, examples for simple calculations, and coding in single lines: streams. The [advanced scripting tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/advanced-groovy-scripting/) will go into more detail on less straight-forward Groovy scripting in **Mars**. For more information on the Groovy scripting language the reader is referred to the [Groovy documentation](https://groovy-lang.org/learn.html).


#### 1. How to open the script editor in Fiji
To follow along with this tutorial open the 'TestVideo_archive.yama' archive in **Mars** that was created in the [Let's Make a MoleculeArchive](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/create-a-Molecule-Archive/) tutorial. Alternatively, download this archive from the [mars tutorials repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files).

<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img1.png' width='350' />
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img2.png' width='450' />

To open the script editor press File > New > Script in the Fiji menu as displayed in the image. This will open a new window to be used for scripting. Select the language of choice using the Language menu in the Fiji toolbar. In this case, set the language to 'Groovy'. Notice that the script name changes to the .groovy extension after the Groovy language is selected.

<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img3.png' width='450' />

#### 2. How to execute a script
Next, copy the short example script below and paste it into the scripting window. If this is done correctly, a grey 1 and 2 should show up in front of the lines annotating the line numbers. Run the script by pressing the run button. A dialogue window will pop up in which the archive of interest has to be selected. In this case choose 'TestVideo_archive.yama' and press OK. The script will now be run on the archive.

```Groovy
#@ MoleculeArchive archive
archive.lock()
```
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img4.png' width='450' />
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img5.png' width='250' />
<img src='{{site.baseurl}}/tutorials/img/intro-groovy/img6.png' width='450' />

The example script used in this tutorial is a rather illustrative one: it locks the archive (i.e. disables editing functions temporarily) and displays the rotating mars icon to show the user that the archive has been locked. To unlock the archive again replace the script in the script editor by the script below and run again.

```groovy
#@ MoleculeArchive archive
archive.unlock()
```

#### 3. The general outline of a script 
To learn how write useful scripts, first the general contents of a script are discussed. To make the platform as versatile as possible, Fiji (and therefore **Mars**) utilises [script parameter harvesting](https://imagej.net/Script_Parameters). This universal way of denoting input and output parameters allow for an easy interpretation of multiple languages in the software. On top of that, it makes the scripts interpretable in other platforms and script running environments. The syntax uses #@ to denote new parameters and explicitly specifies the parameter type (f.e. string, integer, double, MoleculeArchive) and the name of that parameter.

**Script Parameter Syntax**  
```groovy
#@ Parametertype Name          #define an input parameter
#@ OUTPUT Parametertype Name   #define an output parameter
```


#### 4. Access data from the archive with a script




#### 5. Examples for simple calculations


#### 6. Streams (one-line coding)
