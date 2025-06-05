# ObjectArchiveRegionBuilder

## Overview

The `ObjectArchiveRegionBuilder` class extracts regions from objects stored in an `ObjectArchive`. It provides a way to extract a bounded region around a Martian object by analyzing all peak shapes across time points for further processing or visualization.

## Constructor

```java
ObjectArchiveRegionBuilder(Context context, ObjectArchive objectArchive)
```

Creates a new builder for extracting object regions.

**Parameters:**
- `context`: The SciJava context
- `objectArchive`: The archive containing the objects

## Methods

### Setting the Object

| Method | Description |
|--------|-------------|
| `setObject(MartianObject martianObject)` | Set the object to use for region extraction.<br>**Returns:** This builder for method chaining |
| `setObject(String UID)` | Set the object to use by its UID.<br>**Parameters:** `UID` - The unique identifier of the object<br>**Returns:** This builder for method chaining |

### Setting Border Size

| Method | Description |
|--------|-------------|
| `setBorderWidth(int width)` | Set the border width around the object in pixels.<br>**Parameters:** `width` - Border width in pixels<br>**Returns:** This builder for method chaining |
| `setBorderHeight(int height)` | Set the border height around the object in pixels.<br>**Parameters:** `height` - Border height in pixels<br>**Returns:** This builder for method chaining |

### Building the Region

| Method | Description |
|--------|-------------|
| `build()` | Extracts the region around the object.<br>The region is defined by analyzing all peak shapes across time points to find the bounding box that encompasses all shape coordinates, with added borders.<br>**Returns:** An `ImgPlus<?>` containing the extracted region |
| `getImage()` | Get the built image (same as returned by `build()`).<br>**Returns:** The `ImgPlus<?>` containing the extracted region |

## Example Usage

```java
// Create the builder
ObjectArchiveRegionBuilder regionBuilder = 
    new ObjectArchiveRegionBuilder(context, objectArchive);

// Configure and build
ImgPlus<?> imgPlus = regionBuilder
    .setObject("objectUID")
    .setBorderWidth(10)
    .setBorderHeight(10)
    .build();
```

## Implementation Details

The class uses the `MarsIntervalExporter` to export an interval from the archive that contains the object. The interval is computed by:

1. Iterating through all time points where the object has shape data
2. Examining all coordinates from the peak shapes (x and y arrays)
3. Finding the minimum and maximum X and Y coordinates across all shapes and time points
4. Adding the specified borders to create the final extraction interval

This ensures that the extracted region encompasses the entire object trajectory across time while maintaining a consistent field of view suitable for montage creation or further analysis.
