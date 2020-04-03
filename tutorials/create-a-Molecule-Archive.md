---
layout: tutorials
title: "Let's Make A MoleculeArchive Tutorial"
permalink: /tutorials/create-a-Molecule-Archive/index.html
---

_level: beginner, duration: 5-10 min_

In this tutorial we will go through the basics of loading a video, finding
and tracking peaks, navigating through the MARS GUI, looking at the traces
corresponding to the tracked peaks, tagging them and saving the result as a
MoleculeArchive (.yama) file.

This tutorial is based on an example dataset called 'TestVideo.tif'. This video
investigates the transcription movement of fluorescently labeled T7 polymerase
over DNA by means of TIRF microscopy. The DNA itself is not labeled. This
single-molecule dataset provides an excellent way to familiarise yourself with
MARS and the most important plugins.

#### 1. Import a video in FIJI
Open the example dataset 'TestVideo.tif'. This will open the video in a new
screen.

[img1][img2]

__Background Information:__

When playing the movie, it is clear that molecules with different behaviour
are observed:
- Fast moving molecules only present in a few subsequent frames: these are
molecules that are not associated with a DNA molecule and float in buffer.
Note that the buffer flow direction in this experiment is opposite (upwards)
from that of the transcription movement of the molecules of interest (downwards).
- Moving molecules that travel downward slowly over a large amount of frames:
these are transcribing enzymes.
- Disappearing molecules towards the end of the movie: fluorophores that are
photobleached over time.

#### 2. Optional: Use peak finder to find peaks
The next step is to find the peaks in each frame. Open the peak finder of MARS
as indicated below. This will open a new window in which settings and
selection criteria have to be provided.

[img3][img7][img8]

Now set the parameters to the values indicated in the image. Make sure to tick
the box 'preview' and have a look at how the peaks that are found change when
changing some of the settings. This should look like this:

[img9]

The results do not have to be saved.

_note:_ This step is optional since it is also automatically done when running
the peak tracker plugin (next step). However, we do recommend to also run this
step since it allows for an easier way to optimise the selected settings for
peak finding.

__Background Information:__
Explanation of settings to set:
- ROI: instead of analysing the whole frame, one can select a region of interest
(ROI) instead. Set this manually in this window, or select a region on the image
by clicking and dragging the mouse to form a square, before starting the plugin.
[img5]
- Discoidal Averaging Filter: enhances peaks in a noisy image by applying the
following rule to the intensity value of each pixel: new intensity = old
intensity pixel - sum of intensity in surrounding pixels.
In case it hits a pixel that has a peak, this will enhance the intensity, whilst
the intensity of a noise pixel will be diminished.
- Peak finding parameters: (provided in the unit # pixels)
  - Inner radius: radius in which the peak must be found.
  - Outer radius: radius that should be the background around the peaks.
  - Detection threshold(N): sets the sensitivity of which peak will be found.
  - Minimum distance between peaks

  To find a peak, the global mean intensity of the image is calculated as well
  as the standard deviation. These values are used to calculate the threshold
  criterium using: Threshold = mean + N * standard deviation.
  Next, the system loops through all the pixels to check if pixel intensity >
  threshold. If this is true it marks the pixel as a peak.

- Find Negative Peaks: finds pixels which are darker than their surroundings.
Not relevant for this dataset.
- Generate peak count table: if ticked, opens a new window in which the total
amount of peaks found can be found.
- Generate peak table: if ticked, opens a new window showing the coordinates
of all peaks found.
- Add to ROI manager: if ticked, adds all the peaks to the ROI manager.
In this manager all peaks are listed and can be followed through different frames.
- Molecule Names in Manager: gives each peak a number.
- Process all slides: execute the peak finding procedure with these settings
to each slide (frame) of the movie.
- Preview: if ticked, shows on the image which spots are considered peaks with
the settings provided.
- Peak fitter settings: applied if the box 'Fit peaks' is ticked.
Fits the peak to a Gaussian distribution to find the middle of the peak in
subpixel resolution. Requires the input of initial fitting conditions.
Apart from initial fitting parameters, one can set the error allowances.
  - Fit radius
  - Initial Baseline
  - Initial Height
  - Initial Sigma
  - Max Error Baseline
  - Max Error Height
  - Max Error X
  - Max Error Y
  - Max Error Sigma

#### 3. Use peak tracker to track peaks through frames
Now, open the peak tracker plugin and supply all settings found in step 2. This
makes sure that the tracker can find the peaks correctly.

[img9][img10][img11]

Press OK to run the tracking plugin and proceed to the MARS GUI.
The following window will open:

[img12]

__Background Information:__
On top of the peak tracking parameters, also tracker and peak integration
parameters have to be set.
- Peak tracker settings (all in # pixels):
  - Max Difference X: maximum distance a fluorophore can travel between frames
  in the X direction.
  - Max Difference Y: maximum distance a fluorophore can travel between frames
  in the Y direction.
  - Max Difference Slice: maximum distance a fluorophore can change between two
  frames.
  - Min trajectory length: minimum amount of displacement that has to be covered
  in the total experiment. This filters for molecules that transcribed a piece
  of DNA of in this case 10 pixels length.

- Peak integration settings:
  - Inner radius
  - Outer radius
  - Microscope: select the instrument on which the data was recorded. This
  makes it easier to connect the data to the metadata specific to the instrument.
  The microscope on which this dataset was collected is called "Dobby".
  - Format

#### 4. Navigating through the MARS GUI

The GUI automatically opens on the desktop page. In this overview page you can
see information on your dataset, and get some statistical data. More statistical
data will become available when we have analysed the data in the next steps.

[img14]

By using the tabs on the left, we can access the data.
Click on the microscope icon to reveal more information about the video
(DataTable tab) and used settings (Log tab). Next to this, the ID in the left
frame shows from which experiment number the data in this set originates.
The DataTable tab shows the slice numbers and the corresponding relative time
when the slide was recorded.

[img15]

Next, open the molecule tab. This tab reveals a table with all molecules found.
Each molecule is assigned an unique identifier (UID) by which the molecule can
be easily traced back after analysis. Click on an UID to see the corresponding
data in the adjacent DataTable. This table shows the coordinates and intensity
corresponding to the tracked peak over different slides.

[img16]

#### 5. Plotting traces
Switch to the Plot tab in order to plot this data.

In the top left corner, click on the plotting icon. This opens a menu in which
the following settings can be provided. This makes a line plot of the y position
versus the slide number. In this dataset that corresponds with the direction of
transcription over the DNA. Press the refresh button to show the graph.

[img17][img18]

Move through the UIDs on the left to show the plot for every molecule. As you
will see some molecules show a trace in which the y position almost
linearly changes with the slice number. These are the molecules that are
relevant for this dataset since they correspond to a T7 polymerase that
transcribes the DNA with a uniform speed.

[img19]

#### 6. Tagging data

When interesting molecules in the data have been found, one can easily Tag them
for later reference.
Write 'Active' in the Tag window on the right and press enter. Move through
all UIDs and select all molecules fulfilling the criteria in this way.

[img20]

#### 7. Saving the MoleculeArchive

The archive can be safed as a .yama file by selecting the save option.

[img21]
