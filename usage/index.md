---
layout: page
title: Usage
menu: usage
permalink: /usage/index.html
---
Once you have installed Mars in your Fiji using the update site (see below for instructions), the submenu "MoleculeArchive Suite" will show up under the Plugins containing the Mars [commands](../docs). Usually in the bottom section. In this submenu are a series of commands for general processing of single-molecule data. There are tools for both the analysis of fluorescence data as well as particle finding, fitting, and tracking. From there a range of further Commands allow for fitting, filtering, sorting, classifying with

### How to install this project in your local Fiji
An imagej update site has been created to help with maintanance and distribution of MARS and all necessary dependencies. Follow the directions below to install MARS in your local copy of Fiji:
1. If you haven't already, download and install Fiji from https://imagej.net/Fiji/Downloads.
1. Open Fiji and make sure you are up-to-date by running Help>Update. Click accept changes to update to the newest versions of all components. After the update, restart Fiji.
1. Run Help>Update a second time, but now click manage update sites. Then click Add update site to create a new entry. For Name put MARS and for URL put http://sites.imagej.net/Mars/ and then check the box to activate the update site. Now mars-core and mars-swing (the gui for Tables, Plots and MoleculeArchives) files should show up as well as the necessary dependencies. Install them all and restart Fiji. If you don't see the new jars restart the Updater.
1. If the plugins have been installed correctly, the submenu "MoleculeArchive Suite" should show up under Plugins.
1. From now on all you need to do is run the updater to ensure you have the current version of MARS installed. Please update frequently to ensure you benefit from the most recent bug fixes.

[Back to reference](#reference)
