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

This tutorial is based on an example dataset called 'TestVideo.tif'. In this video,
the transcription movement of fluorescently labeled T7 polymerase
over DNA is investigated by means of TIRF microscopy. The DNA itself is not labeled
and therefore is not visible in the video. This
single-molecule dataset provides an excellent way to familiarise yourself with
MARS and the most important plugins.

### 1. Import a video in Fiji
Open the example dataset 'TestVideo.tif' in Fiji. This will open the video in a new
screen. Use the slider below the video to go through all recorded frames.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img1.png' width='150' />
<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img2.png' width='450' />


__Background Information:__

When playing the movie, it is clear that molecules with different behaviour
are observed:
- Fast moving molecules only present in a few subsequent frames: these are
molecules that are not associated with a DNA molecule and float in the buffer.
Note that the buffer flow direction in this experiment is opposite (upwards)
from that of the transcription movement of the molecules of interest (downwards).
- Moving molecules that travel downward slowly over a large amount of frames:
these are transcribing polymerases.
- Disappearing molecules towards the end of the movie: fluorophores that are
photobleached over time.

### 2. Optional: Use peak finder to find peaks
The next step is to find the peaks in each frame. Open the peak finder of MARS
as indicated below.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img3.png' width='450' />

This will open a new window in which settings and
selection criteria have to be provided.


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img7.png' width='250' />  
<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img8.png' width='250' />

Now set the parameters to the values indicated in the image. Make sure to tick
the box 'preview' and have a look at how the peaks that are found change when
changing some of the settings. This should look like this the image below. Use
the slider to have a look at the found peaks on other frames (slices) of the video.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img9.png' width='450' />

Press OK when you found the right settings for the data. The results do not have to be saved at this point to continue with the tutorial.

_note:_ This step is optional since it is also automatically done when running
the peak tracker plugin (next step). However, we do recommend to also run this
step since it allows for an easier way to optimise the selected settings for
peak finding.

__Background Information:__
Explanation of settings to set:
(Also have a look at the documentation section for a more thorough explanation)
- ROI: instead of analysing the whole frame, one can select a region of interest
(ROI) instead. Set this manually in this window, or select a region on the image
by clicking and dragging the mouse to form a square, before starting the plugin.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img5.png' width='450' />

- DoG filter: tick if you want to filter the raw images before analysis. The
difference of gaussians (DoG) filter enhances bright spots and decreases noise.
For more information have a look at the documentation section of the software.
  - DoG filter radius: radius used during fitting. It should reflect the size of the desired peaks.
  - Detection threshold: the raw pixel intensity value that is used as a cut-off to identify peaks. We found that optimal detection is obtained for values between 40 and 60.
  - Minimum distance between peaks (pixels): provide the amount of pixels there need to be between peaks to prevent the software from identifying one peak multiple times.

- Preview: previews the fit results on the image. Use the preview slice slider to go through the different slices of the dataset.
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
- Fit peaks: tick if you want to fit 2D Gaussians to the peaks found to determine their sub pixel position.
  - Fit radius: radius of the Gaussian that is used for fitting.
  - Minimum R-squared: allowed error margin for the fit.
- Integrate: tick if you want to integrate the peak intensities.
  - Inner radius: radius in which the peak should be found. All intensities are added together.
  - Outer radius: radius in which the background can be found. The total intensity in these pixels will be substracted from the intensity found in the inner radius.
- Verbose output: if checked all fit parameters are included in the output. This might be of use for further analysis.


### 3. Use peak tracker to track peaks through frames
Now, open the peak tracker plugin and supply all settings found in step 2. This
makes sure that the tracker can find the peaks correctly. As you notice, the peak tracker wizard looks similar to the peak finder window.


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img10.png' width='450' />


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img11.png' width='250' />  
<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img12.png' width='250' />


Press OK to run the tracking plugin and proceed to the MARS GUI.

__Background Information:__
On top of the peak tracking parameters, some more parameters have to be set.
- Peak tracker settings (all in # pixels):
  - Max difference X: maximum distance a fluorophore can travel between frames
  in the X direction.
  - Max difference Y: maximum distance a fluorophore can travel between frames
  in the Y direction.
  - Max difference Slice: maximum distance a fluorophore can change between two
  frames.
  - Minimum track length: minimum amount of displacement that has to be covered
  in the total experiment. This filters for molecules that transcribed a piece
  of DNA of in this case 10 pixels length.
- Microscope: select the instrument on which the data was recorded. This
makes it easier to connect the data to the metadata specific to the instrument.
The microscope on which this dataset was collected is called "Dobby".
- Format: data format of the metadata generated by your microscope.

### 4. Navigating through the MARS GUI

The GUI automatically opens on the desktop page. In this overview page you can
see information on your dataset, and get some statistical data. More statistical
data will become available when we have analysed the data in the next steps.


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img14.png' width='450' />


By using the tabs on the left, we can access the data.
Click on the microscope icon to reveal more information about the video
(DataTable tab) and used settings (Log tab). Next to this, the ID in the left
frame shows from which experiment number the data in this set originates.
The DataTable tab shows the slice numbers and the corresponding relative time
when the slide was recorded.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img15.png' width='450' />

Next, open the molecule tab. This tab reveals a table with all molecules found.
Each molecule is assigned an unique identifier (UID) by which the molecule can
be easily traced back after analysis. Click on an UID to see the corresponding
data in the adjacent DataTable. This table shows the coordinates and intensity
corresponding to the tracked peak over different slides.
Note that each time you run peak tracker, a new UID will be assigned to the tracked molecule.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img16.png' width='450' />

### 5. Plotting traces
Next to the automatically opened 'DataTable' tab, there is a tab called 'Plot'. Switch to the Plot tab now to plot the data points.

In the top left corner, click on the plotting icon. This opens a menu in which
the settings shown can be provided. In this case, the settings shown make
a line plot of the y position versus the slide number. In this dataset that corresponds with the direction of transcription over the DNA. Press the refresh button in the right corner of the menu to show the graph.


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img17.png' width='450' />

Now a plot is obtained showing the vertical displacement of the peak versus the slice number of the experiment: it shows the position of this single T7 polymerase on the DNA molecule.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img18.png' width='250' />

Move through the UIDs on the left to show the plot for every molecule. As you
will see some molecules show a trace in which the y position almost
linearly changes with the slice number. These are the molecules that are
relevant for this dataset since they correspond to a T7 polymerase that
transcribes the DNA with a uniform speed. Below is an example of such a molecule.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img19.png' width='250' />

### 6. Tagging data

When interesting molecules in the data have been found, one can easily Tag them
for later reference. For now we will use the tag name 'Active' since we want to filter for molecules that show active T7 polymerase activity.
Write 'Active' in the Tag window on the right and press enter. If you did this correctly, a grey marked tag will show up as shown in the screenshot below. Move through
all UIDs and select all molecules fulfilling the criteria in this way.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img20.png' width='450' />

After you went through the entire dataset, go back to the desktop page of the GUI. If you refresh the 'tag plot' you can now clearly see how many molecules with a certain tag are in your dataset. In our case: 11

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img22.png' width='450' />

### 7. Saving the MoleculeArchive

The archive can be safed as a .yama file by selecting the save option. Now the data can be further analysed and plotted using Python. If you are interested in that, please continue with the tutorial on MoleculeArchive data handling in Python.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img21.png' width='200' />
