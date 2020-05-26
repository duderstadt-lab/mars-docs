---
layout: scripting
title: "Let's Learn Advanced Groovy Scripting Tutorial"
permalink: /tutorials/scripting/advanced-groovy-scripting/index.html
---
Advanced Groovy Scripting!!

Groovy One liners:
```groovy
archive.getMoleculeUIDs().stream().filter(UID -> archive.moleculeHasTag(UID, tag) ).maptoDouble{ UID -> molecule.getParameter("MSD")}.collect(toList())
```

And example widget for making a histogram from a parameter
```groovy
#@ MoleculeArchive(required=false) archive
#@OUTPUT String xlabel
#@OUTPUT String ylabel
#@OUTPUT String title
#@OUTPUT Integer bins
#@OUTPUT Double xmin
#@OUTPUT Double xmax

//Set global outputs
xlabel = "var"
ylabel = "Frequency"
title = "Variance"
bins = 100
xmin = -10.0
xmax = 10.0

//Series 1 Outputs
#@OUTPUT Double[] series1_values
#@OUTPUT String series1_strokeColor
#@OUTPUT Integer series1_strokeWidth

series1_strokeColor = "rgb(" + r.nextInt(255) + "," + r.nextInt(255) + "," + r.nextInt(255) + ")";
series1_strokeWidth = 2

series1_values = archive.getMoleculeUIDs().stream().mapToDouble{UID -> archive.get(UID).getParameter("var")}.toArray()
```

Much more to come...




### 6. Streams (one-line coding) -> move to advanced tutorial?
