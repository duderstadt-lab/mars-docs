---
layout: home
menu: home
title: Molecule Archive Suite
---

{:.lead}
**Mars** - **M**olecule **ar**chive **s**uite is broad framework for storage and reproducible processing of single-molecule observations.

Mars provides a collection of ImageJ2 commands to find, fit, track and characterize single molecules. The algorithms provided are routinely used to analyze data arising from multicolor single molecule TIRF and DNA flow stretching experiments. Primary image analysis commands generate MoleculeArchives which contain collections of individual molecules and image metadata records. MoleculeArchives have a simple, yet powerful, API for fast and concise access to subsets of records based on arbitrary properties. During creation, all records are assigned universally unique IDs, which serve as the primary keys for retrieval and storage. This architecture allows for seamless virtual storage, merging, and multithreaded processing of very large datasets. Moreover, this framework allows for the same data structure to be used all the way from initial processing to the generation of final plots either in Fiji or in Jupyter notebooks using Python.

Mars, a molecule archive suite for reproducible analysis and reporting of single molecule properties from bioimages
Nadia M Huisjes, Thomas M Retzer, Matthias J Scherr, Rohit Agarwal, Barbara Safaric, Anita Minnen, Karl E Duderstadt
bioRxiv 2021.11.26.470105; doi: [https://doi.org/10.1101/2021.11.26.470105](https://doi.org/10.1101/2021.11.26.470105)

To get started with Mars, take a look at the [tutorials](tutorials), [example gallery](examples), and [how to install guide](install), or read [about the project's goals](about). Comprehensive reference material can be found in the [documentation](docs) section.

<div style="text-align: center"><img src='{{site.baseurl}}/assets/Mars_gif.gif' width="650"/></div>
