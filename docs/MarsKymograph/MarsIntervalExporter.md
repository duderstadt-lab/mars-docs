---
layout: marskymograph
title: Mars Interval Exporter
permalink: /docs/MarsKymograph/MarsIntervalExporter/index.html
---
The `MarsIntervalExporter` class is a powerful utility for extracting specific regions from N5-based image datasets stored in Mars molecule archives. It provides efficient access to arbitrary intervals from large-scale imaging data, handling multi-channel, multi-timepoint datasets with proper coordinate transformations and drift correction.

## Generic Type Parameter

```java
MarsIntervalExporter<T extends NumericType<T> & NativeType<T>>
```

The class is parameterized with type `T` which must be both a `NumericType` and `NativeType`, ensuring compatibility with ImgLib2's type system for efficient numerical operations.

## Constructor

```java
MarsIntervalExporter(Context context, MoleculeArchive<Molecule, MarsMetadata, MoleculeArchiveProperties<Molecule, MarsMetadata>, MoleculeArchiveIndex<Molecule, MarsMetadata>> archive)
```

Creates a new exporter for extracting intervals from the specified molecule archive.

**Parameters:**
- `context`: The SciJava context for dependency injection
- `archive`: The molecule archive containing the image metadata and source information

## Methods

### Molecule Configuration

| Method | Description |
|--------|-------------|
| `setMolecule(Molecule molecule)` | Set the molecule to use for interval extraction. This determines which image metadata and sources will be used.<br>**Parameters:** `molecule` - The molecule object<br>**Returns:** This exporter for method chaining |
| `setMolecule(String UID)` | Set the molecule to use by its UID.<br>**Parameters:** `UID` - The unique identifier of the molecule<br>**Returns:** This exporter for method chaining |

### Time Range Configuration

| Method | Description |
|--------|-------------|
| `setMinT(int minT)` | Set the minimum time point to include in the export.<br>**Parameters:** `minT` - The minimum time point (0-based)<br>**Returns:** This exporter for method chaining |
| `setMaxT(int maxT)` | Set the maximum time point to include in the export.<br>**Parameters:** `maxT` - The maximum time point (0-based)<br>**Returns:** This exporter for method chaining |

### Spatial Configuration

| Method | Description |
|--------|-------------|
| `setInterval(Interval interval)` | Set the spatial interval (region) to extract from the image.<br>**Parameters:** `interval` - The ImgLib2 Interval defining the spatial bounds (min/max X and Y coordinates)<br>**Returns:** This exporter for method chaining |

### Building the Export

| Method | Description |
|--------|-------------|
| `build()` | Extract the specified interval and time range from the image sources.<br>**Returns:** An `ImgPlus<T>` containing the extracted image data with proper axis information, or null if extraction fails |

## Example Usage

```java
// Create the exporter
MarsIntervalExporter<UnsignedShortType> exporter = 
    new MarsIntervalExporter<>(context, moleculeArchive);

// Define the spatial region to extract
Interval interval = Intervals.createMinMax(100, 100, 200, 200); // Extract 101x101 pixel region

// Configure and build
ImgPlus<UnsignedShortType> extractedImage = exporter
    .setMolecule("moleculeUID")
    .setMinT(0)                    // Start from frame 0
    .setMaxT(49)                   // End at frame 49
    .setInterval(interval)         // Extract the specified region
    .build();

// The result is ready for further processing
if (extractedImage != null) {
    // Process the extracted image
    System.out.println("Extracted image dimensions: " + 
        extractedImage.getWidth() + "x" + extractedImage.getHeight() + 
        "x" + extractedImage.dimension(2)); // width x height x time
}
```

## Advanced Usage with Dynamic Intervals

```java
// Extract region around a specific molecule's coordinates
Molecule molecule = archive.get("moleculeUID");
MarsMetadata metadata = archive.getMetadata(molecule.getMetadataUID());

// Calculate bounding box from molecule data
double minX = Double.POSITIVE_INFINITY;
double maxX = Double.NEGATIVE_INFINITY;
double minY = Double.POSITIVE_INFINITY;
double maxY = Double.NEGATIVE_INFINITY;

// Analyze molecule positions across time
for (int t = 0; t < metadata.getImage(0).getSizeT(); t++) {
    if (molecule.hasPosition(t)) {
        double x = molecule.getPosition(t).getX();
        double y = molecule.getPosition(t).getY();
        minX = Math.min(minX, x);
        maxX = Math.max(maxX, x);
        minY = Math.min(minY, y);
        maxY = Math.max(maxY, y);
    }
}

// Add borders and create interval
int border = 50;
Interval dynamicInterval = Intervals.createMinMax(
    (long)(minX - border), (long)(minY - border),
    (long)(maxX + border), (long)(maxY + border)
);

// Extract the dynamic region
ImgPlus<?> result = exporter
    .setMolecule(molecule)
    .setInterval(dynamicInterval)
    .build();
```

## Implementation Details

### Source Management

The `MarsIntervalExporter` implements efficient caching mechanisms for image sources:

- **Source Caching**: Sources are cached per metadata UID to avoid redundant loading
- **N5 Reader Caching**: N5 readers are cached per file path for optimal performance
- **Dimension Caching**: Image dimensions are cached to avoid repeated queries

### Coordinate Transformations

The exporter handles complex coordinate transformations:

- **Affine Transformations**: Applies source-specific affine transformations
- **Drift Correction**: Automatically applies drift correction when enabled in the source
- **Multi-resolution Support**: Works with multi-resolution N5 datasets

### Multi-channel Support

For multi-channel datasets:

- **Channel Separation**: Automatically handles channel dimension slicing
- **Type Consistency**: Ensures all sources have compatible pixel types
- **Proper Axis Ordering**: Maintains correct axis order (X, Y, Time, Channel)

### Memory Efficiency

The implementation is designed for large datasets:

- **Lazy Loading**: Sources are loaded only when needed
- **Interval-based Access**: Only the requested spatial region is loaded into memory
- **Streaming**: Uses ImgLib2's streaming capabilities for efficient processing

### Error Handling

The exporter includes robust error handling:

- Returns `null` if sources cannot be loaded or are incompatible
- Logs warnings for type mismatches between sources
- Handles missing time points gracefully
- Validates interval bounds against source dimensions

## Integration with Other Components

The `MarsIntervalExporter` is commonly used as a foundation for other builders:

- **Region Builders**: Used by `DnaArchiveMoleculeRegionBuilder`, `ObjectArchiveRegionBuilder`, and `TransverseFlowArchiveMoleculeRegionBuilder`
- **Kymograph Builders**: Provides source data for kymograph generation
- **Montage Builders**: Supplies image data for montage creation

## Performance Considerations

For optimal performance:

1. **Reuse Exporters**: Create one exporter instance per archive and reuse it
2. **Reasonable Intervals**: Keep spatial intervals reasonably sized to avoid memory issues
3. **Time Range Limits**: Limit time ranges for large datasets
4. **Thread Safety**: The class uses static caching, so be aware of concurrent access patterns

## Supported Data Formats

The exporter specifically works with:

- **N5 Format**: Optimized for N5-based image storage
- **BDV Sources**: Compatible with BigDataViewer source definitions
- **Mars Archives**: Designed for Mars molecule archive structure
- **Multi-dimensional Data**: Supports XY, XYT, XYCT, and XYZCT datasets

This class provides the foundational capability for extracting arbitrary regions from large-scale imaging datasets efficiently, making it possible to work with specific areas of interest without loading entire datasets into memory.
