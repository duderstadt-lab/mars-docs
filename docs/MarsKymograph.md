---
layout: marskymograph
title: Mars Kymograph
permalink: /docs/MarsKymograph/index.html
---

**Mars** includes tools for creating kymographs and montages with an array of options for layouts, filters, labels and color schemes. These tools can be used in the [Comments](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/Comments/) tab of the **Mars Rover** with special fenced code blocks. Alternatively, these tools can be used in the Fiji scripting editor independently from other **Mars**  components to create PNG or SVG output images. Here method reference and sample documentation is provided for the available tools. 

**Mars Kymograph**

| :------------- | :------------ |
| [Interval Exporter](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/MarsIntervalExporter/) | Export an interval |
| [Dna Archive Kymograph](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/DnaArchiveKymographBuilder/) | Dna Archive Kymograph |
| [Dna Archive Region](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/DnaArchiveMoleculeRegionBuilder/) | Dna Archive Region for Montages |
| [Montage](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/MontageBuilder/) | Montage builder |
| [Object Archive Region](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/ObjectArchiveRegionBuilder/) | Extract region from Object Archive |
| [Object Formatter](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/ObjectShapeFormatter/) | Format Object Montage |
| [Transverse Flow Archive Region](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/TransverseFlowArchiveMoleculeRegionBuilder/) | Extract region from Transverse Flow Archive |
| [Image Formatter](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/ImageFormatter/) | Format Montage |

Sample script that can be used in a fenced code block in the **Mars Rover** comments tab with the keyword groovy-image-widget. Below is a comprehensive script that demonstrates all available options for the Mars Kymograph & Montage Builder. Options that are commented out represent alternatives or additional configurations that can be enabled as needed.

