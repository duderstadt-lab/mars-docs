## **MARS** - **M**olecule **AR**chive **S**uite - docs

**MARS** - **M**olecule **AR**chive **S**uite is a collection of ImageJ2 commands for single-molecule analysis based on a cored data model of molecule objects for optimal storage and multithreaded processing of large single-molecule time-series datasets.

This is just a rough outline of what's to come. mars-docs is currently being written, please check back in the next days and weeks for frequent updates.

# How to install this project in your local Fiji
An imagej update site has been created to help with maintanance and distribution of MARS and all necessary dependencies. Follow the directions below to install MARS in your local copy of Fiji:
1. If you haven't already, download and install Fiji from https://imagej.net/Fiji/Downloads.
2. Open Fiji and make sure you are up-to-date by running Help>Update. Click accept changes to update to the newest versions of all components. After the update, restart Fiji.
3. Run Help>Update a second time, but now click manage update sites. Then click Add update site to create a new entry. For Name put MARS and for URL put http://sites.imagej.net/Mars/ and then check the box to activate the update site. Now mars-core and mars-swing (the gui for Tables, Plots and MoleculeArchives) files should show up as well as the necessary dependencies. Install them all and restart Fiji. If you don't see the new jars restart the Updater.
4. If the plugins have been installed correctly, the submenu "MoleculeArchive Suite" should show up under Plugins.
5. From now on all you need to do is run the updater to ensure you have the current version of MARS installed. Please update frequently to ensure you benefit from the most recent bug fixes.

# Commands
Once you have installed MARS in your Fiji using the update site, the submenu "MoleculeArchive Suite" will show up under the Plugins menu. Usually in the bottom section. In this submenu are a series of commands for general processing of single-molecule data. There are tools for both the analysis of fluorescence data as well as particle findering, fitting, and fracking. From there a range of futher Commands allow for fitting, filtering, sorting, classifying.

Here is a complete list of commands available in MoleculeArchive Suite submenu with corresponding links to documentation. There are individual pages for each tool to make it easy to comment on a specific tool and keep all the documentation organized:

# Molecule Utils
   * Open MoleculeArchive
   * Build Archive From Table
   * Region Difference Calculator
   * Generate bps
   * Add Time
   * Drift Calculator
   * Drift Corrector
   * MSDCalculatorCommand
   * Interpolate Missing Points (x,y)
   * Merge Archives
# Table Utils
   * Open ResultsTable
   * Drag & Drop Window
   * Results Sorter
   * Results Filter
   * Histogram Builder
# Image Processing
   * Peak Finder
   * Peak Tracker
   * Beam Profile Corrector
   * Integrate Molecules
   * Discoidal Averaging Filter
   * Gradient Calculator
# ROI Tools
   * Open ROIs
   * Save ROIs
   * Transformation Parameter Calculator
   * Transform ROIs
   * Build ROIs from MoleculeArchive
# KCP
   * Change Point Finder
   * Segment Distribution Builder
   * Sigma Calculator

# API
   * MARSResultsTable
   * MoleculeArchive
   * Molecule
   
are among come of the classes that makeup the core API of MARS.

# Copyright and license information
Copyright (C) 2019, Karl Duderstadt
All rights reserved.
 
Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:
 
1. Redistributions of source code must retain the above copyright notice,
   this list of conditions and the following disclaimer.
 
2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.
 
3. Neither the name of the copyright holder nor the names of its contributors
   may be used to endorse or promote products derived from this software
   without specific prior written permission.
 
THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.
