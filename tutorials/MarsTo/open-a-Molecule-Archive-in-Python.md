---
layout: marsto
title: Open a MoleculeArchive in Python
permalink: /tutorials/marsto/open-a-Molecule-Archive-in-Python/index.html
---

Here is a wonderful set of instructions on how to work with MoleculeArchives in Python notebooks by Thomas Retzer and Nadia Huisjes. All potential problems encountered when configuring the conda environment are fully addressed here.

### Create the Environment
First, an environment has to be created that makes it possible to use ImageJ together with Python. The environment is based on this **[repository](https://github.com/imagej/tutorials)** on GitHub (it is basically a storage place). It contains all the necessary packages needed to build a bridge between java and python and also makes sure that the correct version of the package is used. The next steps will explain how to create the environment step by step.

The next part will use basic commands from Git. To get familiar with **[Git](https://git-scm.com)** and its function we recommend the **[Git-it workshop](https://github.com/jlord/git-it-electron/releases)**. The workshop consists of different challenges that explain how to use Git. So if you want to understand the commands in more detail you can work through the challenges but this is not needed for working through the tutorial.

1. Install **[Anaconda](https://www.anaconda.com/distribution/)**(Python 3.7). Alternatively, one can also install **[Miniconda](https://conda.io/miniconda.html)**. If Anaconda/Miniconda is already installed on the computer the download is not needed. Anaconda/Miniconda is used to install a basic Python distribution.

2. Download or clone (for those who are familiar with Git) the tutorial repository. To download the repository just go to the website and press download. To clone the repository use the following command.
```terminal
git clone https://github.com/imagej/tutorials.git
```
3. The environment.yml has to be edited. To do so open the environment.yml file with a text editor (e.g. **[Atom](https://atom.io)**). The file can be found in the downloaded folder. There is a list of dependencies and two dependencies have to be added. Make sure that the dependencies are in line with the others:
```terminal
  - pyjnius=1.2.0
  - seaborn
```
Save the file in the same location and with the same name.

4. Open a terminal window and navigate to the repository folder using cd:
```terminal
cd tutorials
```
For people who are not familiar with the cd command an example is given. Lets say the folder of interest is located inside the folder
called my_documents (The file path would look something like this /Users/your_name/my_documents/tutorials). When a terminal window is opened the working directory is the users name ("your_name"). Now one has to navigate first in the my_documents directory with "cd my_documents". To go one layer deeper
one has to type "cd tutorials". Basically the cd command navigates to the specified layer which lays deeper in the file path. With "cd .." one can go one layer out.

5. Create the environment:
```terminal
conda env create -f environment.yml
```
It now uses the modified environment.yml file to create a python environment using the specified packages (like pyjnius version 1.2.0) which are specified inside.

6. Activate the environment in the terminal:
```terminal
conda activate scijava
```

7. Launch Jupyter notebook by typing Jupyter notebook in the terminal. Your browser will be used to display a interface for Jupyter notebook:
```terminal
jupyter notebook
```

Now the environment is created and activated and everything is ready to start working with the data from the MoleculeArchive.

For the next section create a new notebook (New -> Python 3). The code below can be copied and pasted in a cell and by using "Shift + Enter" the code inside will be executed (when pressing "Option/Alt + Enter" the cell will be executed and a new cell is created).

Before continuing, one can get familiar to ImageJ in the Jupyter Notebook. When the repository was cloned a folder called "notebooks" was copied. Inside one can find different examples which can be tried out. But this is not necessary for completing this tutorial.

### Load Fiji/ImageJ and the Python Packages
Before starting one needs to load Fiji and all the necessary Python packages. To run the commands it uses the local copy of Fiji that is stored on your computer. It should be equipped with Mars (**[instructions how to get Mars ready on a local computer](https://duderstadt-lab.github.io/mars-docs/usage/)**).

```python
import imagej
# One needs to add the path to the Fiji application on the computer
ij = imagej.init('/Applications/Fiji.app')
# Python packages
import scyjava as sc
from scyjava.convert import _pandas
import jnius
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from jnius import autoclass
import seaborn as sns

#instantiate required classes to load archives
File = autoclass('java.io.File')
SingleMoleculeArchive = autoclass('de.mpg.biochem.mars.molecule.SingleMoleculeArchive')
```

One has to specify the path where the Fiji application is located. If it is located in the application folder the path in the code is fine. Otherwise one has to
check the location.

**[Scijava](https://github.com/scijava/scyjava)** is needed to transform java class objects into python objects. The **[jnius package](https://github.com/kivy/pyjnius)** also helps to access Java classes. **[Numpy](https://numpy.org)** is essential for scientific computing. **[Pandas](https://pandas.pydata.org)** is a great tool to manipulate and analysis data sets. **[matplotlib.pyplot](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.html)** and **[seaborn](https://seaborn.pydata.org)** are plotting libraries.  

### Load the archive
```python
# Here the archive is loaded. Add your file path here
yamaFile = File('/Users/your_archive.yama')
archive = SingleMoleculeArchive(yamaFile)
```
Now one has to add the location of the yama archive which will be used for analysis. The file path has to be added in the code line. Just
right click on the file and "Get Info" and there the file path will be written out.

### Get started with simple Operations
After loading on the necessary packages for python and loading the file one can finally can work with the archive. The archive is specified as a "SingleMoleculeArchive". An extensive list of methods that can be called can be found **[here](https://duderstadt-lab.github.io/mars-core/javadoc/de/mpg/biochem/mars/molecule/SingleMoleculeArchive.html)**. In the next section the most commonly used ones are presented.

#### Get the Number of Molecules
As a start, the number of molecules in the archive is read out. The method which will be used for that is called "getNumberOfMolecules()".
```python
archive.getNumberOfMolecules()
```
The notebook will print out the total number of molecules. So here a function from java is used inside of a python script.
#### Get the UIDs
If one wants to know all the UIDs from all your molecules the following line can be used. The method is called "getMoleculeUIDs()".
```python
for UID in archive.getMoleculeUIDs():
    molecule = archive.get(UID)
    print(molecule.getUID())
```
The notebook will display all the UIDs.

#### Get single Molecule Entries from the Archive
There are two ways of excessing the data entries. One can use the index from the archive. For example: excessing the first entry one can use the index "0" (indexing in Python starts with 0).
```python
#get the DataTable for the molecule at index 0 as a pandas dataframe
tableByIndex = _pandas.table_to_pandas(archive.get(0).getDataTable())
tableByIndex
```
One can also use the UIDs to excess certain molecules. Just copy and paste on of them into the following line of code.

```python
#get the DataTable for molecule UID as a pandas dataframe
tableByUID = _pandas.table_to_pandas(archive.get('22HniKENuPgefz6YHvk1Pm').getDataTable())
tableByUID
```
The build-in function "table_to_pandas(data)" from **"_pandas"** (imported from scijava.convert at the top) makes it possible load the table as a pandas data frame. Pandas makes it possible to handle big data sets. The counterpart of the function is "pandas_to_table(data)" which does the opposite (which is not needed for the rest of the tutorial). The function to get the data table is called "getDataTable()".
#### More elegant Way to excess Molecules: Mapping
Usually not a particular molecule is needed. Most of the times one wants to loop through all of the molecules. To make that possible in an easy fashion the map function can be used.
```python
molecules = map(lambda UID: archive.get(UID), archive.getMoleculeUIDs())
```
Now it is possible to loop through the molecules and print their corresponding UIDs.
```python
for molecule in molecules:
    print(molecule.getUID())    
```
Mapping makes it possible to excess the tags of the molecule. This for-loop will print all the molecules tagged with "reaction".

```python
molecules = map(lambda UID: archive.get(UID), archive.getMoleculeUIDs())
for molecule in molecules:
    if molecule.hasTag('reaction'):
        print(molecule.getUID())
```
Side note: The map function has to be in the same cell if it is not transformed into a list(). Otherwise, the code will not work. If the map function is not saved as a list the data is in virtual storage. Otherwise the data is saved as a list() which can cause problems for big data sets. In the next section, the MSD values are stored in a list() that is way the line can be in a separate cell.

#### Getting Parameters from the Data
In this ***[tutorial for the mean squared displacement](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-msd/)*** of the data along the y axis was calculated. The following line saves the MSD values in a list which makes it possible to excess the parameters. The parameter was called "msd" in the archive and has to be specified if this parameter has to be called with "getParameter()".
```python
MSDs = list(map(lambda UID: archive.get(UID).getParameter('msd'), archive.getMoleculeUIDs()))
```
Finally, one can plot the distribution of the MSD in a histogram using the matplotlib package.
```python
plt.xlim([min(MSDs)-5, max(MSDs)+5])
plt.hist(MSDs, bins=24, alpha=0.5)
plt.title('Mean Squared Displacement')
plt.xlabel('MSD')
plt.ylabel('count')

plt.show()
```
The seaborn package from python is really good for statistical plots and is based on the matplotlib package.
When using seaborn the histogram can be plotted with one simple line. Kernel density estimation (kde) is turned off ("False").
Bins specifies the number of bins.
```python
sns.distplot(MSDs,kde =False, bins =24)
```

Now the file can be saved under File -> Save as.

This is the end of the tutorial. The environment can be deactivated with the following
command in the terminal:
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
conda activate scijava
```

3. Select the notebook of interest from the list and open it.
```terminal
jupyter notebook my_notebook.ipnb
```

### Great Job!
