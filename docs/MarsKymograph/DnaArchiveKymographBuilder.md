# DnaArchiveKymographBuilder

## Overview

The `DnaArchiveKymographBuilder` class creates kymographs directly from DNA molecules stored in a `DnaMoleculeArchive`. It provides a streamlined approach to generate kymographs by automatically extracting the DNA region, applying optional filtering and frame reduction, and projecting along the DNA trajectory. This builder combines region extraction, image processing, and kymograph generation into a single, convenient interface.

## Constructor

```java
DnaArchiveKymographBuilder(Context context, DnaMoleculeArchive dnaMoleculeArchive)
```

Creates a new builder for generating kymographs from DNA molecules.

**Parameters:**
- `context`: The SciJava context for dependency injection
- `dnaMoleculeArchive`: The archive containing the DNA molecules and associated image data

## Methods

### Molecule Configuration

| Method | Description |
|--------|-------------|
| `setMolecule(DnaMolecule dnaMolecule)` | Set the DNA molecule to use for kymograph generation.<br>**Parameters:** `dnaMolecule` - The DNA molecule object<br>**Returns:** This builder for method chaining |
| `setMolecule(String UID)` | Set the DNA molecule to use by its UID.<br>**Parameters:** `UID` - The unique identifier of the DNA molecule<br>**Returns:** This builder for method chaining |

### Kymograph Configuration

| Method | Description |
|--------|-------------|
| `setProjectionWidth(int width)` | Set the width of the projection line for the kymograph.<br>**Parameters:** `width` - The projection width in pixels (default: 1)<br>**Returns:** This builder for method chaining |
| `setMinT(int minT)` | Set the minimum time point to include in the kymograph.<br>**Parameters:** `minT` - The minimum time point (0-based)<br>**Returns:** This builder for method chaining |
| `setMaxT(int maxT)` | Set the maximum time point to include in the kymograph.<br>**Parameters:** `maxT` - The maximum time point (0-based)<br>**Returns:** This builder for method chaining |

### Frame Reduction Methods

| Method | Description |
|--------|-------------|
| `skipFrames(int skipFactor)` | Include only every Nth frame in the kymograph.<br>**Parameters:** `skipFactor` - Include every Nth frame (e.g., 5 means include frames 0, 5, 10, etc.)<br>**Returns:** This builder for method chaining |
| `averageFrames(int groupSize)` | Average groups of N frames to create each kymograph frame.<br>**Parameters:** `groupSize` - Number of frames to average together<br>**Returns:** This builder for method chaining |
| `sumFrames(int groupSize)` | Sum groups of N frames to create each kymograph frame.<br>**Parameters:** `groupSize` - Number of frames to sum together<br>**Returns:** This builder for method chaining |
| `useAllFrames()` | Reset frame reduction to use all frames without reduction.<br>**Returns:** This builder for method chaining |

### Frame Processing

| Method | Description |
|--------|-------------|
| `setVerticalReflection(boolean reflect)` | Enable or disable vertical reflection (flipping) of the frames.<br>**Parameters:** `reflect` - True to vertically reflect frames, false for normal orientation<br>**Returns:** This builder for method chaining |
| `medianFilter(int radius)` | Apply median filter to all frames before kymograph generation.<br>**Parameters:** `radius` - The radius of the median filter<br>**Returns:** This builder for method chaining |
| `gaussianFilter(double sigma)` | Apply Gaussian filter to all frames before kymograph generation.<br>**Parameters:** `sigma` - The sigma value for the Gaussian filter<br>**Returns:** This builder for method chaining |
| `tophatFilter(int radius)` | Apply top-hat filter to all frames before kymograph generation.<br>**Parameters:** `radius` - The radius of the top-hat filter<br>**Returns:** This builder for method chaining |
| `threads(int numThreads)` | Set the number of threads to use for filtering operations.<br>**Parameters:** `numThreads` - The number of threads (minimum: 1)<br>**Returns:** This builder for method chaining |
| `interpolation(double factor)` | Enable interpolation to increase resolution before kymograph generation.<br>**Parameters:** `factor` - The factor by which to increase resolution (must be > 0)<br>**Returns:** This builder for method chaining |

### Building and Retrieval

| Method | Description |
|--------|-------------|
| `build()` | Build the kymograph from the DNA molecule.<br>The process includes: extracting the DNA region, applying frame reduction/filtering/interpolation, creating the projection line, and generating the final kymograph.<br>**Returns:** A `Dataset` containing the generated kymograph |
| `getKymograph()` | Get the built kymograph (same as returned by `build()`).<br>**Returns:** The `Dataset` containing the kymograph |

## Enums

### FrameReductionMethod

Defines the available frame reduction strategies:

- `NONE`: Use all frames without reduction
- `SKIP`: Skip frames (include every Nth frame)
- `AVERAGE`: Average groups of frames
- `SUM`: Sum groups of frames

### ImageFilterMethod

Defines the available image filtering options:

