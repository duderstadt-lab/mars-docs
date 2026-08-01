---
layout: page
title: How to Install Mars
menu: Install
permalink: /install/index.html
---

### 1. Install Fiji on your computer
Mars consists of a collection of scijava commands and user interface that runs in [Fiji](https://imagej.net/Fiji). Therefore an updated version of Fiji is required to run Mars on your computer.

1. Download and install **Fiji Latest** from [Fiji Downloads](https://imagej.net/Fiji/Downloads). The recommended **Mars-Latest** update site below requires Fiji Latest and will not work with the older, stable Fiji build.
2. Open Fiji and make sure you are up-to-date by running Help>Update. Click accept changes to update to the newest versions of all components. After the update, restart Fiji.

### 2. Install the Mars project in your local Fiji
Add Mars from the Fiji update site:
1. Run Help>Update in the Fiji menu and click 'manage update sites'. Then check the box for the **Mars-Latest** update site. Close the menu and install the required packages. Restart Fiji. If the plugins have been installed correctly, the submenu "Mars" should show up under Plugins.
2. From now on all you need to do is run the updater to ensure you have the current version of Mars installed. Please update frequently to ensure you benefit from the most recent bug fixes.

There are two Mars update sites available, and which one you want depends on which Fiji you're running:
- **Mars-Latest** (recommended) — works only with **Fiji Latest**, and is the version that continues to receive updates.
- **Mars** — works with the older, stable Fiji build (Java 8), but is no longer receiving updates.

Once you have installed Mars in your Fiji using the update site, the submenu "Mars" will show up under the Plugins containing the Mars [commands](../docs). Usually in the bottom section. In this submenu are a series of commands for general processing of single-molecule data. There are tools for both the analysis of fluorescence data as well as particle finding, fitting, and tracking. From there a range of other commands allow for filtering, sorting, and classification as outlined in the [documentation](../docs) section.  


### Don't be a stranger
If you encounter problems of any kind or have a usage question please create an issue on one of the mars repositories in Github or post your question on the [ImageJ forum](https://forum.image.sc/tag/mars). If you post in the forum please make sure to tag it with mars and add *@karlduderstadt* in your comment. We would love to hear back about your experience using mars.

### System compatibility
Mars is compatible with Windows, Linux and Mac OS. Mars has been extensively tested on mac and linux systems. However, we have not tested Mars as extensively on windows.

<img align='center' src='{{site.baseurl}}/usage/img/img1.png' width='50' />
<img align='center' src='{{site.baseurl}}/usage/img/img2.png' width='50' />
<img align='center' src='{{site.baseurl}}/usage/img/img3.png' width='50' />
