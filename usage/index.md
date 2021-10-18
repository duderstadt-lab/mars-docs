---
layout: page
title: How to Install Mars
menu: Install
permalink: /install/index.html
---

### 1. Install Fiji on your computer and check the presence of javafx
Mars consists of a collection of scijava commands and user interface that runs in [Fiji](https://imagej.net/Fiji). Therefore an updated version of Fiji is required to run Mars on your computer.

1. Download and install [Fiji](https://imagej.net/Fiji/Downloads).
2. Open Fiji and make sure you are up-to-date by running Help>Update. Click accept changes to update to the newest versions of all components. After the update, restart Fiji.


3. **Mac:** Verify that javafx is installed in your Fiji. To check this, show the package contents of the Fiji folder by right-mouse clicking on the Fiji folder and selecting 'Show Package Contents'. Check inside the java folder and macosx subfolder. If you see 'adoptopenjdk-8.jdk' in the folder then you do not have javafx installed. In this case you will need to replace this jdk file with a 'zulu_8.jre' for the Mars interface to work. You can download the jre file [here](https://github.com/duderstadt-lab/mars-tutorials/blob/bf1f8eb908ad9d29f94f5a2503a50e00f6c9ec6c/macos_jre/zulu_8.jre.zip). Unzip the downloaded file and replace 'adoptopenjdk-8.jdk' with 'zulu_8.jre'. Re-open Fiji after making this change.

### 2. Install the Mars project in your local Fiji
Add Mars from the Fiji update site:
1. Run Help>Update in the Fiji menu and click 'manage update sites'. Then check the box for the Mars update site. Close the menu and install the required packages. Restart Fiji. If the plugins have been installed correctly, the submenu "Mars" should show up under Plugins.  
<img align='center' src='{{site.baseurl}}/usage/img/img4.png' width='350' />
2. From now on all you need to do is run the updater to ensure you have the current version of Mars installed. Please update frequently to ensure you benefit from the most recent bug fixes.


Once you have installed Mars in your Fiji using the update site, the submenu "Mars" will show up under the Plugins containing the Mars [commands](../docs). Usually in the bottom section. In this submenu are a series of commands for general processing of single-molecule data. There are tools for both the analysis of fluorescence data as well as particle finding, fitting, and tracking. From there a range of other commands allow for filtering, sorting, and classification as outlined in the [documentation](../docs) section.  


### Don't be a stranger
If you encounter problems of any kind or have a usage question please create an issue on one of the mars repositories in Github or post your question on the [ImageJ forum](https://forum.image.sc/tag/mars). If you post in the forum please make sure to tag it with mars and add *@karlduderstadt* in your comment. We would love to hear back about your experience using mars.

### System compatibility
Mars is compatible with Windows, Linux and Mac OS. Mars has been extensively tested on mac and linux systems. However, we have not tested Mars as extensively on windows.

<img align='center' src='{{site.baseurl}}/usage/img/img1.png' width='50' />
<img align='center' src='{{site.baseurl}}/usage/img/img2.png' width='50' />
<img align='center' src='{{site.baseurl}}/usage/img/img3.png' width='50' />
