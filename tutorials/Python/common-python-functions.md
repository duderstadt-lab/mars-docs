---
layout: python
title: Common python functions and patterns
permalink: /tutorials/python/common-python-functions/index.html
---

_level: intermediate_

Here you can find an ever growing list of useful python functions and
scripting patterns for working with Molecule Archives via
**[marspylib](https://github.com/duderstadt-lab/marspylib)**, a pure-Python
library — no Fiji or Java required. If you haven't already, start with
[Open and Create a Molecule Archive in Python](https://duderstadt-lab.github.io/mars-docs/tutorials/python/open-a-Molecule-Archive-in-Python/)
first. We will constantly update this page.

Every method used below, and its Java/Groovy equivalent if you're coming
from Fiji scripting, is listed in marspylib's
[README method-mapping table](https://github.com/duderstadt-lab/marspylib#coming-from-javagroovyfiji-method-name-mapping).
The full API reference, with every class, method, and field, is at
[marspylib.readthedocs.io](https://marspylib.readthedocs.io/). For the
underlying Java data model itself, see the
[Java docs](https://duderstadt-lab.github.io/mars-core/javadoc/).


##### Useful python packages

```python
# Reading and writing .yama archives -- pure Python, no Fiji needed
import marspylib.yama as yama

# Scientific computing
import numpy as np

# Manipulation and analysis of datasets/dataframes
import pandas as pd

# Data visualization
import matplotlib.pyplot as plt
```


##### Opening archives

`yama.open()` works for every archive type (`SingleMoleculeArchive`,
`DnaMoleculeArchive`, `DefaultMoleculeArchive`, `ObjectArchive`,
`TransverseFlowArchive`) and for both single-file `.yama` archives and
`.yama.store` "virtual" archives (see
[Large archives](#large-archives-yamastore) below) — there's no need to
pick a different function or class depending on what kind of archive you
have. The archive's actual type is available as `archive.properties.archive_type`,
and each molecule you get back is already the matching subclass
(`SingleMolecule`, `DnaMolecule`, etc.) with whatever extra fields that type
carries.

```python
archive = yama.open('/path/to/your_archive.yama')
archive.properties.archive_type
# 'de.mpg.biochem.mars.molecule.SingleMoleculeArchive'
```


##### Basic commands

```python
# Number of molecules in the archive
len(archive)

# Access a molecule by UID
molecule = archive['2AEygnwajcvHUBYGGUHcNa']

# Check whether a UID is present
'2AEygnwajcvHUBYGGUHcNa' in archive

# Access a parameter on a molecule
molecule.parameters['ParameterName']

# Check a metadata tag
archive.metadata_has_tag(molecule.metadata_uid, 'Meta Tag')
```


##### Tags, parameters, regions, and positions

```python
for molecule in archive:
    # Check if molecule was tagged with 'Tag'
    molecule.has_tag('Tag')

    # Check if molecule has no tags at all
    not molecule.tags

    # Check if molecule has a parameter named 'ParameterName'
    'ParameterName' in molecule.parameters

    # Check if molecule has a region named 'RegionName'
    molecule.has_region('RegionName')

    # Check if molecule has a position named 'PositionName'
    molecule.has_position('PositionName')
```

Adding a tag or setting a parameter is just as direct:
```python
molecule.add_tag('reviewed')
molecule.parameters['dwell'] = 5.5
```


##### Working with the data table

Every molecule's trace table is already a
**[pandas](https://pandas.pydata.org)** `DataFrame` — there's no conversion
step, unlike when working through a Java bridge.

```python
# The table for a molecule
molecule.table

# A single column
molecule.table['Y']
```


##### Looping and building distributions by tag

A common pattern: split a value across two groups depending on a tag, e.g.
grouping the range of the `Y` column by whether a molecule is tagged
`'Active'`.
```python
dist_y_active = []
dist_y_inactive = []

for molecule in archive:
    y = molecule.table['Y']
    if molecule.has_tag('Active'):
        dist_y_active.append(y.max() - y.min())
    else:
        dist_y_inactive.append(y.max() - y.min())
```


##### List comprehensions

Iterating directly over `archive` already gives you full molecule objects
(not just UIDs), so there's no need for the `map(lambda uid: archive[uid], ...)`
pattern you may have seen elsewhere — a plain list comprehension does the
same job more simply:
```python
# UIDs of every molecule
uids = [molecule.uid for molecule in archive]

# UIDs of molecules tagged 'Active'
active_uids = [molecule.uid for molecule in archive if molecule.has_tag('Active')]

# Every 'dwell' parameter value
dwell_times = [molecule.parameters['dwell'] for molecule in archive if 'dwell' in molecule.parameters]
```

`archive.molecule_uids()` is still available directly when you need just
the UIDs without touching the records themselves (see
[Large archives](#large-archives-yamastore) below for why that matters).


##### Access values of a Segments Table

Segments tables (e.g. from kinetic change point analysis) are keyed by the
x column, y column, and region name used to generate them:
```python
A_values = []
A_sigma = []

for molecule in archive:
    key = ('T', 'Y', 'RegionName')
    if key in molecule.segment_tables:
        segments_table = molecule.segment_tables[key]
        A_values.append(segments_table['A'][0])
        A_sigma.append(segments_table['sigma_A'][0])
```


##### Editing an archive: put and remove

Mutating a molecule you already hold a reference to (like
`molecule.add_tag(...)` above) is enough for a single-file archive —
just call `archive.save()` afterwards. `put()` is how you add a **new**
molecule (one whose UID wasn't already in the archive), and `remove()`
deletes one:
```python
new_molecule = yama.SingleMolecule(metadata_uid=metadata.uid)
new_molecule.add_tag('accepted')
archive.put(new_molecule)

archive.remove('some-uid-to-delete')

archive.save()
```


##### Large archives (`.yama.store`)

Archives saved from Fiji as a `.yama.store` directory (rather than a single
`.yama` file) open exactly the same way — `yama.open()` detects it
automatically — but records are read lazily, one at a time, instead of all
at once:
```python
archive = yama.open('/path/to/large_experiment.yama.store')  # nothing loaded yet

for molecule in archive:       # each one read from disk as you reach it
    ...
```
For a `.yama.store` archive, checking a tag/channel/image/metadata link via
`archive.molecule_tags(uid)`, `archive.molecule_channel(uid)`,
`archive.molecule_image(uid)`, or `archive.molecule_metadata_uid(uid)`
answers from the archive's index without loading the full molecule record —
useful when filtering a large archive down before doing real work on it:
```python
uids_to_process = [uid for uid in archive.molecule_uids()
                    if archive.molecule_has_tag(uid, 'Active')]

for uid in uids_to_process:
    molecule = archive[uid]   # only now is the full record actually read
    ...
```


##### Opening and saving archives on S3

Archives (single `.yama` files or `.yama.store` directories) can also be
opened and saved directly on S3 or any S3-compatible endpoint, with no
local copy needed:
```python
archive = yama.open_s3(server_address='storage.example.org',
                        bucket='my-bucket', key='path/to/experiment.yama')

archive['some-uid'].add_tag('reviewed')
archive.save()   # saves back to the same S3 location
```
See marspylib's [README](https://github.com/duderstadt-lab/marspylib#using-s3)
for the full details, including credentials (resolved automatically, the
same way the AWS SDK for Java does) and the combined-URL form.
