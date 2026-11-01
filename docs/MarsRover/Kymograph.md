---
layout: marsrover
title: Kymograph
permalink: /docs/MarsRover/Kymograph/index.html
---

The Kymograph tab in the center of the Molecules tab creates kymographs and montages of the selected molecule with [Mars Kymograph](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/). It is shown right after the Plot tab when [Mars Kymograph](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/) is installed and the archive supports figures:

| :------------- | :------------- |
| DnaMoleculeArchive       | Kymographs along the DNA and montages of the region around the DNA.       |
| SingleMoleculeArchive       | Montages of the region around each spot. The tab is called Montage.       |
| TransverseFlowArchive       | Montages of the region around each molecule. The tab is called Montage.       |

Figures are made from the N5 sources configured for the metadata in the [BDV](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/bdv/) settings, with one channel per source.

### Settings

Settings are edited in the sections on the left. Each section maps onto one of the [Mars Kymograph](https://duderstadt-lab.github.io/mars-docs/docs/MarsKymograph/) builders, and the steps are always applied in the same order: frame reduction, filter, interpolation and reflection.

| :------------- | :------------- |
| Figure       | Kymograph or montage (DNA archives only) and the title. An empty title uses the molecule UID.       |
| Source       | Projection width for kymographs. Border width and height around the DNA or transverse flow molecule for montages. For spots, the radius of the square region and the table columns (mean position) or parameters used for the spot center.       |
| Time points  | The time point range and frame reduction: skip, average or sum groups of frames.       |
| Processing       | Median, Gaussian or top-hat filter and interpolation. For montages the filter can be applied to one channel only.       |
| Layout       | Orientation of the figure: horizontal, or rotated 90 degrees to the left or right, and vertical reflection (horizontal reflection for montages too). For montages, also how the time points are arranged (a single row, a single column or a grid with a set number of columns) and the spacing between them.       |
| Channels       | Show or hide each channel, choose the LUT (type to filter the list) and the display range. With Auto the display range is calculated and the values used are shown after generating, so they can be adjusted.       |
| Axes       | Axis labels, ranges, step sizes and precision. The x range can be calculated from the time of each frame in seconds, minutes or T. The automatic y range runs from 0 to the height of the region in image pixels; turn it off to enter your own range, for example in kb.       |
| Style       | Resolution (output pixels per image pixel), dark theme, stacked channels, font sizes, axis line width and tick length and width. Sizes are in pixels at resolution 1. Resolution and, for montages, interpolation increase the resolution of the figure without changing the size of the axes and text relative to the image.       |

### Generating figures

Press **Generate** to create the figure for the selected molecule. While it is being generated a progress indicator is shown over the figure and Generate turns into Cancel. Only one figure is generated at a time and the archive is not locked. If the selection changes or Generate is pressed again, the figure being generated is discarded.

The figure is stored in the molecule record together with the settings used, so it is shown again when the molecule is selected and is saved with the archive. Deleting a molecule also deletes its figure. When a molecule with a figure is selected, its settings are loaded. Turn on **Keep settings when switching molecules** to keep the current settings while looking through many molecules.

Turn on **Auto-generate on select** to generate a figure whenever a molecule without one is selected.

### Many molecules

Press **Batch...** or choose **Tools > Generate Figures...** to generate figures for all molecules, or only for molecules with certain tags. The figures can use the current settings for all molecules, or the settings stored with each molecule. Existing figures are kept unless **Replace existing** is selected. The archive is locked while the figures are generated, and molecules that fail are listed in the log.

### Export and scripts

**Export** saves the figure as a PNG image, or as an SVG vector image, which is generated again from the settings stored with the figure.

**Add to Comments** adds a new document to the [Comments](https://duderstadt-lab.github.io/mars-docs/docs/MarsRover/Comments/) with a `groovy-image-widget` fenced code block containing the script for the current settings. The script can be edited further or copied to the Fiji script editor.
