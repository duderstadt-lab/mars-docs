---
layout: marsto
title: Open a Molecule Archive in Python
permalink: /tutorials/marsto/open-a-Molecule-Archive-in-Python/index.html
---

_level: intermediate, duration: 20 min (including one-time setup)_

In this tutorial you can find a set of instructions on how to work with Molecule Archives in Python by using Anaconda's Jupyter. To do so, one first needs to create a special 'Environment'. This environment only has to be build the first time. When inspecting Molecule Archives later on the environment can be selected and the user can proceed immediately to the 'Molecule Archives in Python' section of this tutorial.

### Create the Environment
First, an environment has to be created that makes it possible to use ImageJ together with Python. The environment is based on this **[repository](https://github.com/imagej/pyimagej)** on GitHub (it is basically a storage place). It contains all the necessary packages needed to build a bridge between java and python and also makes sure that the correct version of the package is used. The next steps will explain how to create the environment step by step.

The next part will use basic commands from Git. To get familiar with **[Git](https://git-scm.com)** and its function we recommend the **[Git-it workshop](https://github.com/jlord/git-it-electron/releases)**. The workshop consists of different challenges that explain how to use Git. So if you want to understand the commands in more detail you can work through the challenges but this is not needed for working through the tutorial.

1. Install **[Anaconda](https://www.anaconda.com/distribution/)** (Python 3.7). Alternatively, one can also install **[Miniconda](https://conda.io/miniconda.html)**. If Anaconda/Miniconda is already installed on the computer the download is not needed. Anaconda/Miniconda is used to install a basic Python distribution.

2. Open the terminal and paste the following line. This will automatically download the environment and it will be called conda pyimageMars:
```terminal
conda create -n pyimagejMars -c conda-forge pyimagej openjdk=8
```
3. The environment has everything you need to work with MARS and python. But for convenience we will add two packages for python. The first one helps to create better looking plots. It is called seaborn. For that activate the environment first with the following line:
```terminal
conda activate pyimagejMars
```
4. Now copy and paste the next line. You will use conda to install the package. Follow the instructions in the terminal:
```terminal
conda install seaborn
```
5. Now we need to install jupyter notebook. Use the following line. Copy and paste it and again follow the instructions in the terminal:
```terminal
conda install jupyter notebook
```
6. Launch Jupyter notebook by typing Jupyter notebook in the terminal. Your browser will be used to display a interface for Jupyter notebook:
```terminal
jupyter notebook
```

Now the environment is created and activated and everything is ready to start working with the data from the Molecule Archive.

For the next section create a new notebook (New -> Python 3). The code below can be copied and pasted in a cell and by using "Shift + Enter" the code inside will be executed (when pressing "Option/Alt + Enter" the cell will be executed and a new cell is created).


### Molecule Archives in Python
To start working with a Molecule Archive in a Jupyter notebook first the Fiji/ImageJ and Python packages should be loaded.

**Load Fiji/ImageJ and Python Packages**  
Before starting one needs to load Fiji and all the necessary Python packages. To run the commands it uses the local copy of Fiji that is stored on your computer. It should be equipped with Mars (**[instructions how to get Mars ready on a local computer](https://duderstadt-lab.github.io/mars-docs/usage/)**).

```python
import imagej
# One needs to add the path to the Fiji application on the computer
ij = imagej.init('/Applications/Fiji.app')

# Python packages
import jpype
import jpype.imports
import scyjava as sc
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from java.io import File
from de.mpg.biochem.mars.molecule import SingleMoleculeArchive
import seaborn as sns

```

One has to specify the path where the Fiji application is located. If it is located in the application folder the path in the code is fine. Otherwise one has to check the location.

**[Scijava](https://github.com/scijava/scyjava)** is needed to transform java class objects into python objects. **[Numpy](https://numpy.org)** is essential for scientific computing. **[Pandas](https://pandas.pydata.org)** is a great tool to manipulate and analysis data sets. **[matplotlib.pyplot](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.html)** and **[seaborn](https://seaborn.pydata.org)** are plotting libraries.  

**Load the Archive to the Jupyter Notebook**  
Next, one loads a specified Molecule Archive in the Jupyter Notebook.

```python
# Here the archive is loaded. Add your file path here
yamaFile = File('/Users/your_archive.yama')
archive = SingleMoleculeArchive(yamaFile)
```
Now one has to add the location of the .yama archive which will be used for analysis. The file path has to be added in the code line. Just
right click on the file and "Get Info" and there the file path will be written out. For testing purposes the 'TestVideo_archive_var.yama' can be downloaded from the [git tutorials repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files/Mars%20to%20%5B%5D).

**Get Started with Simple Operations**  
To start exploring the Molecule Archive in Python the most commonly used methods that can be called on it are presented. For a more extensive overview of all the methods available please visit the [JavaDocs](https://duderstadt-lab.github.io/mars-core/javadoc/).


**Get the Number of Molecules**  
As a start, the number of molecules in the archive is read out. The method which will be used for that is called "getNumberOfMolecules()".
```python
archive.getNumberOfMolecules()
```
The notebook will print out the total number of molecules. So here a function from java is used inside of a python script.

**Get the UUIDs**  
If one wants to know all the UUIDs from all your molecules the following line can be used. The method is called "getMoleculeUIDs()".
```python
for UID in archive.getMoleculeUIDs():
    molecule = archive.get(UID)
    print(molecule.getUID())
```
The notebook will display all the UUIDs.

**Get single Molecule Entries from the Archive**  
There are two ways of accessing the data entries themselves. One can use the index from the archive. For example: accessing the first entry one can use the index "0" (indexing in Python starts with 0).
```python
#get the Table for the molecule at index 0 as a pandas dataframe
tableByIndex = sc._table_to_pandas(archive.get(0).getTable())
tableByIndex
```
One can also use the UUIDs to access certain molecules. Just copy and paste on of them into the following line of code.

```python
#get the Table for molecule UID as a pandas dataframe
tableByUID = sc._table_to_pandas(archive.get('2AEygnwajcvHUBYGGUHcNa').getTable())
tableByUID
```
The build-in function "_table_to_pandas(data)" from **"sc"** (imported from scijava at the top) makes it possible load the table as a pandas data frame. Pandas makes it possible to handle big data sets. The counterpart of the function is "_pandas_to_table(data)" which does the opposite (which is not needed for the rest of the tutorial). The function to get the data table is called "getTable()".

**A More elegant Way to excess Molecules: Mapping**  
Usually not a particular molecule is needed. Most of the times one wants to loop through all of the molecules. To make that possible in an easy fashion the map function can be used.
```python
molecules = map(lambda UID: archive.get(UID), archive.getMoleculeUIDs())
```
Now it is possible to loop through the molecules and print their corresponding UIDs.
```python
for molecule in molecules:
    print(molecule.getUID())      
```
Mapping makes it possible to access the tags of the molecule. This for-loop will print all the molecules tagged with "Active".

```python
molecules = map(lambda UID: archive.get(UID), archive.getMoleculeUIDs())
for molecule in molecules:
    if molecule.hasTag('Active'):
        print(molecule.getUID())
```
Side note: The map function has to be in the same cell if it is not transformed into a list(). Otherwise, the code will not work. If the map function is not saved as a list the data is in virtual storage. Otherwise the data is saved as a list() which can cause problems for big data sets. In the next section, the calculated variance values are stored in a list() that is way the line can be in a separate cell.

**Getting Parameters from the Data**
In this ***[tutorial for calculating variance](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-var/)*** of the data along the y axis was calculated. The following line saves the variance values in a list which makes it possible to access the parameters. The parameter was called "var" in the archive and has to be specified if this parameter has to be called with "getParameter()".
```python
VARs = list(map(lambda UID: archive.get(UID).getParameter('var'), archive.getMoleculeUIDs()))
```
Finally, one can plot the distribution of the variance in a histogram using the matplotlib package.
```python
plt.xlim([min(VARs)-5, max(VARs)+5])

numberofpoints = len(VARs)
bins = round(np.sqrt(numberofpoints))

plt.hist(VARs, bins=bins, alpha=0.5)
plt.title('Variance')
plt.xlabel('var')
plt.ylabel('count')

plt.show()
```
The seaborn package from python is really good for statistical plots and is based on the matplotlib package.
When using seaborn the histogram can be plotted with one simple line. Kernel density estimation (kde) is turned off ("False").
Bins specifies the number of bins.
```python
sns.set_style("darkgrid")
sns.distplot(VARs,kde =False, bins =bins)
```

Now the file can be saved under File -> Save as.

This is the end of the tutorial. The environment can be deactivated with the following command in the terminal:
```terminal
conda deactivate
```

Or simply by closing the notebook and the terminal window.

Now you can make a bridge between the archive and python and use the great packages from python to create awesome plots.
This connections offers unlimited possibilities to work with the data created in Mars.

### Restart the environment and reopen a Jupyter notebook

To reopen the Jupyter notebook one needs to activate the environment again.

1. Open a terminal window and navigate to the repository folder where the jupyter notebooks are stored:
```terminal
cd notebooks
```

2. Activate the environment in the terminal:
```terminal
conda activate pyimagejMars
```

3. Select the notebook of interest from the list and open it.
```terminal
jupyter notebook my_notebook.ipynb
```
