---
layout: roi
title: Open ROIs
permalink: /docs/roi/OpenROIs/index.html
---
This command brings up a GUI for quickly filtering through lots of small ROI videos. In the GUI, small videos are tiled and you can select if you want to keep or reject a video. As you look through and filter data the command can move the selected and rejected videos into different subdirectories (/accepted and /rejected). This command is typically used in combination with the [Save ROIs](SaveROIs) command. The [Save ROIs](SaveROIs) command should generate a bunch of small ROI videos of equal size and frame number, which allows them to be tiled nicely.

#### Inputs

<img align='center' src='{{site.baseurl}}/docs/roi/img/Open ROIs Dialog.png' width='500' />

* *Choose input directory* - A directory containing a collection of small tif ROI videos.
* *Vertical Tiles* - Number of videos to load vertically in the window.
* *Horizontal Tiles* - Number of videos to load horizontally in the window.

#### ROI Manual Filter ROI

<img align='center' src='{{site.baseurl}}/docs/roi/img/Open ROIs Tiled Window.png' width='700' />

The individual videos are tiled for manual sorting. By default all are rejected. To accept a video just click and the box will turn green. Once you have finished with the current set press the Next Button to see the next set, if you need to go back and check your choices use the Previous Button. Once you are sure about your selections you can use the Process button to move all sorted files into either an accepted or rejected subdirectory (This will Process all past ROI sets and the currently open one). Process can be used at any time as you progress through the ROIs, but once you hit Process button you will no longer be able to go back and change your selections, because the ROI video will have been moved into the subdirectories.

The Start/Stop Button is used to start and stop the video with a text box indicating the number of frames to skip during play back. This is helpful for quickly sorting through lost of videos based on visual properties - mobility etc. So to use the Start/Stop buttons you should first set the animation frame rate very high. For this go under Image>Stacks>Animation>Animation Options... and for Speed select 1000 fps (for example). This means if you have a 5000 frame bead tracking video, it will play all the way through in 5 seconds. The frame skipping will ensure the playback is smooth.

Alternatively you can just play the videos with the arrow button but in this case all the frame will be played back, so this is only ideal for slow frame rates. In either case you can adjust the playback speed using the Animation Options.

Finally the labels at the bottom tell you the range of ROI you have open and the total. Also, the file name for each ROI appears upon mouseover. This could be useful if you want quickly look back at a MoleculeArchive you might have used as the basis for generating the ROI videos using [Build ROIs from MoleculeArchive ](BuildROIsFromMoleculeArchive) in [[AnalysisProtocolID00002]].

Finally, the tilted window functions just like any other Image in ImageJ. Therefore, you can adjust brightness/contrast and other features if needed. The coloured boxes should remain unchanged since they are an overlay.

#### Output

* */accepted and /rejected sub-directory sorted videos* - Every time the Process button is pressed, videos are filtered into the subfolders based on the manual selections.

#### Groovy scripting

This is a GUI based tool and requires lots of user input, so there is no way to run it as a groovy script. However, the analysis pipeline [[AnalysisProtocolID00002]] demonstrates how to use this tool and a groovy script to manually filter ROI videos based on molecules in a MoleculeArchive and then add accepted and rejected tags back to the archive at the end.
