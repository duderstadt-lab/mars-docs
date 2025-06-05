# MontageBuilder

## Overview

The `MontageBuilder` class creates montages from time series images. It arranges frames from a time series (with optional channel information) into a grid or linear layout, with flexible options for frame selection, arrangement, and preprocessing.

## Constructor

```java
MontageBuilder(Context context)
```

Creates a new builder for creating montages.

**Parameters:**
- `context`: The SciJava context

## Methods

### Source Configuration

| Method | Description |
|--------|-------------|
| `setSource(ImgPlus<?> imgPlus)` | Set the source image to use for building the montage.<br>**Parameters:** `imgPlus` - The source image<br>**Returns:** This builder for method chaining |

### Layout Configuration

| Method | Description |
|--------|-------------|
| `setSpacing(int spacing)` | Set spacing between frames in the montage.<br>**Parameters:** `spacing` - The spacing in pixels<br>**Returns:** This builder for method chaining |
| `setHorizontalLayout(boolean horizontal)` | Set the layout orientation.<br>**Parameters:** `horizontal` - True for horizontal layout (left to right), false for vertical (top to bottom)<br>**Returns:** This builder for method chaining |
| `setColumns(int columns)` | Set the number of columns for grid layout. If not set or set to -1, frames will be arranged in a single row or column based on the layout orientation.<br>**Parameters:** `columns` - Number of columns<br>**Returns:** This builder for method chaining |

### Time Range Configuration

| Method | Description |
|--------|-------------|
| `setMinT(int minT)` | Set the minimum time point to include.<br>**Parameters:** `minT` - The minimum time point<br>**Returns:** This builder for method chaining |
| `setMaxT(int maxT)` | Set the maximum time point to include.<br>**Parameters:** `maxT` - The maximum time point<br>**Returns:** This builder for method chaining |

### Frame Reduction Methods

| Method | Description |
|--------|-------------|
| `skipFrames(int skipFactor)` | Set the frame reduction method to skip frames. This will include only every Nth frame in the montage.<br>**Parameters:** `skipFactor` - Include every Nth frame (e.g., 5 means include frames 0, 5, 10, etc.)<br>**Returns:** This builder for method chaining |
| `averageFrames(int groupSize)` | Set the frame reduction method to average frames. This will average groups of N frames to create each montage frame.<br>**Parameters:** `groupSize` - Number of frames to average together<br>**Returns:** This builder for method chaining |
| `sumFrames(int groupSize)` | Set the frame reduction method to sum frames. This will sum groups of N frames to create each montage frame.<br>**Parameters:** `groupSize` - Number of frames to sum together<br>**Returns:** This builder for method chaining |
| `useAllFrames()` | Reset frame reduction to use all frames without reduction.<br>**Returns:** This builder for method chaining |

### Frame Processing

| Method | Description |
|--------|-------------|
| `setVerticalReflection(boolean reflect)` | Enable or disable vertical reflection (flipping) of the frames.<br>**Parameters:** `reflect` - True to vertically reflect frames, false for normal orientation<br>**Returns:** This builder for method chaining |
| `setHorizontalReflection(boolean reflect)` | Enable or disable horizontal reflection (flipping) of the frames.<br>**Parameters:** `reflect` - True to horizontally reflect frames, false for normal orientation<br>**Returns:** This builder for method chaining |
| `medianFilter(int radius)` | Set the filter method to median filter for all channels.<br>**Parameters:** `radius` - The radius of the median filter<br>**Returns:** This builder for method chaining |
| `medianFilter(int radius, int channel)` | Set the filter method to median filter for a specific channel.<br>**Parameters:** `radius` - The radius of the median filter, `channel` - The channel number (1-based)<br>**Returns:** This builder for method chaining |
| `gaussianFilter(double sigma)` | Set the filter method to Gaussian filter for all channels.<br>**Parameters:** `sigma` - The sigma value for the Gaussian filter<br>**Returns:** This builder for method chaining |
| `gaussianFilter(double sigma, int channel)` | Set the filter method to Gaussian filter for a specific channel.<br>**Parameters:** `sigma` - The sigma value for the Gaussian filter, `channel` - The channel number (1-based)<br>**Returns:** This builder for method chaining |
| `tophatFilter(int radius)` | Set the filter method to top-hat filter for all channels.<br>**Parameters:** `radius` - The radius of the top-hat filter<br>**Returns:** This builder for method chaining |
| `tophatFilter(int radius, int channel)` | Set the filter method to top-hat filter for a specific channel.<br>**Parameters:** `radius` - The radius of the top-hat filter, `channel` - The channel number (1-based)<br>**Returns:** This builder for method chaining |
| `clearChannelFilters()` | Clear all channel-specific filters and reset to global filtering mode.<br>**Returns:** This builder for method chaining |
| `threads(int numThreads)` | Set the number of threads to use for filter operations.<br>**Parameters:** `numThreads` - The number of threads<br>**Returns:** This builder for method chaining |
| `interpolation(double factor)` | Enable interpolation to increase resolution.<br>**Parameters:** `factor` - The factor by which to increase resolution<br>**Returns:** This builder for method chaining |

### Building the Montage

| Method | Description |
|--------|-------------|
| `build()` | Builds the montage.<br>**Returns:** A `Dataset` containing the montage |
| `getMontage()` | Get the built montage (same as returned by `build()`).<br>**Returns:** The `Dataset` containing the montage |

## Enums

### FrameReductionMethod

| Value | Description |
|-------|-------------|
| `NONE` | Use all frames |
| `SKIP` | Skip frames (e.g., every Nth frame) |
| `AVERAGE` | Average groups of frames |
| `SUM` | Sum groups of frames |

### ImageFilterMethod

| Value | Description |
|-------|-------------|
| `NONE` | No filter |
| `MEDIAN` | Median filter |
| `GAUSSIAN` | Gaussian filter |
| `TOPHAT` | Top-hat filter |

## Example Usage

```java
// Create the builder
MontageBuilder montageBuilder = new MontageBuilder(context);

// Configure and build
Dataset montage = montageBuilder
    .setSource(imgPlus)
    .setMinT(0)
    .setMaxT(49)
    .setSpacing(5)
    .setHorizontalLayout(true)
    .setColumns(5)
    .skipFrames(2)
    .tophatFilter(2)
    .setVerticalReflection(false)
    .build();
```

## Implementation Details

The `MontageBuilder` class processes a time series image to create a montage. It can apply various filters from the `MarsKymographUtils` class before arranging the frames. The montage can be arranged in horizontal or vertical layouts, or in a grid with a specified number of columns. Frame reduction methods can be used to include only a subset of frames, or to combine frames through averaging or summing.

### Channel-Specific Filtering

The builder supports both global filtering (applied to all channels) and channel-specific filtering. When channel-specific filters are used, the global filter is automatically disabled. Channel filters are applied independently to each specified channel, allowing for different processing of different channels in multi-channel images.

### Nested Classes

#### ChannelFilterConfig

A static nested class that stores filter configuration for individual channels:

- `method`: The `ImageFilterMethod` to apply
- `size`: The filter size parameter (radius for median/top-hat, sigma for Gaussian)
