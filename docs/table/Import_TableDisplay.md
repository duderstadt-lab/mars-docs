---
layout: table
title: Import TableDisplay
permalink: /docs/table/Import_TableDisplay/index.html
---

Command to import any Scijava table (such as TableDisplay) to the MarsTable format. This allows for a smooth interaction between the Mars-related commands as well as table features (see also the [MarsTable tutorial](https://duderstadt-lab.github.io/mars-docs/tutorials/scripting/marstable/)). By default, the MarsTable also gives access to the plotter and the scriptable widgets.

#### Inputs
* *SciJava Table* - Table that should be converted. This needs to be an active table in the SciJava format (TableDisplay).

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/table/img/img4.png' width='400'/></div>
<div style="text-align: center"><img  src='{{site.baseurl}}/docs/table/img/img5.png' width='400'/></div>

#### Outputs
* *MarsTable* - The generated output is a MarsTable.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/table/img/img6.png' width='400'/></div>

### How to run this Command from a groovy script

```groovy
#@ org.scijava.TableDisplay tableDisplay
#@output MarsTable table

import de.mpg.biochem.mars.table.*
import org.scijava.Table

table = new MarsTable((Table) display.get(0))
```