- `NONE`: No filtering applied
- `MEDIAN`: Median filter for noise reduction
- `GAUSSIAN`: Gaussian filter for smoothing
- `TOPHAT`: Top-hat filter for background subtraction

## Example Usage

### Basic Kymograph Generation

```java
// Create the builder
DnaArchiveKymographBuilder kymographBuilder = 
    new DnaArchiveKymographBuilder(context, dnaMoleculeArchive);

// Build a simple kymograph
Dataset kymograph = kymographBuilder
    .setMolecule("dnaUID")
    .setProjectionWidth(3)
    .build();
```

### Advanced Kymograph with Processing

```java
// Create a kymograph with filtering and frame reduction
Dataset kymograph = kymographBuilder
    .setMolecule("dnaUID")
    .setProjectionWidth(5)              // Wider projection for better signal
    .setMinT(10)                        // Start from frame 10
    .setMaxT(100)                       // End at frame 100
    .skipFrames(2)                      // Use every 2nd frame
    .gaussianFilter(1.5)                // Apply Gaussian smoothing
    .interpolation(2.0)                 // Double the resolution
    .setVerticalReflection(true)        // Flip vertically
    .threads(4)                         // Use 4 threads for processing
    .build();
```

### Time-lapse Analysis Workflow

```java
// Process multiple DNA molecules
for (String uid : dnaMoleculeArchive.getMoleculeUIDs()) {
    Dataset kymograph = kymographBuilder
        .setMolecule(uid)
        .setProjectionWidth(3)
        .averageFrames(5)               // Average every 5 frames for noise reduction
        .tophatFilter(2)                // Remove background
        .build();
    
    // Save or analyze the kymograph
    IJ.save(kymograph.getImgPlus(), "/path/to/kymographs/" + uid + "_kymograph.tif");
}
```

## Implementation Details

### Automatic Region Extraction

The builder automatically determines the DNA region by:

1. **Reading DNA Parameters**: Extracts DNA coordinates from molecule parameters (`Dna_Top_X1`, `Dna_Top_Y1`, `Dna_Bottom_X2`, `Dna_Bottom_Y2`)
2. **Adding Borders**: Automatically adds 10-pixel borders around the DNA region
3. **Creating Interval**: Constructs an ImgLib2 interval for the `MarsIntervalExporter`

### Processing Pipeline

The image processing follows this sequence:

1. **Region Extraction**: Extract DNA region using `MarsIntervalExporter`
2. **Frame Reduction**: Apply temporal reduction if specified
3. **Filtering**: Apply spatial filtering if specified
4. **Interpolation**: Increase resolution if specified
5. **Reflection**: Apply vertical reflection if specified
6. **Line Creation**: Create projection line from DNA coordinates
7. **Kymograph Generation**: Generate final kymograph using `KymographBuilder`

### Coordinate Transformation

The builder handles coordinate transformations:

- **Global to Local**: Converts DNA coordinates from global image space to the extracted region space
- **Line Construction**: Creates an ImageJ `Line` ROI with transformed coordinates
- **Projection Setup**: Configures the `LinesBuilder` with the correct line width

### Error Handling and Type Safety

The implementation includes robust error handling:

- **Type Checking**: Validates that images contain RealType pixels before processing
- **Parameter Validation**: Ensures valid values for filter parameters and thread counts
- **Graceful Degradation**: Returns original image if processing fails
- **Generic Safety**: Uses proper generic type handling with runtime type checking

### Integration with Mars Utilities

The builder leverages `MarsKymographUtils` for:

- **Frame Reduction**: `skipFrames()`, `averageFrames()`, `sumFrames()`
- **Image Filtering**: `applyMedianFilter()`, `applyGaussianFilter()`, `applyTopHatFilter()`
- **Resolution Enhancement**: `increaseResolution()`
- **Image Reflection**: `applyVerticalReflection()`

### Performance Considerations

For optimal performance:

1. **Threading**: Use multiple threads for filtering operations on large datasets
2. **Frame Reduction**: Apply frame reduction early to reduce processing load
3. **Filter Selection**: Choose appropriate filters based on data characteristics
4. **Memory Management**: Consider interpolation factor impact on memory usage

### Data Flow

```
DNA Molecule → Region Extraction → Frame Processing → Line Generation → Kymograph Creation
     ↓              ↓                    ↓                ↓               ↓
Parameters → MarsIntervalExporter → MarsKymographUtils → LinesBuilder → KymographBuilder
```

## Use Cases

The `DnaArchiveKymographBuilder` is ideal for:

- **Single-molecule DNA analysis**: Tracking DNA dynamics over time
- **Replication studies**: Monitoring DNA replication fork progression
- **Protein-DNA interactions**: Visualizing protein movement along DNA
- **High-throughput analysis**: Processing many DNA molecules with consistent parameters
- **Quality control**: Generating kymographs with standardized processing pipelines

This builder provides a comprehensive solution for DNA kymograph generation, combining the flexibility of the underlying components with the convenience of an integrated workflow specifically designed for DNA molecule analysis.
