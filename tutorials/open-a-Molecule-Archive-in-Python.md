---
layout: tutorials
title: Open a MoleculeArchive in Python
permalink: /tutorials/open-a-Molecule-Archive-in-Python/index.html
---

Here is a wonderful set of instructions on how to work with MoleculeArchives in Python notebooks by Thomas Retzer and Nadia Huisjes. All potential problems encountered when configuring the conda environment are full addressed here.

### Create the Environment
First, an environment has to be created that makes it possible to use ImageJ together with Python. The environment is based on this **[repository](https://github.com/imagej/tutorials)** on GitHub. One has to follow the steps on the page with one tiny addition. The steps will be also written out on this page.  

1. Install **[Anaconda](https://www.anaconda.com/distribution/)**. Alternatively, one can also install **[Miniconda](https://conda.io/miniconda.html)**.

2. Clone the tutorial repository. One can either download the repository or use the following command line (if git is installed).
```terminal
git clone https://github.com/imagej/tutorials.git
```
3. This step is different to the instructions on the website. In order to prevent problems we discovered that the environment has to be adjusted. Open the environment.yml
file with a text editor (e.g. **[Atom](https://atom.io)**). There is a list of dependencies and two dependencies have to be added. Make sure that the dependencies are in line with the others:
```terminal
  - pyjnius=1.2.0
  - seaborn
```
Save the file and continue with the steps described here or on the GitHub page.

4. Open a terminal window and navigate to the repository folder using cd:
```terminal
cd tutorials
```

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

Now everything is ready to go with a yama repository. Before starting the tutorial one can get familiar to ImageJ in the Jupyter Notebook. When the repository was cloned
a folder called "notebooks" was copied. Inside one can find different examples which can be tried out.

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
import re
from matplotlib.colors import ListedColormap
from jnius import autoclass
from matplotlib import pyplot as plt
```

### Define necessary Functions
The functions which are defined next make it possible to load the archive as a pandas dataframe. Pandas is a great package for Python which makes handling of big data sets smooth and easy and offers a lot of possibilities to excess and manipulate your data.
```python
File = autoclass('java.io.File')
SingleMoleculeArchive = autoclass('de.mpg.biochem.mars.molecule.SingleMoleculeArchive')
# Define important functions for loading the archive
def table_to_pandas(table):
    data = []
    headers = []
    for i, column in enumerate(table.toArray()):
        data.append(column.toArray())
        headers.append(table.getColumnHeader(i))
    df = pd.DataFrame(data).T
    df.columns = headers
    return df
def pandas_to_table(df):
    if len(df.dtypes.unique()) > 1:
        TableClass = jnius.autoclass('org.scijava.table.DefaultGenericTable')
    else:
        table_type = df.dtypes.unique()[0]
        if table_type.name.startswith('float'):
            TableClass = jnius.autoclass('org.scijava.table.DefaultFloatTable')
        elif table_type.name.startswith('int'):
            TableClass = jnius.autoclass('org.scijava.table.DefaultIntTable')
        elif table_type.name.startswith('bool'):
            TableClass = jnius.autoclass('org.scijava.table.DefaultBoolTable')
        else:
            msg = "The type '{}' is not supported.".format(table_type.name)
            raise Exception(msg)
    table = TableClass(*df.shape[::-1])
    for c, column_name in enumerate(df.columns):
        table.setColumnHeader(c, column_name)
    for i, (index, row) in enumerate(df.iterrows()):
        for c, value in enumerate(row):
            header = df.columns[c]
            table.set(header, i, scyjava.to_java(value))
    return table
File = autoclass('java.io.File')
```
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
There are two ways of excessing the data entries. One can use the index from the archive. For example: excessing the first entry one can use the index "0" (indexing in Python starts with 0).
```python
#get the DataTable for the molecule at index 0 as a pandas dataframe
tableByIndex = table_to_pandas(archive.get(0).getDataTable())
tableByIndex
```
One can also use the UIDs to excess certain molecules. Just
copy and paste on of them into the following line of code.

```python
#get the DataTable for molecule UID as a pandas dataframe
tableByUID = table_to_pandas(archive.get('22HniKENuPgefz6YHvk1Pm').getDataTable())
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
bins = np.arange(-100, 100, 5)
plt.xlim([min(MSDs)-5, max(MSDs)+5])
plt.hist(MSDs, bins=bins, alpha=0.5)
plt.title('Mean Squared Displacement')
plt.xlabel('MSD')
plt.ylabel('count')

plt.show()
```

This is the end of the tutorial. The environment can be deactivated with the following
command:
```terminal
conda deactivate
```

Or simply close the notebook and the terminal window.


### Great Job!
