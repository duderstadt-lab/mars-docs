---
layout: marskymograph
title: Object Formatter
permalink: /docs/MarsKymograph/ObjectShapeFormatter/index.html
---

The `ObjectShapeFormatter` class creates visualizations of object shapes over time from a `MartianObject`. It provides extensive control over the appearance and layout of shape montages, including orientation options, styling, and export capabilities.

## Constructors

```java
ObjectShapeFormatter(Context context)
ObjectShapeFormatter(Context context, ObjectArchive objectArchive)
```

Creates a new formatter for object shapes.

**Parameters:**
- `context`: The SciJava context
- `objectArchive`: (Optional) The archive containing the objects

## Methods

### Object Configuration

| Method | Description |
|--------|-------------|
| `setObject(MartianObject martianObject)` | Set the object to use for shape visualization.<br>**Returns:** This formatter for method chaining |
| `setObject(String UID)` | Set the object to use by UID (requires ObjectArchive to be set).<br>**Parameters:** `UID` - The unique identifier of the object<br>**Returns:** This formatter for method chaining |

### Layout Configuration

| Method | Description |
|--------|-------------|
| `setTitle(String title)` | Set the title for the montage.<br>**Parameters:** `title` - The title string<br>**Returns:** This formatter for method chaining |
| `setXAxisLabel(String label)` | Set the x-axis label.<br>**Parameters:** `label` - The x-axis label<br>**Returns:** This formatter for method chaining |
| `setSpacing(int spacing)` | Set spacing between frames in the montage.<br>**Parameters:** `spacing` - The spacing in pixels<br>**Returns:** This formatter for method chaining |
| `setCanvasWidth(int width)` | Set the width of each canvas for individual shapes.<br>**Parameters:** `width` - Width in pixels<br>**Returns:** This formatter for method chaining |
| `setCanvasHeight(int height)` | Set the height of each canvas for individual shapes.<br>**Parameters:** `height` - Height in pixels<br>**Returns:** This formatter for method chaining |
| `setHorizontalMargin(int margin)` | Set horizontal margin.<br>**Parameters:** `margin` - Margin in pixels<br>**Returns:** This formatter for method chaining |
| `setVerticalMargin(int margin)` | Set vertical margin.<br>**Parameters:** `margin` - Margin in pixels<br>**Returns:** This formatter for method chaining |

### Time Range Configuration

| Method | Description |
|--------|-------------|
| `setMinT(int minT)` | Set the minimum time point to include.<br>**Parameters:** `minT` - The minimum time point<br>**Returns:** This formatter for method chaining |
| `setMaxT(int maxT)` | Set the maximum time point to include.<br>**Parameters:** `maxT` - The maximum time point<br>**Returns:** This formatter for method chaining |
| `setFrameStep(int step)` | Set the step size between frames to include.<br>**Parameters:** `step` - Include every Nth frame<br>**Returns:** This formatter for method chaining |
| `setTimeUnitDivider(int divider)` | Set the time unit divider for converting time units.<br>**Parameters:** `divider` - Division factor for time values<br>**Returns:** This formatter for method chaining |

### Display Options

| Method | Description |
|--------|-------------|
| `showTimeLabels(boolean show)` | Enable or disable time labels.<br>**Parameters:** `show` - True to show time labels, false to hide<br>**Returns:** This formatter for method chaining |
| `normalizeShapes(boolean normalize)` | Enable or disable shape normalization (scaling to fit canvas).<br>**Parameters:** `normalize` - True to normalize shapes, false to use original scale<br>**Returns:** This formatter for method chaining |
| `showAxes(boolean show)` | Enable or disable axes.<br>**Parameters:** `show` - True to show axes, false to hide<br>**Returns:** This formatter for method chaining |
| `setXAxisPrecision(int precision)` | Set the precision for the time display.<br>**Parameters:** `precision` - Number of decimal places to show<br>**Returns:** This formatter for method chaining |

### Font Configuration

| Method | Description |
|--------|-------------|
| `setAxisFont(Font font)` | Set the font for axis text.<br>**Parameters:** `font` - Font for the axis text<br>**Returns:** This formatter for method chaining |
| `setAxisFontSize(int size)` | Set the axis font size.<br>**Parameters:** `size` - Font size in points<br>**Returns:** This formatter for method chaining |
| `setLabelFont(Font font)` | Set the font for labels.<br>**Parameters:** `font` - Font for labels<br>**Returns:** This formatter for method chaining |
| `setLabelFontSize(int size)` | Set the label font size.<br>**Parameters:** `size` - Font size in points<br>**Returns:** This formatter for method chaining |
| `setTitleFont(Font font)` | Set the font for the title.<br>**Parameters:** `font` - Font for the title<br>**Returns:** This formatter for method chaining |
| `setTitleFontSize(int size)` | Set the title font size.<br>**Parameters:** `size` - Font size in points<br>**Returns:** This formatter for method chaining |

