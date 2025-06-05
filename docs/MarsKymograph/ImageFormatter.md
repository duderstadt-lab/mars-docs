---
layout: marskymograph
title: Image Formatter
permalink: /docs/MarsKymograph/ImageFormatter/index.html
---
The `ImageFormatter` class formats images (such as kymographs or montages) with axes, labels, and visual styling for presentation. It provides extensive control over appearance, including layout, axis configuration, color management, and various export options.

## Constructor

```java
ImageFormatter(Context context, Dataset kymograph)
```

Creates a new formatter for the specified dataset.

**Parameters:**
- `context`: The SciJava context
- `kymograph`: The dataset to format (can be any image, not just kymographs)

## Methods

### Visual Style Configuration

| Method | Description |
|--------|-------------|
| `setAxisFontSize(int fontSize)` | Set the font size for axis numbers.<br>**Parameters:** `fontSize` - The font size in points<br>**Returns:** This formatter for method chaining |
| `setAxisFont(Font axisFont)` | Set the font for axis numbers.<br>**Parameters:** `axisFont` - The font<br>**Returns:** This formatter for method chaining |
| `setLabelFontSize(int fontSize)` | Set the font size for axis labels.<br>**Parameters:** `fontSize` - The font size in points<br>**Returns:** This formatter for method chaining |
| `setLabelFont(Font labelFont)` | Set the font for axis labels.<br>**Parameters:** `labelFont` - The font<br>**Returns:** This formatter for method chaining |
| `setTitleFontSize(int fontSize)` | Set the font size for the title.<br>**Parameters:** `fontSize` - The font size in points<br>**Returns:** This formatter for method chaining |
| `setTitleFont(Font titleFont)` | Set the font for the title.<br>**Parameters:** `titleFont` - The font<br>**Returns:** This formatter for method chaining |
| `setAxisLineWidth(float width)` | Set the axis line width.<br>**Parameters:** `width` - The line width in pixels<br>**Returns:** This formatter for method chaining |
| `setTickLength(int length)` | Set the tick mark length.<br>**Parameters:** `length` - The length in pixels<br>**Returns:** This formatter for method chaining |
| `setTickLineWidth(float width)` | Set the tick mark line width.<br>**Parameters:** `width` - The line width in pixels<br>**Returns:** This formatter for method chaining |
| `setAxisColor(Color color)` | Set the axis color (applies to both axis lines and tick marks).<br>**Parameters:** `color` - The color<br>**Returns:** This formatter for method chaining |
| `setDarkTheme(boolean darkTheme)` | Enable or disable dark theme.<br>**Parameters:** `darkTheme` - True for dark theme, false for light theme<br>**Returns:** This formatter for method chaining |

### Display Range and Color Configuration

| Method | Description |
|--------|-------------|
| `setDisplayRangeMin(int c, double min)` | Set the minimum display value for a channel.<br>**Parameters:** `c` - The channel index (1-based), `min` - The minimum value<br>**Returns:** This formatter for method chaining |
| `setDisplayRangeMax(int c, double max)` | Set the maximum display value for a channel.<br>**Parameters:** `c` - The channel index (1-based), `max` - The maximum value<br>**Returns:** This formatter for method chaining |
| `setLUT(int c, String lut)` | Set the lookup table (color palette) for a channel.<br>**Parameters:** `c` - The channel index (1-based), `lut` - The name of the LUT (e.g., "Blue", "Magenta", "Green", etc.)<br>**Returns:** This formatter for method chaining |
| `setRescale(int rescaleFactor)` | Set the rescale factor for the image.<br>**Parameters:** `rescaleFactor` - The rescale factor (e.g., 2 for 2x size)<br>**Returns:** This formatter for method chaining |

### Axis Configuration

| Method | Description |
|--------|-------------|
| `setXAxisLabel(String xAxisLabel)` | Set the X-axis label.<br>**Parameters:** `xAxisLabel` - The label text<br>**Returns:** This formatter for method chaining |
| `setYAxisLabel(String yAxisLabel)` | Set the Y-axis label.<br>**Parameters:** `yAxisLabel` - The label text<br>**Returns:** This formatter for method chaining |
| `setTitle(String title)` | Set the title of the image.<br>**Parameters:** `title` - The title text<br>**Returns:** This formatter for method chaining |
| `setXAxisPrecision(int xAxisPrecision)` | Set the decimal precision for X-axis values.<br>**Parameters:** `xAxisPrecision` - The number of decimal places<br>**Returns:** This formatter for method chaining |
| `setYAxisPrecision(int yAxisPrecision)` | Set the decimal precision for Y-axis values.<br>**Parameters:** `yAxisPrecision` - The number of decimal places<br>**Returns:** This formatter for method chaining |
| `setXAxisRange(double min, double max)` | Set the range of values for the X-axis.<br>**Parameters:** `min` - Minimum value, `max` - Maximum value<br>**Returns:** This formatter for method chaining |
| `setYAxisRange(double min, double max)` | Set the range of values for the Y-axis.<br>**Parameters:** `min` - Minimum value, `max` - Maximum value<br>**Returns:** This formatter for method chaining |
| `setXStepSize(double xStepSize)` | Set the step size between tick marks on the X-axis.<br>**Parameters:** `xStepSize` - The step size<br>**Returns:** This formatter for method chaining |
| `setYStepSize(double yStepSize)` | Set the step size between tick marks on the Y-axis.<br>**Parameters:** `yStepSize` - The step size<br>**Returns:** This formatter for method chaining |
| `hideXAxis()` | Hide the X-axis.<br>**Returns:** This formatter for method chaining |
| `hideYAxis()` | Hide the Y-axis.<br>**Returns:** This formatter for method chaining |

