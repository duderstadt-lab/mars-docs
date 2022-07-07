---
layout: page
title: Example Gallery
permalink: /examples/index.html
---

*click the images to visit the corresponding example page*

#### Tracking synthesis by RNA polymerase as it moves along DNA

[<img align='center' src='{{site.baseurl}}/examples/img/index/img2.png' width='350' />](track-position-on-DNA)

Workflow for tracking and analyzing polymerase movement on DNA molecules. ([Scherr *et al.*, 2022, Cell Reports](https://doi.org/10.1016/j.celrep.2022.110531))

---
---

#### Static FRET

[<img align='center' src='{{site.baseurl}}/examples/img/index/img3.png' width='450' />](Static_FRET)

Workflow for processing static single-molecule FRET from short duplex DNA molecules with donor and acceptor fluorophores positioned at two fixed distances apart as described by [Hellenkamp *et al.*, 2018, Nat. Meth.](https://www.nature.com/articles/s41592-018-0085-0). To allow for benchmarking, publicly available [sample data](https://zenodo.org/record/1249497#.YMccli0RrGI) from Hellenkamp et al. were used and their ALEX analysis strategy was adapted into the Mars workflow.

---
---

#### Dynamic FRET

[<img align='center' src='{{site.baseurl}}/examples/img/index/dynamic_FRET_example.png' width='900' />](Dynamic_FRET)

Workflow for processing dynamic single-molecule FRET from a Holliday junction functionalized with donor and acceptor fluorophores positioned on different DNA arms following the design of [Hyeon *et al.*, 2012, Nat. Chem](https://www.nature.com/articles/nchem.1463). New [sample data](https://doi.org/10.5281/zenodo.6659531) were collected for this Mars workflow using an Alternating Laser EXcitation (ALEX) strategy.

There are two example workflows using the Holliday junction [sample data](https://doi.org/10.5281/zenodo.6659531). In the [first dynamic FRET example workflow](Dynamic_FRET), both the direct acceptor excitation (637, C=0) and FRET (532, C=1) information is used to generate fully corrected E and S distributions closely following the analysis protocol from [Hellenkamp *et al.*, 2018, Nat. Meth.](https://www.nature.com/articles/s41592-018-0085-0). In the [second dynamic FRET example workflow](No_aex_FRET), only the FRET (532, C=1) information is used to generate a corrected E distribution following the analysis protocol from [McCann _et al._](https://doi.org/10.1016/j.bpj.2010.04.063). The direct acceptor excitation information is integrated in the first steps of the [second dynamic FRET example workflow](No_aex_FRET) for visual inspection purposes only. Therefore, the [second dynamic FRET example workflow](No_aex_FRET) illustrates how to analyze FRET datasets containing no direct acceptor excitation information.

---
---

#### Flow Magnetic Tweezers

[<img align='center' src='{{site.baseurl}}/examples/img/index/img1.png' width='250' />](flow-Magnetic-Tweezers)

Workflow for tracking and processing the motion of paramagnetic beads attached to DNA molecules and manipulated by Flow Magnetic Tweezers. ([Agarwal & Duderstadt, 2020, Nat. Comm.](https://www.nature.com/articles/s41467-020-18456-y))

---
---


#### Short Script Examples

[1. Plot the Kinetic Change Point Rates with Python](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_scripts_and_notebooks/13_KCP_widget_and_jupyter_plot.ipynb)  
Jupyter notebook showing how to retrieve the segment tables in a Python environment and how to plot the calculated rates with seaborn as well as in the **Rover** scriptable widgets.

[2. Generate a Table of Parameter Values from the Archive](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_scripts_and_notebooks/09_Generate_a_table_of_parameter_values.groovy)  
Groovy script to make a MarsTable showing the value for the specified parameter for each molecule entry. Run the script and a pop-up dialog will be created in which the parameter of interest can be selected.

[3. Plot the Tracking Results as Color Coded Overlay in the Image](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_scripts_and_notebooks/04_Color_coded_tracks_overlay.groovy)  
Script to show the tracking results in the Molecule Archive as color coded ROIs on the original video. Also possible to show the overlay for tagged molecules only (see [repository example](https://github.com/duderstadt-lab/mars-tutorials/blob/master/Example_scripts_and_notebooks/05_Color_coded_tracks_overlay_tagged.groovy)).



More example scripts can be found in the git [repository](https://github.com/duderstadt-lab/mars-tutorials). Please visit the [Mars Java docs](https://duderstadt-lab.github.io/mars-core/javadoc/) for more information about all classes and individual commands.