### Shape Styling

| Method | Description |
|--------|-------------|
| `setShapeColor(Color color)` | Set the shape outline color.<br>**Parameters:** `color` - Color for shape outlines<br>**Returns:** This formatter for method chaining |
| `setShapeLineWidth(float width)` | Set the shape line width.<br>**Parameters:** `width` - Line width in pixels<br>**Returns:** This formatter for method chaining |
| `setFillShape(boolean fill)` | Enable or disable shape filling.<br>**Parameters:** `fill` - True to fill shapes, false for outlines only<br>**Returns:** This formatter for method chaining |
| `setFillColor(Color color)` | Set the fill color for shapes.<br>**Parameters:** `color` - Fill color (can include alpha transparency)<br>**Returns:** This formatter for method chaining |

### Axis Styling

| Method | Description |
|--------|-------------|
| `setAxisColor(Color color)` | Set the axis color.<br>**Parameters:** `color` - Color for axes<br>**Returns:** This formatter for method chaining |
| `setAxisLineWidth(float width)` | Set the axis line width.<br>**Parameters:** `width` - Line width in pixels<br>**Returns:** This formatter for method chaining |

### Orientation Configuration

| Method | Description |
|--------|-------------|
| `rightVerticalOrientation()` | Set orientation to vertical with 90° right rotation. Shapes displayed in vertical column with time increasing downward.<br>**Returns:** This formatter for method chaining |
| `leftVerticalOrientation()` | Set orientation to vertical with 90° left rotation. Shapes displayed in vertical column with time increasing downward.<br>**Returns:** This formatter for method chaining |
| `horizontalOrientation()` | Reset to default horizontal layout.<br>**Returns:** This formatter for method chaining |

### Theme Configuration

| Method | Description |
|--------|-------------|
| `setDarkTheme(boolean darkTheme)` | Enable or disable dark theme.<br>**Parameters:** `darkTheme` - True for dark theme, false for light theme<br>**Returns:** This formatter for method chaining |

### Building and Export

| Method | Description |
|--------|-------------|
| `build()` | Build the shape montage.<br>**Returns:** This formatter after creating the montage |
| `getHtmlEncodedImage()` | Get the HTML-encoded image as a data URL string.<br>**Returns:** Base64-encoded PNG image with data URL prefix |
| `getImage()` | Get the buffered image.<br>**Returns:** The montage as a BufferedImage |
| `saveAsPNG(String path)` | Save the montage as a PNG file.<br>**Parameters:** `path` - The file path to save to<br>**Returns:** This formatter for method chaining |
| `saveAsSVG(String path)` | Save the montage as an SVG file.<br>**Parameters:** `path` - The file path to save to (should end with .svg)<br>**Returns:** This formatter for method chaining |

## Example Usage

```java
// Create the formatter
ObjectShapeFormatter formatter = new ObjectShapeFormatter(context, objectArchive);

// Configure and build
formatter
    .setObject("objectUID")
    .setTitle("Object Shapes Over Time")
    .setXAxisLabel("Time (frames)")
    .setSpacing(10)
    .setCanvasWidth(150)
    .setCanvasHeight(150)
    .setMinT(0)
    .setMaxT(49)
    .setFrameStep(2)
    .showTimeLabels(true)
    .normalizeShapes(true)
    .setShapeColor(Color.BLUE)
    .setFillShape(true)
    .rightVerticalOrientation()
    .setDarkTheme(true)
    .build();

// Get the result
String dataUrl = formatter.getHtmlEncodedImage();
```

## Implementation Details

The `ObjectShapeFormatter` creates a montage of object shapes across time by:

1. Extracting shape data from the MartianObject for selected time points
2. Optionally normalizing shapes to a common scale
3. Drawing each shape on individual canvases
4. Arranging canvases in horizontal or vertical layouts
5. Adding time labels and axis labels as configured
6. Supporting both PNG and SVG export formats

The formatter supports both horizontal and vertical orientations, with vertical orientations rotating both the shapes and text appropriately for the new layout.
