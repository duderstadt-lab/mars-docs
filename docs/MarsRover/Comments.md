---
layout: marsrover
title: Comments
permalink: /docs/MarsRover/Comments/index.html
---

Fully integrated text editor to annotate the archive. Use to annotate with text and inline code. Use the view button (<img align='centre' src='{{site.baseurl}}/docs/img/Rover/img11.png' width='25' />) to show or hide rendered mark down text.


<img align='centre' src='{{site.baseurl}}/docs/img/Rover/img10.png' width='600' />

Markdown rendering depends on extensions from [flexmark](https://github.com/vsch/flexmark-java) and [mermaid](https://mermaid-js.github.io/mermaid/#/README). This allows for easy documentation of tags, parameters, settings and the data analysis flow as a flowchart to be saved in the archive itself to improve reproducibility. Look at the respective web pages for more information on syntax. The example text shows an example of most of the incorporated features. Note that due to the maximum window size the image is split into two separate images.

<img align='centre' src='{{site.baseurl}}/docs/img/Rover/img13.png' width='600' />

<img align='centre' src='{{site.baseurl}}/docs/img/Rover/img14.png' width='600' />

Flexmark extensions in **Mars**:

| :------------- | :------------- |
| Anchorlink       | Converts titels to anchor points. Will automatically move to place the clicked title at the top.       |
| Autolink      | Turns links such as URLs and email addresses into links.       |
| Definition       | Neat formatting of definitions.       |
| Footnotes       | Creates footnote references in the document.        |
| Strikethrough       | Strike through text formatting.       |
| Tables       | Allows for the insertion of a table in the text.        |
| Table of Contents      | Enables the creation of a table of contents based on the defined titles in the document.       |
| Gitlab flavoured markdown       |Enables the addition of some extensions (like strikethrough, tables) as well as mermaid for flowcharts.        |
