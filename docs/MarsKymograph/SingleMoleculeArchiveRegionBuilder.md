---
layout: marskymograph
title: Single Molecule Archive Region
permalink: /docs/MarsKymograph/SingleMoleculeArchiveRegionBuilder/index.html
---

The `SingleMoleculeArchiveRegionBuilder` class extracts the square region around a spot from a `SingleMoleculeArchive`. The result can be passed to the [MontageBuilder](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/MontageBuilder/) to create a montage of the spot over time.

## Constructor

```java
SingleMoleculeArchiveRegionBuilder(Context context, SingleMoleculeArchive singleMoleculeArchive)
```

Creates a new builder for extracting spot regions.

**Parameters:**
- `context`: The SciJava context
- `singleMoleculeArchive`: The archive containing the molecules

## Methods

### Setting the Molecule

| Method | Description |
|--------|-------------|
| `setMolecule(SingleMolecule molecule)` | Set the molecule to use for region extraction.<br>**Returns:** This builder for method chaining |
| `setMolecule(String UID)` | Set the molecule to use by its UID.<br>**Parameters:** `UID` - The unique identifier of the molecule<br>**Returns:** This builder for method chaining |

### Setting the Region

| Method | Description |
|--------|-------------|
| `setRadius(int radius)` | Set the radius of the square region around the spot center. The region is 2 * radius + 1 pixels wide and high. The default is 5.<br>**Returns:** This builder for method chaining |
| `setLocationSource(String xSource, String ySource)` | Set the table columns, or parameters, used for the spot center. The defaults are `x` and `y`.<br>**Returns:** This builder for method chaining |
| `useParameters(boolean useParameters)` | Use molecule parameters instead of the mean of the table columns for the spot center.<br>**Returns:** This builder for method chaining |
| `setMinT(int minT)` | Set the first time point to extract.<br>**Returns:** This builder for method chaining |
| `setMaxT(int maxT)` | Set the last time point to extract.<br>**Returns:** This builder for method chaining |

### Building the Region

| Method | Description |
|--------|-------------|
| `build()` | Extracts the region around the spot. By default the center is the mean of the `x` and `y` columns of the molecule table.<br>**Returns:** An `ImgPlus<?>` containing the extracted region, or null if the molecule or location columns are missing |
| `getRegionImage()` | Get the built image (same as returned by `build()`).<br>**Returns:** The `ImgPlus<?>` containing the extracted region |

## Example Usage

```groovy
#@ Context scijavaContext
#@ MoleculeArchive archive
#@OUTPUT String imgsrc

import de.mpg.biochem.mars.kymograph.SingleMoleculeArchiveRegionBuilder
import de.mpg.biochem.mars.kymograph.MontageBuilder
import de.mpg.biochem.mars.kymograph.ImageFormatter

def UID = "UID"

def region = new SingleMoleculeArchiveRegionBuilder(scijavaContext, archive)
        .setMolecule(UID)
        .setRadius(8)
        .build()

def montage = new MontageBuilder(scijavaContext)
        .setSource(region)
        .setMinT(0)
        .setMaxT(40)
        .setSpacing(2)
        .skipFrames(5)
        .build()

def formatter = new ImageFormatter(scijavaContext, montage)
        .setRescale(3)
        .setTitle(UID)
        .setXAxisLabel("Frame")
        .setXAxisRange(0, 40)
formatter.build()

imgsrc = formatter.getHtmlEncodedImage()
```
