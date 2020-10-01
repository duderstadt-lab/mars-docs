---
layout: page
title: How to Install Mars
menu: Install
permalink: /install/index.html
---
Once you have installed Mars in your Fiji using the update site (see below for instructions), the submenu "MoleculeArchive Suite" will show up under the Plugins containing the Mars [commands](../docs). Usually in the bottom section. In this submenu are a series of commands for general processing of single-molecule data. There are tools for both the analysis of fluorescence data as well as particle finding, fitting, and tracking. From there a range of other commands allow for filtering, sorting, and classification as outlined in the [documentation](../docs) section.

### How to install this project in your local Fiji
An imagej update site has been created to help with maintenance and distribution of Mars and all necessary dependencies. Follow the directions below to install Mars in your local copy of Fiji:
1. If you haven't already, download and install Fiji from https://imagej.net/Fiji/Downloads.
1. Open Fiji and make sure you are up-to-date by running Help>Update. Click accept changes to update to the newest versions of all components. After the update, restart Fiji.
1. Make sure javafx is installed in your Fiji. Javafx was bundled with past versions of Fiji, but the newest version of Fiji for mac does not have javafx installed. Your java version can be found inside the Fiji folder (using show package contents on mac) in the java folder. If javafx is not installed you will need to replace your java folder with one that contains javafx in order for the mars gui to work. See [this](https://forum.image.sc/t/can-not-use-javafx-on-fiji-at-openjdk/27213/10) post for further details. We hope javafx will again be bundled in Fiji soon.
1. Run Help>Update a second time, but now click manage update sites. Then click Add update site to create a new entry. For Name put Mars and for URL put http://sites.imagej.net/Mars/ and then check the box to activate the update site. Now mars-core and mars-fs (the gui for Tables, Plots and MoleculeArchives) files should show up as well as the necessary dependencies (there a lot of dependencies for mars-fx related to the markdown editor and charting library). Install them all and restart Fiji. If you don't see the new jars restart the Updater.
1. If the plugins have been installed correctly, the submenu "Mars" should show up under Plugins.
1. From now on all you need to do is run the updater to ensure you have the current version of Mars installed. Please update frequently to ensure you benefit from the most recent bug fixes.

Installation of Fiji should only take 10s of seconds to minutes. Downloading the jars for mars from the update site is very fast and only requires a couple seconds. Mars has been extensively tested on mac and linux systems. However, we have not tested mars on windows.

### Don't be a stranger
If you encounter problems of any kind or have a usage question please create an issue on one of the mars repositories in Github or post your question on the [ImageJ forum](https://forum.image.sc). If you post in the forum please make sure to tag it with mars and add *@karlduderstadt* in your comment. We would love to hear back about your experience using mars.
