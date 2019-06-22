**MARS** - **M**olecule **AR**chive **S**uite is a collection of ImageJ2 commands for single-molecule analysis based on a cored data model of molecule objects for optimal storage and multithreaded processing of large single-molecule time-series datasets.

This is just a rough outline of what's to come. mars-docs is currently being written, please check back in the next days and weeks for frequent updates.

### How to install this project in your local Fiji
An imagej update site has been created to help with maintanance and distribution of MARS and all necessary dependencies. Follow the directions below to install MARS in your local copy of Fiji:
1. If you haven't already, download and install Fiji from https://imagej.net/Fiji/Downloads.
1. Open Fiji and make sure you are up-to-date by running Help>Update. Click accept changes to update to the newest versions of all components. After the update, restart Fiji.
1. Run Help>Update a second time, but now click manage update sites. Then click Add update site to create a new entry. For Name put MARS and for URL put http://sites.imagej.net/Mars/ and then check the box to activate the update site. Now mars-core and mars-swing (the gui for Tables, Plots and MoleculeArchives) files should show up as well as the necessary dependencies. Install them all and restart Fiji. If you don't see the new jars restart the Updater.
1. If the plugins have been installed correctly, the submenu "MoleculeArchive Suite" should show up under Plugins.
1. From now on all you need to do is run the updater to ensure you have the current version of MARS installed. Please update frequently to ensure you benefit from the most recent bug fixes.

### Commands
Once you have installed MARS in your Fiji using the update site, the submenu "MoleculeArchive Suite" will show up under the Plugins menu. Usually in the bottom section. In this submenu are a series of commands for general processing of single-molecule data. There are tools for both the analysis of fluorescence data as well as particle findering, fitting, and tracking. From there a range of other commands allow for fitting, filtering, sorting, and manual classification.

Here is a complete list of commands available in the MoleculeArchive Suite submenu (further documentation coming soon...):

#### Molecule Utils
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

#### Table Utils
* Open ResultsTable
* Drag & Drop Window
* Results Sorter
* Results Filter
* Histogram Builder

#### Image Processing
* Peak Finder
* Peak Tracker
* Beam Profile Corrector
* Integrate Molecules
* Discoidal Averaging Filter
* Gradient Calculator

#### ROI Tools
* Open ROIs
* Save ROIs
* Transformation Parameter Calculator
* Transform ROIs
* Build ROIs from MoleculeArchive

#### KCP
* Change Point Finder
* Segment Distribution Builder
* Sigma Calculator

### API
The core API of MARS is based around scijava results tables and MoleculeArchives housing Molecule records. So the follow classes make-up the core API:
* MARSResultsTable
* MoleculeArchive
* Molecule
   

