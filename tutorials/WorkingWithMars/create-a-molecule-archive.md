---
layout: workingwithmars
title: "Let's Make A MoleculeArchive - Tutorial"
permalink: /tutorials/workingwithmars/create-a-Molecule-Archive/index.html
---

_level: beginner, duration: 5-10 min_

In this tutorial we will go through the basics of loading a video, finding
and tracking peaks, navigating through **Mars Rover**, looking at the traces
corresponding to the tracked peaks, tagging them and saving the result as a
**MoleculeArchive** (.yama) file. In the example video that is used the transcription movement of fluorescently labeled polymerase over DNA is investigated by means of TIRF microscopy. The DNA itself is not labeled and therefore is not visible in the video. This
single-molecule dataset provides an excellent way to familiarise yourself with
**Mars** and the most important plugins.

### 1. Import a video in Fiji
Open the example dataset ['TestVideo.tif'](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Tutorial_files/TestVideo.tif) in Fiji. This will open the video in a new screen. Use the slider below the video to go through all recorded frames.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img1.png' width='450' />
<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img2.png' width='450' />


__Background Information:__  
When playing the movie, it is clear that molecules with different behaviour
are observed. The different types are listed here:
1. Fast moving molecules only present in a few subsequent frames: these are
molecules that are not associated with a DNA molecule and float in the buffer.
Note that the buffer flow direction in this experiment is upwards whereas the transcription direction is downwards. This ensures fast flow events can be easily excluded from analysis.
2. Moving molecules that travel downward slowly over a large amount of frames:
these are transcribing polymerases.
3. Disappearing molecules towards the end of the movie: fluorophores that are
photobleached over time.


### 2. Use peak tracker to track peaks through frames
Now, open the "Peak Tracker" tool from **Mars** as shown and enter all settings. Providing the correct settings makes sure that the tracker can find the peaks correctly. For more information about these parameters visit the [documentation page](https://duderstadt-lab.github.io/mars-docs/docs/image/PeakTracker/).


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img10.jpg' width='450' />


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img11.png' width='250' />  
<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img12.png' width='250' />


Press OK to run the tracking plugin.


### 3. Navigating through Mars Rover

The GUI automatically opens on the desktop page. In this overview page one can
see information on the dataset, and get some statistical data. More statistical
data will become available when the data is analysed in the next steps.


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img14.png' width='450' />


By using the tabs on the left, the data can be accessed.
Click on the microscope icon to reveal more information about the video
(DataTable tab) and used settings (Log tab). Next to this, the ID in the left
frame shows from which experiment number the data in this set originates. This number is unique for each measurement to make sure filtered data can always be traced back to the correct experiment, even when data from experiments are merged. The DataTable tab shows the slice numbers and the corresponding relative time when the slide was recorded.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img15.png' width='450' />

Next, open the molecule tab. This tab reveals a table with all molecules found.
Each molecule is assigned an unique identifier (UID) by which the molecule can
be easily traced back after analysis. Click on an UID to see the corresponding
data in the adjacent "DataTable". This table shows the coordinates and intensity
corresponding to the tracked peak over different slides.
Note that each time "Peak Tracker" is run, a new UID will be assigned to the tracked molecule.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img16.png' width='450' />


### 4. Plotting traces
Next to the automatically opened 'DataTable' tab, there is a tab called 'Plot'. Switch to the Plot tab now to plot the data points.

In the top left corner, click on the plotting icon. This opens a menu that can be used to modify the plot. In this case, the settings shown make
a line plot of the y position versus the slice number. In this dataset that corresponds with the direction of transcription over the DNA. Press the refresh button in the right corner of the menu to show the graph.


<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img17.png' width='450' />

Now a plot is obtained showing the vertical displacement of the peak versus the slice number of the experiment: it shows the position of this single polymerase on the DNA molecule.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img18.png' width='250' />

Move through the UIDs on the left to show the plot for every molecule. As will become apparent, some molecules show a trace in which the y position almost
linearly changes with the slice number. These are the molecules that are
relevant for this dataset since they correspond to a polymerase that
transcribes the DNA with a uniform speed. Below is an example of such a molecule.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img19.png' width='250' />

### 5. Tagging data

When interesting molecules in the data have been found, one can easily tag them
for later reference. For now the tag name "Active" is used to filter for polymerase active molecules in further analysis. Write 'Active' in the Tag window on the right and press enter. If this is done correctly, a grey marked tag will show up as shown in the screenshot below. Move through all UIDs and select all molecules fulfilling the criteria in this way.

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img20.png' width='450' />

After going through the entire dataset, go back to the desktop page of **Mars Rover**. After refreshing the "tag plot" the amount of molecules per tag is displayed. In our case: 11

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img22.png' width='450' />

### 6. Saving the MoleculeArchive

The archive can be safed as a .yama file by selecting the save option. Now the data can be further analysed and plotted using Python. If you are interested in that, please continue with the [tutorial on MoleculeArchive data handling in Python](https://duderstadt-lab.github.io/mars-docs/tutorials/marsto/open-a-Molecule-Archive-in-Python/)

<img align='center' src='{{site.baseurl}}/tutorials/img/TCreateMoleculeArchive/img21.png' width='200' />


For more detailed information about all functions in **Mars Rover** please visit the [documentation page.](https://duderstadt-lab.github.io/mars-docs/docs/)
