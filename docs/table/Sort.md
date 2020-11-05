---
layout: table
title: Sort
permalink: /docs/table/Sort/index.html
---
This command sorts MarsTables based on the values in the specified column. The default sort order is descending.

#### Inputs

* "ResultsTable" - An open MarsTable one would like to sort.
* "Column" - The column to use when sorting the table.
* "Group Column" - A possible second column to sort by. This can be a molecule column, for example, allowing for the time series data for individual molecules to be sorted as groups.
* "ascending" - If checked the table will be sorted in ascending order. If left unchecked the table will be sorted in descending order.

#### Output
* This command sorts the table given as input, so there isn't any output except a change in the row order of the table given.

<div style="text-align: center"><img  src='{{site.baseurl}}/docs/table/img/img2.png' width='300'/></div>


### How to sort tables in a groovy script

```groovy
#@ MarsTable table

table.sort(true, "Column Name")
// OR if you want to also sort with groupings
//table.sort(true, "Group Column Name", "Column Name")
// OR if you want to sort groups within groups within groups. You can add as many subgroups
// as you want...
//table.sort(true, "Super Group Column Name", "Sub Group Column Name", ..., "Column Name")

//Can use this to make sure the table immediately updates, otherwise, it might only update when clicking on it.
table.getWindow().update()
```

NOTE: The table will not sort unless there is a matching column name. If this happens an error message will appear in the console. Sometimes it can be hard to see if the name actually matches exactly depending on the program used to generate the table. Sometimes tables from excel have an extra space at the beginning of the table. If you open the table in a text editor you can clearly see exactly the correct column names.
