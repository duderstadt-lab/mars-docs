---
layout: about
title: Repositories
permalink: /about/Repositories/index.html
---

All repositories can be found on the [github page](https://github.com/duderstadt-lab). Current and complete dependency information can be found in the pom.xml within each repository. The main dependencies are highlighted below. All projects inherit dependencies from the [SciJava POM](https://github.com/scijava/pom-scijava).

| Repository     | Function     | Dependencies    |
| :------------- | :------------- | :---------------|
| [mars-core](https://github.com/duderstadt-lab/mars-core)       | Primary repository for **Mars** containing core data storage modules, image processing algorithms, and SciJava commands.       |  [JacksonJson](https://github.com/FasterXML/jackson), [SCIFIO](https://scif.io), [Apache Commons](https://commons.apache.org), [imglib2](https://github.com/imglib/imglib2), [SciJava Common](https://github.com/scijava/scijava-common), [SciJava Table](https://github.com/scijava/scijava-table)         |
| [mars-fx](https://github.com/duderstadt-lab/mars-fx)       | Primary repository for the **Mars** GUI, called the Mars Rover. Written in javafx with supporting libraries for charting, image viewing and markdown editing.       |     [mars-core](https://github.com/duderstadt-lab/mars-core), [ChartFx](https://github.com/GSI-CS-CO/chart-fx), [BigDataViewer](https://github.com/bigdataviewer), [RichTextFX](https://github.com/FXMisc/RichTextFX), [Flexmark](https://github.com/vsch/flexmark-java), [UndoFX](https://github.com/FXMisc/UndoFX), [WellBehavedFx](https://github.com/FXMisc/WellBehavedFX), [Flowless](https://github.com/FXMisc/Flowless), [ControlsFX](https://github.com/controlsfx/controlsfx), [ReactFX](https://github.com/TomasMikula/ReactFX), [JFoenix](https://github.com/jfoenixadmin/JFoenix),  and large portions of [Markdown Writer FX](https://github.com/JFormDesigner/markdown-writer-fx) were incorporated and are slowly being updated to better support future development.             |
| [mars-scifio](https://github.com/duderstadt-lab/mars-scifio)      | SCIFIO MarsMicromanager format and translator. Adds MM 1.4 support, bug fixes and enhancements.  |  [SCIFIO](https://scif.io)  |
| [mars-trackmate](https://github.com/duderstadt-lab/mars-trackmate)      | Mars-TrackMate exporter supports for conversion from TrackMate to Molecule Archives. |    [mars-core](https://github.com/duderstadt-lab/mars-core), [TrackMate](https://github.com/fiji/TrackMate)      |
| [mars-tutorials](https://github.com/duderstadt-lab/mars-tutorials)       | Repository with all example files accompanying the tutorials as well as example scripts.       |                 |
| [mars-docs](https://github.com/duderstadt-lab/mars-docs)       | All information to build the mars documentation website.       |                 |
| [mars-fmt](https://github.com/duderstadt-lab/mars-fmt)      | Experimental mars flow magnetic tweezer plugin development.      |      [mars-core](https://github.com/duderstadt-lab/mars-core)           |
| [fmt-scripts](https://github.com/duderstadt-lab/fmt-scripts)       | Analysis scripts from [Agarwal & Duderstadt, 2020, Nat. Comm.](https://www.nature.com/articles/s41467-020-18456-y).      |    [mars-core](https://github.com/duderstadt-lab/mars-core), [mars-fmt](https://github.com/duderstadt-lab/mars-fmt)           |
| [mars-swing](https://github.com/duderstadt-lab/mars-swing)       | Outdated and unmaintained mars gui written in swing.       |   [mars-core](https://github.com/duderstadt-lab/mars-core)         |
