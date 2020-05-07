---
layout: workingwithmars
title: "Kinetic Changepoint Analysis"
permalink: /tutorials/workingwithmars/kcpa/index.html
---

#### Introduction
The algorithm which is used to find the kinetic change points is described in the following paper by ***[Flynn R. Hill, Antoine M. van Oijen, and   Karl E. Duderstadt]( https://doi.org/10.1063/1.5009387)***. Before the tutorial starts a short introduction to the topic is given. For a more in depth description how the algorithm works in detail please check the paper.

If the kinetic behaviour of a trace changes one calls this a kinetic change point. These changes can be hard to detect since they can be very small
and covered by noise. For example on a molecular scale thermal fluctuations are very pronounced which cloak the dynamical behaviour. Extreme changes can be still spotted but the small more subtle changes are hard to detect. The power of single molecule data lays in the detection of these small changes.

An idealized analysis would detect the small changes while not filtering or binning the data and only rely on a small number of user input. Filtering the data would degrade the qualitiy and could lead to loss of information. If the user only gives a small number of inputs the analysis is less biased and is not based on a predefined model. This would also make the analysis more flexible since it can be used for different scenarios. Furthermore, the analysis is more reproducible. The kinetic change point analysis described here and used in this tutorial combines these trades.

#### Assumptions
The trace should be piece-wise linear between the kinetic change points. It is assumed that the noise is Gaussian distributed. This hold true for biological systems observed with single molecule techniques. Assuming a specific kinetic model is not necessary since it would be specific for one type of behaviour.

##### Inputs
1. Position vs. time trajectories:

2. Likelihood ratio/False positive rate:

3. Noise level (if it is known):

#### How to: Run the kinetic change point
1. To run the kinetic change point analysis go under "Plugins-> MoleculeArchive Suite-> KCP-> Change Point Finder" (also shown in the image).
<img src='{{site.baseurl}}/tutorials/img/kcpa/img1.png' width='300' />

2. The window shown in the image will open.
<img src='{{site.baseurl}}/tutorials/img/kcpa/img2.png' width='250' />

Select the correct MoleculeArchive. Select the x and y. The "X Column" is the time in this case it is called slice. The "Y Column" is the position data which will be analysed. The "Confidence value" bascially means that a false positive rate of 1 percent is accepted. The "Global Sigma" will be used if no "Background region" is selected. The "Background region" can be used to specify a region in the trace which will be used to determine the background. In this case it is not used. The whole trace is analysed so "Analyze region" is not ticked. Only the molecules with the tag "Active" are analysed ("Tagged with" is ticked).

#### How to: Plot the result of the kinetic change point analysis
The result from the analysis can be plotted. Go to the menu of the plot and select "Segments". After refreshing the plot straight lines which connect the kinetic change points will appear (the color can be adjusted by changing the "Segment color").
<img src='{{site.baseurl}}/tutorials/img/kcpa/img3.png' width='250' />

#### How to: Read the results of the kinetic change point analysis
After visualising the results one can also check the actual values of the analysis.
<img src='{{site.baseurl}}/tutorials/img/kcpa/img4.png' width='250' />
Open "y vs slice". A tabel with different columns will appear. "x1" gives the start time and "x2" the end time point of a region where the trace is piece-wise linear. "y1" and "y2" give the corresponding position start and end points. The linear fit between these to points can be described with a line equation (y = Bx + A). The "B" value is the slope of the line which also corresponds to the rate. "A" represents the intercept with the y axis. 