```groovy
#@ Context scijavaContext
#@ MoleculeArchive archive
#@OUTPUT String imgsrc

// Molecule identifier - can be changed to any valid UID in your archive
def UID = "2dBR8wa8GWRGsZKD1WaBkE"

// Import necessary classes
import de.mpg.biochem.mars.kymograph.DnaArchiveMoleculeRegionBuilder
import de.mpg.biochem.mars.kymograph.DnaArchiveKymographBuilder
import de.mpg.biochem.mars.kymograph.ObjectArchiveRegionBuilder
import de.mpg.biochem.mars.kymograph.TransverseFlowArchiveMoleculeRegionBuilder
import de.mpg.biochem.mars.kymograph.MontageBuilder
import de.mpg.biochem.mars.kymograph.ImageFormatter
import de.mpg.biochem.mars.kymograph.ObjectShapeFormatter
import de.mpg.biochem.mars.object.ObjectArchive
import de.mpg.biochem.mars.object.MartianObject
import de.mpg.biochem.mars.transverseflow.TransverseFlowArchive
import java.awt.Color
import java.awt.Font

// Retrieve time information from metadata for axis scaling
def omeImage = archive.getMetadata(archive.get(UID).getMetadataUID()).getImage(0)
def startTime = omeImage.getPlane(0, 0, 0).getDeltaTinSeconds()
def endTime = omeImage.getPlane(0, 0, omeImage.getSizeT() - 1).getDeltaTinSeconds()

//===============================================================
// STEP 1: Extract the molecule region from the archive
//===============================================================

// Option 1: Extract region around a DNA molecule
def regionBuilder = new DnaArchiveMoleculeRegionBuilder(scijavaContext, archive)
def imgPlus = regionBuilder
    .setMolecule(UID)                // Set molecule by UID
    //.setMolecule(archive.get(UID))  // Alternative: set molecule directly
    .setBorderWidth(10)              // Set horizontal border around DNA (pixels)
    .setBorderHeight(10)             // Set vertical border around DNA (pixels)
    .build()

// Option 2: Extract region around an object (if using ObjectArchive)
/*
def objectArchive = archive // Assuming archive is an ObjectArchive
def objectRegionBuilder = new ObjectArchiveRegionBuilder(scijavaContext, objectArchive)
def imgPlus = objectRegionBuilder
    .setObject(UID)                  // Set object by UID
    //.setObject(objectArchive.get(UID)) // Alternative: set object directly
    .setBorderWidth(10)              // Set horizontal border around object (pixels)
    .setBorderHeight(10)             // Set vertical border around object (pixels)
    .build()
*/

// Option 4: Create a kymograph directly from a DNA molecule (bypassing region extraction)
/*
def kymographBuilder = new DnaArchiveKymographBuilder(scijavaContext, archive)
def kymograph = kymographBuilder
    .setMolecule(UID)                // Set molecule by UID
    //.setMolecule(archive.get(UID))  // Alternative: set molecule directly
    .setProjectionWidth(3)           // Set width of the projection line (pixels)
    .setMinT(0)                      // Set minimum time point (optional)
    .setMaxT(50)                     // Set maximum time point (optional)
    // Frame reduction methods
    .skipFrames(2)                   // Include only every Nth frame
    //.averageFrames(2)              // Average groups of frames
    //.sumFrames(3)                  // Sum groups of frames
    //.useAllFrames()                // Reset to use all frames
    // Filtering methods
    .medianFilter(2)                 // Apply median filter with radius
    //.gaussianFilter(1.5)           // Apply Gaussian filter with sigma
    //.tophatFilter(2)               // Apply top-hat filter with radius
    .threads(4)                      // Number of threads for filtering
    .interpolation(2.0)              // Increase resolution by factor
    .setVerticalReflection(true)     // Vertically flip the frames
    .build()
def imgPlus = kymograph
*/

//===============================================================
// STEP 2: Create a montage from the extracted region
//===============================================================

def montageBuilder = new MontageBuilder(scijavaContext)
def montage = montageBuilder
    .setSource(imgPlus)              // Set the source image
    .setMinT(0)                      // Set minimum time point (first frame)
    .setMaxT(49)                     // Set maximum time point (limit to 50 frames)
    .setSpacing(5)                   // Set spacing between frames (pixels)
    .setHorizontalLayout(true)       // Arrange frames horizontally (left to right)
    //.setHorizontalLayout(false)    // Arrange frames vertically (top to bottom)
    .setColumns(5)                   // Set number of columns for grid layout
    
    // Frame reduction methods (choose one)
    .skipFrames(2)                   // Include only every Nth frame
    //.averageFrames(2)              // Average groups of frames
    //.sumFrames(3)                  // Sum groups of frames
    //.useAllFrames()                // Reset to use all frames
    
    // Filtering methods (optional, choose one or mix per channel)
    //.medianFilter(2)               // Apply median filter with radius to all channels
    //.medianFilter(2, 1)            // Apply median filter with radius to channel 1 only
    //.gaussianFilter(1.5)           // Apply Gaussian filter with sigma to all channels
    //.gaussianFilter(1.5, 2)        // Apply Gaussian filter with sigma to channel 2 only
    .tophatFilter(2)                 // Apply top-hat filter with radius to all channels
    //.tophatFilter(2, 3)            // Apply top-hat filter with radius to channel 3 only
    //.clearChannelFilters()         // Clear all channel-specific filters
    .threads(4)                      // Number of threads for filtering
    
    // Additional options
    .interpolation(2.0)              // Increase resolution by factor
    .setVerticalReflection(false)    // Don't vertically flip the frames
    .setHorizontalReflection(false)  // Don't horizontally flip the frames
    .build()

//===============================================================
// STEP 3: Format the montage for display
//===============================================================

def formatter = new ImageFormatter(scijavaContext, montage)
formatter
    // Basic configuration
    .setTitle("DNA Molecule ${UID}")  // Set the image title
    .setRescale(2)                    // Rescale the image by factor
    
    // Axis labels and ranges
    .setXAxisLabel("Time (s)")        // Set X-axis label
    .setYAxisLabel("Position (kb)")   // Set Y-axis label
    .setXAxisRange(startTime, endTime) // Set X-axis range
    .setYAxisRange(0, 20)             // Set Y-axis range
    
    // Tick marks and precision
    .setXStepSize(20)                 // Set X-axis tick mark spacing
    .setYStepSize(5)                  // Set Y-axis tick mark spacing
    .setXAxisPrecision(1)             // Set X-axis decimal precision
    .setYAxisPrecision(0)             // Set Y-axis decimal precision
    
    // Margins
    .setHorizontalMargin(50)          // Set horizontal margin (pixels)
    .setVerticalMargin(50)            // Set vertical margin (pixels)
    
    // Channel display options
    .setDisplayRangeMin(1, 0)         // Set min display value for channel 1
    .setDisplayRangeMax(1, 1000)      // Set max display value for channel 1
    .setDisplayRangeMin(2, 0)         // Set min display value for channel 2
    .setDisplayRangeMax(2, 4000)      // Set max display value for channel 2
    //.setDisplayRangeMin(3, 0)       // Set min display value for channel 3
    //.setDisplayRangeMax(3, 2000)    // Set max display value for channel 3
    
    // Color (LUT) settings
    .setLUT(1, "Blue")                // Set color for channel 1
    .setLUT(2, "Magenta")             // Set color for channel 2
    //.setLUT(3, "Green")             // Set color for channel 3
    
    // Channel selection
    //.onlyShowChannel(2)             // Show only channel 2
    //.onlyShowChannels(1, 2)         // Show only channels 1 and 2
    
    // Layout options
    .showChannelStack(25)             // Stack channels vertically with spacing
    //.horizontalOrientation()        // Default horizontal orientation
    .leftVerticalOrientation()        // Rotate 90° left (vertical orientation)
    //.rightVerticalOrientation()     // Rotate 90° right (vertical orientation)
    
    // Axis options
    //.hideXAxis()                    // Hide X-axis
    //.hideYAxis()                    // Hide Y-axis
    
    // Visual styling
    .setDarkTheme(true)               // Use dark theme
    //.setDarkTheme(false)            // Use light theme (default)
    .setAxisFont(new Font("Arial", Font.PLAIN, 12))       // Set axis number font
    .setLabelFont(new Font("Arial", Font.BOLD, 14))       // Set axis label font
    .setTitleFont(new Font("Arial", Font.BOLD, 16))       // Set title font
    //.setAxisFontSize(12)            // Alternative: set just the font size
    //.setLabelFontSize(14)           // Alternative: set just the font size
    //.setTitleFontSize(16)           // Alternative: set just the font size
    .setAxisLineWidth(1.5f)           // Set axis line width
    .setTickLength(5)                 // Set tick mark length
    .setTickLineWidth(1.0f)           // Set tick mark line width
    .setAxisColor(Color.WHITE)        // Set axis color (for dark theme)
    //.setAxisColor(Color.BLACK)      // Set axis color (for light theme)
    //.logFinalDisplayRanges()       // Log final display ranges to console for debugging
    
    // Build the formatted image
    .build()

// Get the final display ranges for debugging (optional)
/*
def displayRanges = formatter.getAllFinalDisplayRanges()
displayRanges.each { channel, range ->
    println "Channel ${channel}: min=${range[0]}, max=${range[1]}"
}
*/

//===============================================================
// STEP 4: Alternative - Format object shapes over time (if using ObjectArchive)
//===============================================================

/*
// This is an alternative to ImageFormatter when visualizing object shapes over time
def objectArchive = archive // Assuming archive is an ObjectArchive
def shapeFormatter = new ObjectShapeFormatter(scijavaContext, objectArchive)
shapeFormatter
    .setObject(UID)                   // Set object by UID
    //.setObject(objectArchive.get(UID)) // Alternative: set object directly
    
    // Basic configuration
    .setTitle("Object Shapes ${UID}")  // Set the title
    .setXAxisLabel("Time (frames)")    // Set X-axis label
    .setSpacing(10)                    // Set spacing between frames
    
    // Canvas sizing
    .setCanvasWidth(150)               // Set width of each shape canvas
    .setCanvasHeight(150)              // Set height of each shape canvas
    .setHorizontalMargin(50)           // Set horizontal margin
    .setVerticalMargin(50)             // Set vertical margin
    
    // Time range
    .setMinT(0)                        // Set minimum time point
    .setMaxT(49)                       // Set maximum time point
    .setFrameStep(2)                   // Include every Nth frame
    .showTimeLabels(true)              // Show time labels
    .normalizeShapes(true)             // Normalize shapes to fit canvas
    
    // Axis options
    .showAxes(true)                    // Show axes
    .setXAxisPrecision(0)              // Set decimal precision for time
    .setTimeUnitDivider(1)             // Divide time values (for unit conversion)
    
    // Font options
    .setAxisFontSize(12)               // Set axis number font size
    .setLabelFontSize(14)              // Set axis label font size
    .setTitleFontSize(16)              // Set title font size
    
    // Shape styling
    .setShapeColor(Color.BLUE)         // Set shape outline color
    .setShapeLineWidth(2.0f)           // Set shape line width
    .setFillShape(true)                // Fill shapes with color
    .setFillColor(new Color(0, 0, 255, 50)) // Set fill color with transparency
    
    // Layout options
    //.horizontalOrientation()         // Default horizontal orientation
    .rightVerticalOrientation()        // Rotate 90° right (vertical orientation)
    //.leftVerticalOrientation()       // Rotate 90° left (vertical orientation)
    
    // Theme
    .setDarkTheme(true)                // Use dark theme
    //.setDarkTheme(false)             // Use light theme (default)
    
    // Build the visualization
    .build()

// Get image as data URL
imgsrc = shapeFormatter.getHtmlEncodedImage()

// Alternative: save to file
// shapeFormatter.saveAsPNG("/path/to/output.png")
// shapeFormatter.saveAsSVG("/path/to/output.svg")
*/

//===============================================================
// STEP 5: Return the formatted image or save to file
//===============================================================

// Return HTML-encoded image for display
imgsrc = formatter.getHtmlEncodedImage()

// Alternative: save to file
// formatter.saveAsPNG("/path/to/output.png")
// formatter.saveAsSVG("/path/to/output.svg")
```

