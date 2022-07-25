---
layout: import
title: Single-molecule dataset (SMD)
permalink: /docs/import/smd/index.html
---

The groups of [Dan Herschlag](http://herschlaglab.stanford.edu) and [Ruben Gonzalez](http://www.columbia.edu/cu/chemistry/groups/gonzalez/index.html) developed the Single-molecule Dataset (SMD) format to facilitate publication and exchange of time-series data and analysis results from single-molecule FRET studies. This repository contains a Fiji/ImageJ2 command that converts json text files in [Single-molecule Dataset (SMD)](https://smdata.github.io) format to Mars Molecule Archives.

A full explanation of the SMD file specification can be found in the publication.

Greenfeld, M., van de Meent, JW., Pavlichin, D.S. et al. Single-molecule dataset (SMD): a generalized storage format for raw and processed single-molecule data. BMC Bioinformatics 16, 3 (2015). [https://doi.org/10.1186/s12859-014-0429-4](https://doi.org/10.1186/s12859-014-0429-4)

Source code for working with SMD files in Matlab and Python can be found [here](https://github.com/smdata).

## Usage

The SMD import command can be installed in Fiji simply by activating the [Mars update site](https://duderstadt-lab.github.io/mars-docs/install/). When Mars is installed and Fiji has been restarted, the Mars submenu should appear at the bottom of the Plugins menu. The SMD import command can be found under Mars>Import>Single-molecule Dataset (SMD). When started a file chooser will appear for selection of the plain-text json file in SMD format to be imported. When the conversion process is done, a new Molecule Archive window should appear with the converted dataset.

If you encounter problems, first confirm the example.json file in this repository is successfully opened. If not, you might have a general configuration issue with Mars. Please get in touch with us by creating a post on the [Scientific Image Forum](https://forum.image.sc/) tagged with mars and/or with @karlduderstadt mentioned.

This command was developed using the example.json file provided. We would be happy to update the command if the specification is somehow different in your files.

## Groovy SMD import script

For quick editing of the import procedure, a scripts folder with an ImportSMD.groovy script is included in this repository. This script can be used in the Fiji script editor to perform the same import procedure. The script is included to provide the possibility to modify the import fields or structure. The script requires the Mars jars to be installed from the update site.

The source code for this importer can be found in the [mars-smd](https://github.com/duderstadt-lab/mars-smd) repository.