### Layout Configuration

| Method | Description |
|--------|-------------|
| `setHorizontalMargin(int horizontalMargin)` | Set the horizontal margin around the image.<br>**Parameters:** `horizontalMargin` - The margin in pixels<br>**Returns:** This formatter for method chaining |
| `setVerticalMargin(int verticalMargin)` | Set the vertical margin around the image.<br>**Parameters:** `verticalMargin` - The margin in pixels<br>**Returns:** This formatter for method chaining |
| `showChannelStack(int spacing)` | Show channels stacked vertically with the specified spacing.<br>**Parameters:** `spacing` - The spacing in pixels between channels<br>**Returns:** This formatter for method chaining |
| `rightVerticalOrientation()` | Set the orientation to vertical with the image rotated 90 degrees to the right.<br>**Returns:** This formatter for method chaining |
| `leftVerticalOrientation()` | Set the orientation to vertical with the image rotated 90 degrees to the left.<br>**Returns:** This formatter for method chaining |
| `horizontalOrientation()` | Reset the orientation to the default horizontal layout.<br>**Returns:** This formatter for method chaining |

### Channel Selection

| Method | Description |
|--------|-------------|
| `onlyShowChannel(int channelIndex)` | Specify that only a single channel should be shown in the output.<br>**Parameters:** `channelIndex` - The 1-based index of the channel to show<br>**Returns:** This formatter for method chaining |
| `onlyShowChannels(int... channelIndices)` | Specify that only certain channels should be shown in the output.<br>**Parameters:** `channelIndices` - An array of 1-based indices of the channels to show<br>**Returns:** This formatter for method chaining |

### Building and Exporting

| Method | Description |
|--------|-------------|
| `build()` | Build the formatted image.<br>**Returns:** void |
| `getHtmlEncodedImage()` | Get the formatted image as an HTML-encoded data URL string (PNG format).<br>**Returns:** A Base64-encoded data URL string |
| `saveAsSVG(String path)` | Save the formatted image as an SVG file.<br>**Parameters:** `path` - The file path to save to (should end with .svg)<br>**Returns:** void |
| `saveAsPNG(String path)` | Save the formatted image as a PNG file.<br>**Parameters:** `path` - The file path to save to (should end with .png)<br>**Returns:** This formatter for method chaining |

### Display Range Analysis

| Method | Description |
|--------|-------------|
| `getFinalDisplayRangeMin(int channel)` | Get the final display range minimum value for a specific channel after processing (includes manual settings or auto contrast).<br>**Parameters:** `channel` - The channel number (1-based)<br>**Returns:** The minimum display value, or Double.NaN if channel doesn't exist |
| `getFinalDisplayRangeMax(int channel)` | Get the final display range maximum value for a specific channel after processing (includes manual settings or auto contrast).<br>**Parameters:** `channel` - The channel number (1-based)<br>**Returns:** The maximum display value, or Double.NaN if channel doesn't exist |
| `getFinalDisplayRange(int channel)` | Get the final display range (min and max) for a specific channel after processing.<br>**Parameters:** `channel` - The channel number (1-based)<br>**Returns:** A double array [min, max], or null if channel doesn't exist |
| `getAllFinalDisplayRanges()` | Get the final display ranges for all channels after processing.<br>**Returns:** A Map where keys are channel numbers (1-based) and values are double arrays [min, max] |
| `getAllFinalDisplayRangeMins()` | Get the final display range minimums for all channels after processing.<br>**Returns:** A Map where keys are channel numbers (1-based) and values are minimum display values |
| `getAllFinalDisplayRangeMaxs()` | Get the final display range maximums for all channels after processing.<br>**Returns:** A Map where keys are channel numbers (1-based) and values are maximum display values |
| `logFinalDisplayRanges()` | Print the final display ranges for all channels to the log. Useful for debugging and understanding auto contrast results.<br>**Returns:** void |

## Example Usage

```java
// Create the formatter
ImageFormatter formatter = new ImageFormatter(context, dataset);

// Configure and build
formatter
    .setRescale(2)
    .setTitle("DNA Molecule")
    .setXAxisLabel("Time (s)")
    .setXAxisRange(0, 100)
    .setYAxisLabel("Position (kb)")
    .setYAxisRange(0, 20)
    .setDisplayRangeMin(1, 0)
    .setDisplayRangeMax(1, 255)
    .setLUT(1, "Blue")
    .rightVerticalOrientation()
    .setYStepSize(5)
    .setXStepSize(20)
    .showChannelStack(25)
    .setDarkTheme(true)
    .build();

// Get the image as a data URL for display in HTML
String dataUrl = formatter.getHtmlEncodedImage();
```

## Implementation Details

The `ImageFormatter` class provides extensive control over the appearance of images for presentation. It can handle multi-channel images, with options to show specific channels or display all channels in a stack. The class supports both horizontal and vertical orientations, with axis labels, tick marks, and a title. Images can be exported as PNG or SVG files.