Other region extraction possibilities.

Option 2: Extract region around an object (if using ObjectArchive)
```groovy
def objectArchive = archive // Assuming archive is an ObjectArchive
def objectRegionBuilder = new ObjectArchiveRegionBuilder(scijavaContext, objectArchive)
def imgPlus = objectRegionBuilder
    .setObject(UID)                  // Set object by UID
    //.setObject(objectArchive.get(UID)) // Alternative: set object directly
    .setBorderWidth(10)              // Set horizontal border around object (pixels)
    .setBorderHeight(10)             // Set vertical border around object (pixels)
    .build()
```

Option 3: Extract region around a transverse flow molecule (if using TransverseFlowArchive)
```groovy
def transverseFlowArchive = archive // Assuming archive is a TransverseFlowArchive
def transverseFlowRegionBuilder = new TransverseFlowArchiveMoleculeRegionBuilder(scijavaContext, transverseFlowArchive)
def imgPlus = transverseFlowRegionBuilder
    .setMolecule(UID)                // Set molecule by UID
    //.setMolecule(transverseFlowArchive.get(UID)) // Alternative: set molecule directly
    .setBorderWidth(10)              // Set horizontal border around molecule (pixels)
    .setBorderHeight(10)             // Set vertical border around molecule (pixels)
    .build()
```

