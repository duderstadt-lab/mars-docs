# DnaArchiveMoleculeRegionBuilder

## Overview

The `DnaArchiveMoleculeRegionBuilder` class extracts regions from DNA molecules stored in a `DnaMoleculeArchive`. It provides a flexible way to extract a bounded region around a DNA molecule for further processing or visualization.

## Constructor

```java
DnaArchiveMoleculeRegionBuilder(Context context, DnaMoleculeArchive dnaMoleculeArchive)
```

Creates a new builder for extracting DNA molecule regions.

**Parameters:**
- `context`: The SciJava context
- `dnaMoleculeArchive`: The archive containing the DNA molecules

## Methods

### Setting the Molecule

| Method | Description |
|--------|-------------|
| `setMolecule(DnaMolecule dnaMolecule)` | Set the molecule to use for region extraction.<br>**Returns:** This builder for method chaining |
| `setMolecule(String UID)` | Set the molecule to use by its UID.<br>**Parameters:** `UID` - The unique identifier of the molecule<br>**Returns:** This builder for method chaining |

### Setting Border Size

| Method | Description |
|--------|-------------|
| `setBorderWidth(int width)` | Set the border width around the DNA molecule in pixels.<br>**Parameters:** `width` - Border width in pixels<br>**Returns:** This builder for method chaining |
| `setBorderHeight(int height)` | Set the border height around the DNA molecule in pixels.<br>**Parameters:** `height` - Border height in pixels<br>**Returns:** This builder for method chaining |

### Building the Region

| Method | Description |
|--------|-------------|
| `build()` | Extracts the region around the DNA molecule.<br>The region is defined by the `Dna_Top_X1`, `Dna_Top_Y1`, `Dna_Bottom_X2`, and `Dna_Bottom_Y2` parameters of the DNA molecule, with added borders.<br>**Returns:** An `ImgPlus<?>` containing the extracted region |
| `getMontage()` | Get the built image (same as returned by `build()`).<br>**Returns:** The `ImgPlus<?>` containing the extracted region |

## Example Usage

```java
// Create the builder
DnaArchiveMoleculeRegionBuilder regionBuilder = 
    new DnaArchiveMoleculeRegionBuilder(context, archive);

// Configure and build
ImgPlus<?> imgPlus = regionBuilder
    .setMolecule("moleculeUID")
    .setBorderWidth(10)
    .setBorderHeight(10)
    .build();
```

## Implementation Details

The class uses the `MarsIntervalExporter` to export an interval from the archive that contains the DNA molecule. The interval is computed based on the DNA molecule's top and bottom coordinates with added borders.
