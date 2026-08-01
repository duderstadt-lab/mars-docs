---
layout: python
title: Open and Create a Molecule Archive in Python
permalink: /tutorials/python/open-a-Molecule-Archive-in-Python/index.html
---

_level: beginner, duration: 10 min_

In this tutorial you will learn how to open, explore, and create Molecule
Archives (`.yama` files) directly in Python — in a Jupyter notebook, a
plain script, or anywhere else Python runs. This uses
**[marspylib](https://github.com/duderstadt-lab/marspylib)**, a pure-Python
library that reads and writes the `.yama` format natively.

**No Fiji, no Java, no special environment is required.** marspylib does
not talk to Fiji at all — it decodes the `.yama` file format directly — so
setup is just installing one package.

### Install marspylib

If you use **[Anaconda](https://www.anaconda.com/distribution/)** or
**[Miniconda](https://conda.io/miniconda.html)**, create an environment with
marspylib and Jupyter in one step:
```terminal
conda create -n mars-python -c conda-forge marspylib notebook
conda activate mars-python
jupyter notebook
```
Your browser will open with the Jupyter interface. Create a new notebook
(New -> Python 3) and you're ready to go.

If you prefer plain `pip`, this works just as well:
```terminal
pip install marspylib notebook
jupyter notebook
```

The code below can be copied into a notebook cell and run with
"Shift + Enter" ("Option/Alt + Enter" runs the cell and creates a new one
below it, which is convenient for working through the rest of this
tutorial).

### Open an Existing Molecule Archive

To start, open a `.yama` file that was saved from Fiji/Mars Rover (or from
another Python session — see [Create a New Molecule Archive](#create-a-new-molecule-archive-from-scratch)
below). For this tutorial you can download the example
`TestVideo_archive_var.yama` from the
[git tutorials repository](https://github.com/duderstadt-lab/mars-tutorials/tree/master/Tutorial_files/Mars%20to%20%5B%5D)
(the same archive used in the
[Let's calculate the Variance](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-var/)
tutorial). Use GitHub's "Download raw file" button on that page rather than
right-click-save, since the file is stored with Git LFS.

```python
import marspylib.yama as yama

archive = yama.open('/path/to/TestVideo_archive_var.yama')  # use your own file path
```

That's it — `archive` now holds every molecule and metadata record from the
file, ready to explore. Note the file path has to point to wherever you
saved the archive on your computer.

### Explore the Archive

**Number of molecules**
```python
len(archive)
```
This also works: `archive.properties.number_of_molecules`.

**Molecule UIDs**
Every molecule has a unique ID (UID). Looping over the archive gives you the
molecule objects directly — no separate lookup step needed:
```python
for molecule in archive:
    print(molecule.uid)
```

**A single molecule**
Molecules are accessed by UID, like a dictionary:
```python
molecule = archive['2AEygnwajcvHUBYGGUHcNa']  # use one of the UIDs printed above
molecule.tags          # e.g. ['Active']
molecule.parameters    # e.g. {'Channel': 0.0, 'var': 0.0136}
```

**The data table**
Every molecule carries a table of traces (e.g. position over time) as a
**[pandas](https://pandas.pydata.org)** `DataFrame` — no conversion step is
needed, it's ready to use immediately:
```python
molecule.table
```

### Get Started with Simple Operations

**Check tags and get a parameter**  
This loop prints the UID of every molecule tagged `'Active'` together with
its `'var'` parameter:
```python
for molecule in archive:
    if molecule.has_tag('Active'):
        print(molecule.uid, molecule.parameters['var'])
```

**Plot a distribution**  
The `'var'` parameter calculated in the
[variance tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/workingwithmars/calculate-var/)
can be collected into a list and plotted directly with
**[matplotlib](https://matplotlib.org)**:
```python
import matplotlib.pyplot as plt
import numpy as np

variances = [molecule.parameters['var'] for molecule in archive]

bins = round(np.sqrt(len(variances)))
plt.hist(variances, bins=bins, alpha=0.5)
plt.title('Variance')
plt.xlabel('var')
plt.ylabel('count')
plt.show()
```

That's the core loop you'll use over and over: iterate over `archive`,
read `.tags` / `.parameters` / `.table`, collect what you need into a list,
and hand it to matplotlib (or pandas, or seaborn, or anything else in the
Python ecosystem).

### Create a New Molecule Archive from Scratch

You don't need Fiji to build a `.yama` file either — you can construct one
entirely in Python. This is a simple example with three molecules, each
holding a small table:
```python
import pandas as pd
import marspylib.yama as yama

properties = yama.Properties(archive_type=yama.ARCHIVE_TYPES['SingleMoleculeArchive'])
archive = yama.Archive(properties, metadata={}, molecules={})

metadata = yama.MarsMetadata(microscope='My Microscope', source_directory='/data/my_experiment')
archive.put_metadata(metadata)

for i in range(3):
    molecule = yama.SingleMolecule(metadata_uid=metadata.uid)
    molecule.add_tag('accepted')
    molecule.parameters['dwell'] = 5.5 + i
    molecule.table = pd.DataFrame({
        'T': [0.0, 1.0, 2.0],
        'Intensity': [10.1 + i, 10.5 + i, 10.9 + i],
    })
    archive.put(molecule)   # molecule.uid was generated automatically

archive.save('my_first_archive.yama')
```

Reopen it to check that everything saved correctly:
```python
archive = yama.open('my_first_archive.yama')
len(archive)   # 3
```

This same archive can be opened later in Fiji/Mars Rover, just like any
other `.yama` file.

### Next Steps

This tutorial deliberately kept things simple. For more patterns — working
with tags, parameters, regions and positions, segment tables, large
`.yama.store` archives, and archives stored on S3 — continue with
[Common python functions and patterns](https://duderstadt-lab.github.io/mars-docs/tutorials/python/common-python-functions/).

If you're coming from Groovy/Java scripting in Fiji and looking for the
Python equivalent of a method you already know, marspylib's
[README](https://github.com/duderstadt-lab/marspylib#coming-from-javagroovyfiji-method-name-mapping)
has a full method-name mapping table, and the full API reference is at
[marspylib.readthedocs.io](https://marspylib.readthedocs.io/).