## Key Points About This Script

1. **Modular Structure**: The script is organized into distinct steps:
   - Extract region of interest from various archive types
   - Process into montage/kymograph
   - Format for display
   - Return or save the result

2. **Alternative Options**: Multiple options are provided with comments to demonstrate all available functionality:
   - Different region extraction methods (DNA, object, transverse flow molecule, direct kymograph)
   - Different processing options (filters, frame reduction methods, reflections)
   - Different formatting approaches (for images vs. object shapes)
   - Channel-specific filtering options

3. **Comprehensive Settings**: All available settings for each component are included:
   - Display ranges and LUTs for each channel
   - Font settings for different text elements
   - Axis configuration with ranges and step sizes
   - Layout options including orientation and stacking
   - Channel-specific and global filtering options

4. **Flexibility**: The script demonstrates how to adapt to different types of data:
   - DNA molecules from a MoleculeArchive
   - Objects from an ObjectArchive
   - Transverse flow molecules from a TransverseFlowArchive
   - Shape visualization over time

5. **Export Options**: Multiple export methods are shown:
   - HTML-encoded image for direct display
   - PNG file export
   - SVG file export

6. **Advanced Features**: The script showcases new capabilities:
   - Horizontal and vertical reflection options
   - Channel-specific filtering
   - Display range analysis and debugging
   - Support for multiple archive types

This template can be customized by uncommenting the desired options and adjusting parameters to meet specific visualization needs. The channel-specific filtering feature allows for sophisticated multi-channel processing where different channels can receive different treatments.
