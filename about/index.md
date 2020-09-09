---
layout: about
title: About the Mars Project
permalink: /about/index.html
---

### For Reproducible Analysis of Single-Molecule Observations go to Mars

**The Reproducibility Crisis**  
Reproducible analysis of bioimaging data is a major challenge slowing scientific progress. The increasing size of acquired datasets and the heterogeneity of data storage formats impedes sharing and re-analysis of data.<sup>1</sup> This challenge has gained wide recognition leading to the development of standardised data and metadata storage formats such as the ['Open Microscopy Environment (OME)'](https://duderstadt-lab.github.io/mars-docs/docs/OME/datarequirements/).<sup>2</sup>
However, few standards exist for the reproducible analysis and re-use of image-derived properties themselves. Often non-transparent, non-traceable data filtering and data point rejection procedures are applied to go from observations to conclusions. This inhibits data re-evaluation leading to low reproducibility of the data analysis part of a study. Robust storage formats that allow for simple, transparent and convenient analysis, classification, and storage of image-derived data are needed to solve this reproducibility crisis.

**The Solution: Mars**  
Recognising the reproducibility crisis in the field of single-molecule imaging, we developed **M**olecule **Ar**chive **S**uite (Mars): an open-source platform for storage of image-derived properties of biomolecules. Mars provides a collection of ImageJ2<sup>3</sup> commands for processing images and image-derived properties based on a new Molecule Archive data storage architecture. Originally developed to overcome performance and reproducibility problems with very large datasets (10-100 GBs) derived from single-particle tracking <sup>4</sup>, MoleculeArchives store data as collections of individual biomolecule properties and image metadata records (_figure 1_). During creation, all records are assigned universally unique IDs (UUIDs) which serve as the primary keys for retrieval and storage. This framework allows for seamless merging of datasets as well as multithreaded processing of virtually stored archives.
A simple, yet powerful user interface built on this architecture (Mars Rover, _figure2_) allows for a fully trackable, precise, and fast data analysis process based on arbitrary properties. These properties can be calculated using Mars commands (f.e. kinetic change points, variance, intensity), using the advanced plotting options within Mars Rover (f.e. distance travelled, speed), or using customised scripting. UUIDs ensure the history of each record remains traceable through long and complex analysis pipelines involving numerous data filtering and merging steps, and at the same time an automatically generated analysis log keeps track of all executed calculations and filter criteria applied.
The implementation of Mars in single-molecule studies allows for an easy, fast and above all reproducible analysis of image-derived properties and will lead to clear and well-substantiated conclusions from single-molecule observations.


<img align='center' src='{{site.baseurl}}/about/img/img2.png' width='250' />  
_Figure 1: Schematic overview of the Molecule Archive architecture comprising **1.** individual molecule records (grey cards) containing information such as molecule UUID, metadata UID, tracking coordinates, tags, notes, advanced plots and properties like intensity and bleaching time & **2.** metadata records (blue card) containing experiment specific information like experiment dimensionality, frame rate, drift, microscope properties, tags, notes, advanced plots etc._

<img align='center' src='{{site.baseurl}}/about/img/img3.png' width='650' />  
_Figure 2: Screenshot of the Mars Rover user interface showing **A.** an individual molecule record showing tracking results, the molecule UUID, metadata UID, tags, and notes; **B.** a metadata record showing the metadata UID, microscope properties and experiment dimensionality; and **C.** the Rover Dashboard with scriptable widgets that can be customised fully according to the needs of the user._


### Start using Mars  
Please visit the [install](https://duderstadt-lab.github.io/mars-docs/install/) section to get Mars running on your machine and start analysing your data in a clear, reproducible manner. Visit the [tutorials](https://duderstadt-lab.github.io/mars-docs/tutorials/) section for a head start and go through the extensive [documentation pages](https://duderstadt-lab.github.io/mars-docs/docs/) for a more detailed description of each Mars command.


**References**  
<sup>1</sup> Marqués, G., Pengo, T., Sanders, M.A. (2020), "Science Forum: Imaging methods are vastly underreported in biomedical research", _eLife_ **9**: e55133  
<sup>2</sup> Goldberg, IG., Allan, C., Burel, J-M. et al. (2005), "The Open Microscopy Environment (OME) Data Model and XML file: open tools for informatics and quantitative analysis in biological imaging", _Genome Biology_ **6**: R47  
<sup>3</sup> Rueden, CT.; Schindelin, J. & Hiner, MC. et al. (2017), "ImageJ2: ImageJ for the next generation of scientific image data", _BMC Bioinformatics_ **18**: 529, PMID 29187165  
<sup>4</sup> Agarwal et al. (2020), in revision.
