---
layout: table
title: Sort
permalink: /docs/Table/Sort/index.html
---
This command sorts MarsTables based on the values in the specified column. Default sort order is descending.

#### Inputs

   * "ResultsTable" - An open MARSResultsTable would would like to sort.
   * "Column" - The column to use when sorting the table.
   * "Group Column" - A possible second column to sort by. In the SDMM_Plugins this usually was the molecule column, allowing for the time series data for individual molecules to be sorted as groups in the table.
   * "ascending" - If checked the table will be sorted in ascending order. If left unchecked the table will be sorted in descending order.

#### Examples

Sorting without grouping. The table shows the result.
<img align='center' src='{{site.baseurl}}/docs/Table/img/ResultsTable Sorter Dialog.png' width='700' />
Sorting with grouping. The table displays the result.
<img align='center' src='{{site.baseurl}}/docs/Table/img/Results Sorter with group.png' width='700' />

#### Output

   * This command sorts the table given as input, so there isn't any output except a change in the row order of the table given.

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
