---
layout: marskymograph
title: Transverse Flow Archive Region
permalink: /docs/MarsKymograph/TransverseFlowArchiveMoleculeRegionBuilder/index.html
---

The `TransverseFlowArchiveMoleculeRegionBuilder` class extracts regions from transverse flow molecules stored in a `TransverseFlowArchive`. It provides a way to extract a bounded region around a transverse flow molecule by analyzing replication fork shapes across all time points for further processing or visualization.

## Constructor

```java
TransverseFlowArchiveMoleculeRegionBuilder(Context context, TransverseFlowArchive transverseFlowArchive)
```

Creates a new builder for extracting transverse flow molecule regions.

**Parameters:**
- `context`: The SciJava context
- `transverseFlowArchive`: The archive containing the transverse flow molecules

## Methods

### Setting the Molecule

| Method | Description |
|--------|-------------|
| `setMolecule(TransverseFlowMolecule transverseFlowMolecule)` | Set the molecule to use for region extraction.<br>**Returns:** This builder for method chaining |
| `setMolecule(String UID)` | Set the molecule to use by its UID.<br>**Parameters:** `UID` - The unique identifier of the molecule<br>**Returns:** This builder for method chaining |

### Setting Border Size

| Method | Description |
|--------|-------------|
| `setBorderWidth(int width)` | Set the border width around the molecule in pixels.<br>**Parameters:** `width` - Border width in pixels<br>**Returns:** This builder for method chaining |
| `setBorderHeight(int height)` | Set the border height around the molecule in pixels.<br>**Parameters:** `height` - Border height in pixels<br>**Returns:** This builder for method chaining |

### Building the Region

| Method | Description |
|--------|-------------|
| `build()` | Extracts the region around the transverse flow molecule.<br>The region is defined by analyzing all replication fork shapes across time points to find the bounding box that encompasses all parental, leading, and lagging strand coordinates, with added borders.<br>**Returns:** An `ImgPlus<?>` containing the extracted region |
| `getRegionImage()` | Get the built image (same as returned by `build()`).<br>**Returns:** The `ImgPlus<?>` containing the extracted region |

## Example Usage

```java
// Create the builder
TransverseFlowArchiveMoleculeRegionBuilder regionBuilder = 
    new TransverseFlowArchiveMoleculeRegionBuilder(context, transverseFlowArchive);

// Configure and build
ImgPlus<?> imgPlus = regionBuilder
    .setMolecule("moleculeUID")
    .setBorderWidth(10)
    .setBorderHeight(10)
    .build();
```

## Implementation Details

The class uses the `MarsIntervalExporter` to export an interval from the archive that contains the transverse flow molecule. The interval is computed by:

1. Iterating through all time points where the molecule has shape data
2. Examining all coordinates from the replication fork shapes (parental, leading, and lagging strands)
3. Finding the minimum and maximum X and Y coordinates across all shapes and time points
4. Adding the specified borders to create the final extraction interval

This ensures that the extracted region encompasses the entire molecule trajectory across time while maintaining a consistent field of view.
