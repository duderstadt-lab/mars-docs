---
layout: tutorials
title: Open a MoleculeArchive in Python
permalink: /tutorials/open-a-Molecule-Archive-in-Python/index.html
---

Here is a wonderful set of instructions on how to work with MoleculeArchives in Python notebooks by Thomas Retzer and Nadia Huisjes. All potential problems encountered when configuring the conda environment are full addressed here.

### Create the Environment
The next part will use basic commands from Git. To get familiar with **[Git](https://git-scm.com)** and its function we recommend the **[Git-it workshop](https://github.com/jlord/git-it-electron/releases)**. The workshop consist of different challenges which explain how to use Git. So if you want to understand the commands in more detail you can work through the challenges.

First, an environment has to be created that makes it possible to use ImageJ together with Python. The environment is based on this **[repository](https://github.com/imagej/tutorials)** on GitHub. One has to follow the steps on the page with one tiny addition. The steps will be also written out on this page.  

1. Install **[Anaconda](https://www.anaconda.com/distribution/)**(Python 3.7). Alternatively, one can also install **[Miniconda](https://conda.io/miniconda.html)**. If Anaconda/Miniconda is already install on the computer the download is not needed. Anaconda/Miniconda is a basic Python distribution.

2. Clone the tutorial repository. One can either download the repository or use the following command line (if git is installed). Cloning means in this context to get a local copy of the repository on a computer.
```terminal
git clone https://github.com/imagej/tutorials.git
```
3. This step is different to the instructions on the website. In order to prevent problems we discovered that the environment has to be adjusted. Open the environment.yml file with a text editor (e.g. **[Atom](https://atom.io)**). The file can be found in the downloaded folder. There is a list of dependencies and two dependencies have to be added. Make sure that the dependencies are in line with the others:
```terminal
  - pyjnius=1.2.0
  - seaborn
```
Save the file and continue with the steps described here or on the GitHub page.

4. Open a terminal window and navigate to the repository folder using cd:
```terminal
cd tutorials
```
For people who are not familiar with the cd command an example is given. Lets say the folder of interest is located inside the folder
called git (The file path would look something like this /Users/your_name/git/tutorials). When a terminal window is open one starts
the working directory is the users name ("your_name"). Now one has to navigate first in the git directory with "cd git". To go one layer deeper
one has to type "cd tutorials". Basically the cd command goes one layer deeper in the file path. With "cd .." one can go one layer out.

5. Create the environment:
```terminal
conda env create -f environment.yml
```

6. Activate the environment in the terminal:
```terminal
conda activate scijava
```

7. Launch Jupyter notebook. Your browser will open the directory:
```terminal
jupyter notebook
```

Now everything is ready to go with a yama repository. Before starting the tutorial one can get familiar to ImageJ in the Jupyter Notebook. When the repository was cloned a folder called "notebooks" was copied. Inside one can find different examples which can be tried out.

For the next section create a new notebook (File -> New Notebook -> Python 3). The code can be copied in a cell and by using "Shift + Enter" the code inside will be executed (when pressing "Option + Enter" the cell will be executed and a new cell is created).

### Load Fiji/ImageJ and the Python Packages
Before starting one needs to load Fiji and all the necessary Python packages.
```python
import imagej
# One needs to add the path to the Fiji application on the computer
ij = imagej.init('/Applications/Fiji.app')
## Will give you the version of ImageJ/Fiji
ij.getVersion()
# Python packages
import scyjava as sc
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
Scijava is needed to transform java class objects into python objects. The **[jnius package](https://github.com/kivy/pyjnius)** also helps to access Java classes. **[Numpy](https://numpy.org)** is essential for scientific computing. **[Pandas](https://pandas.pydata.org)** is a great tool to manipulate and analysis data sets. **[matplotlib.pyplot](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.html)** is a 2D plotting library.  

### Load the archive
```python
# Here the archive is loaded. Add your file path here
yamaFile = File('/Users/your_archive.yama')
archive = SingleMoleculeArchive(yamaFile)
```
### Get started with simple Operations
#### Get the Number of Molecules
Now everything is ready to go. As a start, the number of molecules in the archive is read out.
```python
archive.getNumberOfMolecules()
```
The notebook will print out the total number of molecules.
#### Get the UIDs
If one wants to know all the UIDs from all your molecules the following line can be used.
```python
for UID in archive.getMoleculeUIDs():
    molecule = archive.get(UID)
    print(molecule.getUID())
```
The notebook will display all the UIDs.

#### Get single Molecule Entries from the Archive
There are two ways of excessing the data entries. One can use the index from the archive. For example: excessing the first entry one can use the index "0" (indexing in Python starts with 0). The build-in function "to_python(data)" from scijava (imported as sc at the top) makes it possible load the table as a pandas data frame. Pandas makes it possible to handle big data sets. The counterpart of the function is "to_java(data)" which does the opposite (which is not needed for the rest of the tutorial).
```python
#get the DataTable for the molecule at index 0 as a pandas dataframe
tableByIndex = sc.to_python(archive.get(0).getDataTable())
tableByIndex
```
One can also use the UIDs to excess certain molecules. Just copy and paste on of them into the following line of code.

```python
#get the DataTable for molecule UID as a pandas dataframe
tableByUID = sc.to_python(archive.get('22HniKENuPgefz6YHvk1Pm').getDataTable())
tableByUID
```
#### More elegant Way to excess Molecules: Mapping
Usually not a particular molecule is needed. Most of the times one wants to loop through all of the molecules. To make that possible in a easy fashion the map function can be used.
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
In the ***[tutorial before the mean squared displacement](https://duderstadt-lab.github.io/mars-docs/tutorials/calculate-msd/)*** of the y data was calculated. The following line saves the MSD values in a list which makes it possible to excess the parameters.
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


This is the end of the tutorial. The environment can be deactivated with the following
command in the terminal:
```terminal
conda deactivate
```

Or simply by closing the notebook and the terminal window.

Now you can make a bridge between the archive and python and use the great packages from python to create awesome plots.
This connections offers unlimited possibilities to work with the data created in Mars.

### Great Job!
