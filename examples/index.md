---
layout: page
title: Example Gallery
permalink: /examples/index.html
---

*click the images to visit the corresponding example page*

#### Tracking of a Protein on DNA

[<img align='center' src='{{site.baseurl}}/examples/img/index/img2.png' width='350' />](track-position-on-DNA)

Complete workflow for the tracking and processing of polymerase movement on a stretched DNA molecule. ([Scher, Wahab, Remus & Duderstadt, 2021, under consideration at Cell Press](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3775178))


---
---

#### FRET Analysis

[<img align='center' src='{{site.baseurl}}/examples/img/index/img3.png' width='450' />](FRET)

Complete workflow for the analysis and processing of a FRET dataset containing short DNA molecules with a covalently attached donor and acceptor fluorophore. Benchmarked against openly available standardization data as published by [Hellenkamp *et al.*, 2018, Nat. Meth.](https://www.nature.com/articles/s41592-018-0085-0)

---
---

#### Flow Magnetic Tweezers

[<img align='center' src='{{site.baseurl}}/examples/img/index/img1.png' width='250' />](flow-Magnetic-Tweezers)


Complete workflow for tracking and processing the motion of magnetic beads tethered to individual DNA molecules under flow. ([Agarwal & Duderstadt, 2020, Nat. Comm.](https://www.nature.com/articles/s41467-020-18456-y))

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
