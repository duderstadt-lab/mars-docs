---
layout: python
title: Common python functions and patterns
permalink: /tutorials/python/common-python-functions/index.html
---

Here you can find an ever growing list of useful python functions and scripting patterns. We will constantly update the page.

##### Useful python packages

```python
# Accessing ImageJ/Fiji
import imagej
ij = imagej.init('/Applications/Fiji.app')

# Python packages
import jpype
import jpype.imports

# Transformation of java class objects into python objects
import scyjava as sc

# Scientific computing
import numpy as np

# Manipulation and analysis of datasets/dataframes
import pandas as pd

# Data visualization
import matplotlib.pyplot as plt
import seaborn as sns

```

##### Access an archives


```python
# Single Molecule Archive
yamaFile = File('/Users/your_archive.yama')
archive = SingleMoleculeArchive(yamaFile)
```


##### Basic commands from Mars

```python
# Number of molecules in Archive
archive.getNumberOfMolecules()

# Access parameter based on UID
archive.get(UID).getParameter('ParameterName')

# Access Metadata tags
archive.metadataHasTag(molecule.getMetadataUID(),'Meta Tag')
```

```python
for UID in archive.getMoleculeUIDs():
    molecule = archive.get(UID)

    # Check if molecule was tagged with 'Tag'
    molecule.hasTag('Tag')

    #Check if molecule has no tag
    molecule.hasNoTag()

    # Check if molecule has parameter named 'ParameterName'
    molecule.hasParameter('ParameterName')

    # Check if molecule has region named 'RegionName'
    molecule.hasRegion('RegionName')

    # Check if molecule has position named 'PositionName'
    molecule.hasPosition('PositionName')
```



##### Create a dataframe from Archive entries (scijava)

```python
# Table for the molecule as a pandas dataframe using the index
tableByIndex = sc._table_to_pandas(archive.get(0).getTable())
tableByIndex

# Table for the molecule as a pandas dataframe using UIDs
tableByUID = sc._table_to_pandas(archive.get('2AEygnwajcvHUBYGGUHcNa').getTable())
tableByUID
```
##### Looping

```python

# For loop to print out all UIDs in Archive
for UID in archive.getMoleculeUIDs():
    molecule = archive.get(UID)
    print(molecule.getUID())

# For loop to calculate value with respect to category
dist_y_active = []
dist_y_unactive = []

for UID in archive.getMoleculeUIDs():
    table_y = _pandas.table_to_pandas(archive.get(UID).getDataTable())["y"]
    if archive.get(UID).hasTag("Active"):
        dist_y_active.append(max(table_y)-min(table_y))
    else:
        dist_y_unactive.append(max(table_y)-min(table_y))

```


##### Lambda and mapping function
```python
# Map of each molecule in the Archive
molecules = map(lambda UID: archive.get(UID), archive.getMoleculeUIDs())

# Get UIDs from each Molecule in Archive
for molecule in molecules:
    print(molecule.getUID())      

# Get UIDs from Molecules tagged 'Active'
for molecule in molecules:
    if molecule.hasTag('Active'):
        print(molecule.getUID())

# Store value of specific parameter ('ParameterName')
listname = list(map(lambda UID: archive.get(UID).getParameter('ParameterName'), archive.getMoleculeUIDs()))
```

##### Access values of a SegmentsTable

```python
# Access values for further analysis
# e. g. from kinetic change point analysis
A_values=[]
A_sigma=[]
B_values=[]
B_sigma=[]

for UID in archive.getMoleculeUIDs():
    if archive.get(UID).hasSegmentsTable("T","y"):
        A_values.append(archive.get(UID).getSegmentsTable("T",'y').getValue("A",0))
        A_sigma.append(archive.get(UID).getSegmentsTable("T",'y').getValue("sigma_A",0))
        B_values.append(archive.get(UID).getSegmentsTable("T",'y').getValue("B",0))
        B_sigma.append(archive.get(UID).getSegmentsTable("T",'y').getValue("sigma_B",0))
```
