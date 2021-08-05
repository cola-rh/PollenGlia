cola Report for Hierarchical Partitioning - 'PollenGlia'
==================

**Date**: 2021-07-26 10:23:01 CEST, **cola version**: 1.9.4

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary



First the variable is renamed to `res_rh`.


```r
res_rh = rh
```



The partition hierarchy and all available functions which can be applied to `res_rh` object.


```r
res_rh
```

```
#> A 'HierarchicalPartition' object with 'ATC:skmeans' method.
#>   On a matrix with 9856 rows and 367 columns.
#>   Performed in total 1650 partitions.
#>   There are 7 groups under the following parameters:
#>     - min_samples: 6
#>     - mean_silhouette_cutoff: 0.9
#>     - min_n_signatures: 203 (signatures are selected based on:)
#>       - fdr_cutoff: 0.05
#>       - group_diff (scaled values): 0.5
#> 
#> Hierarchy of the partition:
#>   0, 367 cols
#>   |-- 01, 129 cols, 1046 signatures
#>   |   |-- 011, 44 cols, 33 signatures (c)
#>   |   |-- 012, 45 cols, 8 signatures (c)
#>   |   `-- 013, 40 cols, 44 signatures (c)
#>   |-- 02, 112 cols, 597 signatures
#>   |   |-- 021, 64 cols, 26 signatures (c)
#>   |   `-- 022, 48 cols, 177 signatures (c)
#>   `-- 03, 126 cols, 329 signatures
#>       |-- 031, 64 cols, 5 signatures (c)
#>       `-- 032, 62 cols, 5 signatures (c)
#> Stop reason:
#>   c) There were too few signatures.
#> 
#> Following methods can be applied to this 'HierarchicalPartition' object:
#>  [1] "all_leaves"            "all_nodes"             "cola_report"           "collect_classes"      
#>  [5] "colnames"              "compare_signatures"    "dimension_reduction"   "functional_enrichment"
#>  [9] "get_anno_col"          "get_anno"              "get_children_nodes"    "get_classes"          
#> [13] "get_matrix"            "get_signatures"        "is_leaf_node"          "max_depth"            
#> [17] "merge_node"            "ncol"                  "node_info"             "node_level"           
#> [21] "nrow"                  "rownames"              "show"                  "split_node"           
#> [25] "suggest_best_k"        "test_to_known_factors" "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single node by e.g. object["01"]
```

The call of `hierarchical_partition()` was:


```
#> hierarchical_partition(data = lt$mat, anno = lt$anno, subset = 500, cores = 4)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_rh)
dim(mat)
```

```
#> [1] 9856  367
```

All the methods that were tried:


```r
res_rh@param$combination_method
```

```
#> [[1]]
#> [1] "ATC"     "skmeans"
```

### Density distribution

The density distribution for each sample is visualized as one column in the following heatmap.
The clustering is based on the distance which is the Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_rh), 
    col = get_anno_col(res_rh)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 1)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)



Some values about the hierarchy:


```r
all_nodes(res_rh)
```

```
#>  [1] "0"   "01"  "011" "012" "013" "02"  "021" "022" "03"  "031" "032"
```

```r
all_leaves(res_rh)
```

```
#> [1] "011" "012" "013" "021" "022" "031" "032"
```

```r
node_info(res_rh)
```

```
#>     id best_method depth best_k n_columns n_signatures p_signatures is_leaf
#> 1    0 ATC:skmeans     1      3       367         4074     0.413352   FALSE
#> 2   01 ATC:skmeans     2      3       129         1046     0.106128   FALSE
#> 3  011 ATC:skmeans     3      2        44           33     0.003348    TRUE
#> 4  012 ATC:skmeans     3      2        45            8     0.000812    TRUE
#> 5  013 ATC:skmeans     3      2        40           44     0.004464    TRUE
#> 6   02 ATC:skmeans     2      2       112          597     0.060572   FALSE
#> 7  021 ATC:skmeans     3      2        64           26     0.002638    TRUE
#> 8  022 ATC:skmeans     3      3        48          177     0.017959    TRUE
#> 9   03 ATC:skmeans     2      2       126          329     0.033381   FALSE
#> 10 031 ATC:skmeans     3      2        64            5     0.000507    TRUE
#> 11 032 ATC:skmeans     3      2        62            5     0.000507    TRUE
```

In the output from `node_info()`, there are the following columns:

- `id`: The node id.
- `best_method`: The best method selected.
- `depth`: Depth of the node in the hierarchy.
- `best_k`: Best number of groups of the partition on that node.
- `n_columns`: Number of columns in the submatrix.
- `n_signatures`: Number of signatures with the `best_k`.
- `p_signatures`: Proportion of hte signatures in total number of rows in the matrix.
- `is_leaf`: Whether the node is a leaf.

Labels of nodes are encoded in a special way. The number of digits
correspond to the depth of the node in the hierarchy and the value of the
digits correspond to the index of the subgroup in the current node, E.g. a label
of “012” means the node is the second subgroup of the partition which is the
first subgroup of the root node.

### Suggest the best k



Following table shows the best `k` (number of partitions) for each node in the
partition hierarchy. Clicking on the node name in the table goes to the
corresponding section for the partitioning on that node.

[The cola vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.



```r
suggest_best_k(res_rh)
```


|Node              |Best method |Is leaf   | Best k|1-PAC |Mean silhouette |Concordance | #samples|   |
|:-----------------|:-----------|:---------|------:|:-----|:---------------|:-----------|--------:|:--|
|[Node0](#Node0)   |ATC:skmeans |          |      4|0.98  |0.95            |0.96        |      367|** |
|[Node01](#Node01) |ATC:skmeans |          |      3|1.00  |0.98            |0.99        |      129|** |
|Node011-leaf      |ATC:skmeans |✓ (&#99;) |      2|0.95  |0.96            |0.98        |       44|** |
|Node012-leaf      |ATC:skmeans |✓ (&#99;) |      2|0.91  |0.92            |0.97        |       45|*  |
|Node013-leaf      |ATC:skmeans |✓ (&#99;) |      2|0.95  |0.96            |0.98        |       40|*  |
|[Node02](#Node02) |ATC:skmeans |          |      2|1.00  |0.97            |0.99        |      112|** |
|Node021-leaf      |ATC:skmeans |✓ (&#99;) |      2|1.00  |0.97            |0.99        |       64|** |
|Node022-leaf      |ATC:skmeans |✓ (&#99;) |      3|1.00  |0.99            |0.99        |       48|** |
|[Node03](#Node03) |ATC:skmeans |          |      2|1.00  |0.97            |0.99        |      126|** |
|Node031-leaf      |ATC:skmeans |✓ (&#99;) |      2|1.00  |0.94            |0.98        |       64|** |
|Node032-leaf      |ATC:skmeans |✓ (&#99;) |      2|0.93  |0.94            |0.98        |       62|*  |


Stop reason: c) There were too few signatures. 

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9


### Partition hierarchy

The nodes of the hierarchy can be merged by setting the `merge_node` parameters. Here we 
control the hierarchy with the `min_n_signatures` parameter. The value of `min_n_signatures` is
from `node_info()`.





<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-classes-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-hierarchical-partition'>
<ul>
<li><a href='#tab-collect-classes-from-hierarchical-partition-1'>n_signatures ≥ 329</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-2'>n_signatures ≥ 597</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-3'>n_signatures ≥ 1046</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-4'>n_signatures ≥ 4074</a></li>
</ul>
<div id='tab-collect-classes-from-hierarchical-partition-1'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 329))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-1-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-1"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-2'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 597))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-2-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-2"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-3'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 1046))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-3-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-3"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-4'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 4074))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-4-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-4"/></p>

</div>
</div>

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it).


<script>
$( function() {
	$( '#tabs-get-classes-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-get-classes-from-hierarchical-partition'>
<ul>
<li><a href='#tab-get-classes-from-hierarchical-partition-1'>n_signatures ≥ 329</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-2'>n_signatures ≥ 597</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-3'>n_signatures ≥ 1046</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-4'>n_signatures ≥ 4074</a></li>
</ul>

<div id='tab-get-classes-from-hierarchical-partition-1'>
<p><a id='tab-get-classes-from-hierarchical-partition-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 329))
</code></pre>

<pre><code>#&gt;  O10-A1 O10-A11 O10-A12  O10-A3  O10-A4  O10-A5  O10-A7  O10-A8  O10-A9  O10-B1 O10-B10 O10-B11 
#&gt;   &quot;022&quot;   &quot;031&quot;   &quot;022&quot;   &quot;012&quot;   &quot;021&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;013&quot;   &quot;011&quot;   &quot;032&quot;   &quot;032&quot; 
#&gt; O10-B12  O10-B2  O10-B5  O10-B6  O10-B7  O10-B9 O10-C10 O10-C11 O10-C12  O10-C2  O10-C4  O10-C5 
#&gt;   &quot;013&quot;   &quot;032&quot;   &quot;032&quot;   &quot;031&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;013&quot;   &quot;032&quot;   &quot;013&quot;   &quot;022&quot;   &quot;032&quot; 
#&gt;  O10-C6  O10-C9  O10-D1 O10-D11 O10-D12  O10-D2  O10-D3  O10-D5  O10-D6  O10-D8  O10-D9  O10-E1 
#&gt;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;012&quot;   &quot;021&quot;   &quot;032&quot;   &quot;032&quot;   &quot;013&quot;   &quot;032&quot;   &quot;032&quot; 
#&gt; O10-E11 O10-E12  O10-E6  O10-E8  O10-E9  O10-F1 O10-F11 O10-F12  O10-F2  O10-F4  O10-F5  O10-F6 
#&gt;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;012&quot;   &quot;013&quot;   &quot;031&quot;   &quot;031&quot;   &quot;032&quot;   &quot;013&quot;   &quot;012&quot;   &quot;031&quot;   &quot;021&quot; 
#&gt;  O10-F8  O10-F9  O10-G1 O10-G12  O10-G2  O10-G3  O10-G4  O10-G5  O10-G7  O10-H1  O10-H4  O10-H5 
#&gt;   &quot;012&quot;   &quot;032&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt;  O10-H6  O10-H7  O12-A1 O12-A12  O12-A2  O12-A3  O12-B1 O12-B12  O12-C1  O12-D1  O12-D2  O12-E1 
#&gt;   &quot;011&quot;   &quot;011&quot;   &quot;021&quot;   &quot;021&quot;   &quot;012&quot;   &quot;021&quot;   &quot;022&quot;   &quot;031&quot;   &quot;022&quot;   &quot;022&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt; O12-E12 O12-F12  O12-F2  O12-G2  O12-H1  O12-H2   O7-A3   O7-A5   O7-A7   O7-A8   O7-A9   O7-B1 
#&gt;   &quot;021&quot;   &quot;031&quot;   &quot;021&quot;   &quot;022&quot;   &quot;021&quot;   &quot;022&quot;   &quot;032&quot;   &quot;022&quot;   &quot;022&quot;   &quot;021&quot;   &quot;031&quot;   &quot;021&quot; 
#&gt;  O7-B11  O7-B12   O7-B6   O7-B7   O7-B8   O7-B9   O7-C1  O7-C11   O7-C2   O7-C3   O7-C6   O7-C7 
#&gt;   &quot;032&quot;   &quot;022&quot;   &quot;032&quot;   &quot;032&quot;   &quot;022&quot;   &quot;021&quot;   &quot;031&quot;   &quot;032&quot;   &quot;022&quot;   &quot;022&quot;   &quot;021&quot;   &quot;032&quot; 
#&gt;   O7-D1  O7-D11  O7-D12   O7-D2   O7-D3   O7-D4   O7-D5  O7-E10  O7-E11   O7-E5   O7-E6   O7-F1 
#&gt;   &quot;021&quot;   &quot;021&quot;   &quot;021&quot;   &quot;021&quot;   &quot;012&quot;   &quot;031&quot;   &quot;013&quot;   &quot;032&quot;   &quot;032&quot;   &quot;021&quot;   &quot;022&quot;   &quot;021&quot; 
#&gt;  O7-F10  O7-F11   O7-F2   O7-F3   O7-F4   O7-F7   O7-F9  O7-G12   O7-G3   O7-G9   O7-H3   O7-H4 
#&gt;   &quot;022&quot;   &quot;021&quot;   &quot;031&quot;   &quot;031&quot;   &quot;021&quot;   &quot;032&quot;   &quot;032&quot;   &quot;022&quot;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot;   &quot;022&quot; 
#&gt;   O7-H8   O8-A2   O8-A3   O8-A5   O8-A8   O8-A9   O8-B1   O8-B2   O8-B4   O8-B6   O8-B7   O8-B9 
#&gt;   &quot;022&quot;   &quot;031&quot;   &quot;032&quot;   &quot;022&quot;   &quot;032&quot;   &quot;021&quot;   &quot;022&quot;   &quot;031&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot; 
#&gt;  O8-C11  O8-C12   O8-C2   O8-C3   O8-C4   O8-C5   O8-C6   O8-C8   O8-D3   O8-D5   O8-D6   O8-D9 
#&gt;   &quot;031&quot;   &quot;032&quot;   &quot;021&quot;   &quot;031&quot;   &quot;031&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;031&quot;   &quot;031&quot;   &quot;031&quot;   &quot;031&quot; 
#&gt;   O8-E1  O8-E11   O8-E2   O8-E3   O8-E5   O8-E7   O8-E9  O8-F10  O8-F11  O8-F12   O8-F2   O8-F3 
#&gt;   &quot;012&quot;   &quot;032&quot;   &quot;031&quot;   &quot;021&quot;   &quot;031&quot;   &quot;022&quot;   &quot;032&quot;   &quot;011&quot;   &quot;032&quot;   &quot;032&quot;   &quot;022&quot;   &quot;032&quot; 
#&gt;   O8-F6   O8-F8   O8-F9   O8-G1  O8-G12   O8-G5   O8-G9   O8-H1  O8-H11   O8-H2   O8-H6  O9-A10 
#&gt;   &quot;022&quot;   &quot;021&quot;   &quot;011&quot;   &quot;031&quot;   &quot;032&quot;   &quot;032&quot;   &quot;011&quot;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot;   &quot;012&quot;   &quot;031&quot; 
#&gt;  O9-A12   O9-A2   O9-A3   O9-A5   O9-A6   O9-A8   O9-A9  O9-B10  O9-B11  O9-B12   O9-B2   O9-B3 
#&gt;   &quot;022&quot;   &quot;013&quot;   &quot;022&quot;   &quot;021&quot;   &quot;021&quot;   &quot;031&quot;   &quot;013&quot;   &quot;013&quot;   &quot;013&quot;   &quot;031&quot;   &quot;031&quot;   &quot;013&quot; 
#&gt;   O9-B5   O9-B6   O9-B7   O9-B8  O9-C10   O9-C2   O9-C3   O9-C4   O9-C5   O9-C6   O9-C9   O9-D1 
#&gt;   &quot;021&quot;   &quot;031&quot;   &quot;031&quot;   &quot;021&quot;   &quot;013&quot;   &quot;031&quot;   &quot;013&quot;   &quot;031&quot;   &quot;013&quot;   &quot;031&quot;   &quot;013&quot;   &quot;013&quot; 
#&gt;  O9-D10  O9-D11  O9-D12   O9-D2   O9-D5   O9-D6   O9-D7   O9-D8   O9-E1  O9-E10  O9-E11  O9-E12 
#&gt;   &quot;011&quot;   &quot;013&quot;   &quot;031&quot;   &quot;021&quot;   &quot;012&quot;   &quot;031&quot;   &quot;022&quot;   &quot;022&quot;   &quot;022&quot;   &quot;013&quot;   &quot;011&quot;   &quot;031&quot; 
#&gt;   O9-E2   O9-E4   O9-E5   O9-E7   O9-E8   O9-E9   O9-F1  O9-F10  O9-F12   O9-F2   O9-F4   O9-F5 
#&gt;   &quot;021&quot;   &quot;031&quot;   &quot;013&quot;   &quot;013&quot;   &quot;011&quot;   &quot;013&quot;   &quot;021&quot;   &quot;013&quot;   &quot;013&quot;   &quot;013&quot;   &quot;021&quot;   &quot;021&quot; 
#&gt;   O9-F9  O9-G10  O9-G12   O9-G2   O9-G3   O9-G4   O9-G6   O9-G7   O9-G9  O9-H12   O9-H3   O9-H5 
#&gt;   &quot;031&quot;   &quot;011&quot;   &quot;013&quot;   &quot;013&quot;   &quot;031&quot;   &quot;011&quot;   &quot;013&quot;   &quot;031&quot;   &quot;021&quot;   &quot;022&quot;   &quot;013&quot;   &quot;022&quot; 
#&gt;   O9-H9  S37-A1 S37-A10  S37-A2  S37-A3  S37-A4  S37-A5  S37-A6  S37-A7  S37-A8  S37-A9  S37-B1 
#&gt;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;   &quot;022&quot;   &quot;012&quot;   &quot;031&quot;   &quot;021&quot;   &quot;021&quot;   &quot;031&quot;   &quot;022&quot;   &quot;011&quot;   &quot;013&quot; 
#&gt; S37-B10 S37-B11 S37-B12  S37-B2  S37-B3  S37-B4  S37-B5  S37-B6  S37-B7  S37-B9 S37-C10 S37-C12 
#&gt;   &quot;012&quot;   &quot;021&quot;   &quot;022&quot;   &quot;012&quot;   &quot;031&quot;   &quot;013&quot;   &quot;031&quot;   &quot;031&quot;   &quot;012&quot;   &quot;031&quot;   &quot;012&quot;   &quot;022&quot; 
#&gt;  S37-C3  S37-C4  S37-C6  S37-C7  S37-C8  S37-C9 S37-D10 S37-D11 S37-D12  S37-D2  S37-D4  S37-D6 
#&gt;   &quot;012&quot;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;   &quot;021&quot;   &quot;011&quot;   &quot;012&quot;   &quot;031&quot;   &quot;021&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot; 
#&gt;  S37-D8  S37-D9  S37-E1 S37-E10 S37-E11  S37-E2  S37-E3  S37-E5  S37-E6  S37-E7  S37-E8  S37-E9 
#&gt;   &quot;012&quot;   &quot;031&quot;   &quot;012&quot;   &quot;031&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;031&quot;   &quot;013&quot;   &quot;021&quot;   &quot;021&quot;   &quot;012&quot; 
#&gt;  S37-F1 S37-F10 S37-F12  S37-F2  S37-F3  S37-F5  S37-F7  S37-F9  S37-G1 S37-G10 S37-G12  S37-G2 
#&gt;   &quot;011&quot;   &quot;011&quot;   &quot;013&quot;   &quot;012&quot;   &quot;013&quot;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;022&quot; 
#&gt;  S37-G3  S37-G4  S37-G5  S37-G6  S37-G7  S37-G8  S37-H2  S37-H4  S37-H7  S37-H8  S37-H9  S38-A1 
#&gt;   &quot;012&quot;   &quot;022&quot;   &quot;031&quot;   &quot;012&quot;   &quot;012&quot;   &quot;031&quot;   &quot;011&quot;   &quot;031&quot;   &quot;012&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt; S38-A10 S38-A12  S38-A2  S38-A3  S38-A5  S38-A9 S38-B10 S38-B11  S38-B2  S38-B6  S38-B7  S38-B8 
#&gt;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot;   &quot;031&quot;   &quot;021&quot;   &quot;021&quot;   &quot;032&quot;   &quot;013&quot;   &quot;021&quot;   &quot;011&quot;   &quot;021&quot;   &quot;021&quot; 
#&gt;  S38-B9  S38-C1 S38-C10 S38-C11  S38-C3  S38-C4  S38-C5  S38-C6  S38-C7  S38-C9  S38-D1 S38-D10 
#&gt;   &quot;022&quot;   &quot;032&quot;   &quot;032&quot;   &quot;032&quot;   &quot;021&quot;   &quot;012&quot;   &quot;032&quot;   &quot;031&quot;   &quot;031&quot;   &quot;022&quot;   &quot;021&quot;   &quot;032&quot; 
#&gt; S38-D11 S38-D12  S38-D2  S38-D4  S38-D5  S38-D6  S38-D7  S38-D8  S38-D9  S38-E1  S38-E2  S38-E3 
#&gt;   &quot;032&quot;   &quot;011&quot;   &quot;013&quot;   &quot;031&quot;   &quot;021&quot;   &quot;021&quot;   &quot;031&quot;   &quot;011&quot;   &quot;012&quot;   &quot;021&quot;   &quot;031&quot;   &quot;031&quot; 
#&gt;  S38-E4  S38-E5  S38-E6  S38-E7  S38-E8  S38-E9 S38-F10 S38-F11  S38-F2  S38-F3  S38-F5  S38-F6 
#&gt;   &quot;021&quot;   &quot;022&quot;   &quot;022&quot;   &quot;021&quot;   &quot;031&quot;   &quot;022&quot;   &quot;013&quot;   &quot;012&quot;   &quot;021&quot;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot; 
#&gt;  S38-F7  S38-F8  S38-F9 S38-G10 S38-G12  S38-G4  S38-G5  S38-G6  S38-G7  S38-G8  S38-G9  S38-H1 
#&gt;   &quot;031&quot;   &quot;021&quot;   &quot;013&quot;   &quot;011&quot;   &quot;011&quot;   &quot;021&quot;   &quot;021&quot;   &quot;022&quot;   &quot;032&quot;   &quot;031&quot;   &quot;032&quot;   &quot;011&quot; 
#&gt; S38-H11  S38-H2  S38-H3  S38-H4  S38-H5  S38-H6  S38-H8 
#&gt;   &quot;013&quot;   &quot;021&quot;   &quot;031&quot;   &quot;012&quot;   &quot;022&quot;   &quot;021&quot;   &quot;012&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-1-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-1-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-2'>
<p><a id='tab-get-classes-from-hierarchical-partition-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 597))
</code></pre>

<pre><code>#&gt;  O10-A1 O10-A11 O10-A12  O10-A3  O10-A4  O10-A5  O10-A7  O10-A8  O10-A9  O10-B1 O10-B10 O10-B11 
#&gt;   &quot;022&quot;    &quot;03&quot;   &quot;022&quot;   &quot;012&quot;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;   &quot;011&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt; O10-B12  O10-B2  O10-B5  O10-B6  O10-B7  O10-B9 O10-C10 O10-C11 O10-C12  O10-C2  O10-C4  O10-C5 
#&gt;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;   &quot;022&quot;    &quot;03&quot; 
#&gt;  O10-C6  O10-C9  O10-D1 O10-D11 O10-D12  O10-D2  O10-D3  O10-D5  O10-D6  O10-D8  O10-D9  O10-E1 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;012&quot;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt; O10-E11 O10-E12  O10-E6  O10-E8  O10-E9  O10-F1 O10-F11 O10-F12  O10-F2  O10-F4  O10-F5  O10-F6 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;012&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;   &quot;012&quot;    &quot;03&quot;   &quot;021&quot; 
#&gt;  O10-F8  O10-F9  O10-G1 O10-G12  O10-G2  O10-G3  O10-G4  O10-G5  O10-G7  O10-H1  O10-H4  O10-H5 
#&gt;   &quot;012&quot;    &quot;03&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt;  O10-H6  O10-H7  O12-A1 O12-A12  O12-A2  O12-A3  O12-B1 O12-B12  O12-C1  O12-D1  O12-D2  O12-E1 
#&gt;   &quot;011&quot;   &quot;011&quot;   &quot;021&quot;   &quot;021&quot;   &quot;012&quot;   &quot;021&quot;   &quot;022&quot;    &quot;03&quot;   &quot;022&quot;   &quot;022&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt; O12-E12 O12-F12  O12-F2  O12-G2  O12-H1  O12-H2   O7-A3   O7-A5   O7-A7   O7-A8   O7-A9   O7-B1 
#&gt;   &quot;021&quot;    &quot;03&quot;   &quot;021&quot;   &quot;022&quot;   &quot;021&quot;   &quot;022&quot;    &quot;03&quot;   &quot;022&quot;   &quot;022&quot;   &quot;021&quot;    &quot;03&quot;   &quot;021&quot; 
#&gt;  O7-B11  O7-B12   O7-B6   O7-B7   O7-B8   O7-B9   O7-C1  O7-C11   O7-C2   O7-C3   O7-C6   O7-C7 
#&gt;    &quot;03&quot;   &quot;022&quot;    &quot;03&quot;    &quot;03&quot;   &quot;022&quot;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot;   &quot;022&quot;   &quot;022&quot;   &quot;021&quot;    &quot;03&quot; 
#&gt;   O7-D1  O7-D11  O7-D12   O7-D2   O7-D3   O7-D4   O7-D5  O7-E10  O7-E11   O7-E5   O7-E6   O7-F1 
#&gt;   &quot;021&quot;   &quot;021&quot;   &quot;021&quot;   &quot;021&quot;   &quot;012&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;   &quot;021&quot;   &quot;022&quot;   &quot;021&quot; 
#&gt;  O7-F10  O7-F11   O7-F2   O7-F3   O7-F4   O7-F7   O7-F9  O7-G12   O7-G3   O7-G9   O7-H3   O7-H4 
#&gt;   &quot;022&quot;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot;   &quot;022&quot;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot;   &quot;022&quot; 
#&gt;   O7-H8   O8-A2   O8-A3   O8-A5   O8-A8   O8-A9   O8-B1   O8-B2   O8-B4   O8-B6   O8-B7   O8-B9 
#&gt;   &quot;022&quot;    &quot;03&quot;    &quot;03&quot;   &quot;022&quot;    &quot;03&quot;   &quot;021&quot;   &quot;022&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;  O8-C11  O8-C12   O8-C2   O8-C3   O8-C4   O8-C5   O8-C6   O8-C8   O8-D3   O8-D5   O8-D6   O8-D9 
#&gt;    &quot;03&quot;    &quot;03&quot;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;   O8-E1  O8-E11   O8-E2   O8-E3   O8-E5   O8-E7   O8-E9  O8-F10  O8-F11  O8-F12   O8-F2   O8-F3 
#&gt;   &quot;012&quot;    &quot;03&quot;    &quot;03&quot;   &quot;021&quot;    &quot;03&quot;   &quot;022&quot;    &quot;03&quot;   &quot;011&quot;    &quot;03&quot;    &quot;03&quot;   &quot;022&quot;    &quot;03&quot; 
#&gt;   O8-F6   O8-F8   O8-F9   O8-G1  O8-G12   O8-G5   O8-G9   O8-H1  O8-H11   O8-H2   O8-H6  O9-A10 
#&gt;   &quot;022&quot;   &quot;021&quot;   &quot;011&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;011&quot;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot;   &quot;012&quot;    &quot;03&quot; 
#&gt;  O9-A12   O9-A2   O9-A3   O9-A5   O9-A6   O9-A8   O9-A9  O9-B10  O9-B11  O9-B12   O9-B2   O9-B3 
#&gt;   &quot;022&quot;   &quot;013&quot;   &quot;022&quot;   &quot;021&quot;   &quot;021&quot;    &quot;03&quot;   &quot;013&quot;   &quot;013&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot; 
#&gt;   O9-B5   O9-B6   O9-B7   O9-B8  O9-C10   O9-C2   O9-C3   O9-C4   O9-C5   O9-C6   O9-C9   O9-D1 
#&gt;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot;   &quot;021&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;   &quot;013&quot; 
#&gt;  O9-D10  O9-D11  O9-D12   O9-D2   O9-D5   O9-D6   O9-D7   O9-D8   O9-E1  O9-E10  O9-E11  O9-E12 
#&gt;   &quot;011&quot;   &quot;013&quot;    &quot;03&quot;   &quot;021&quot;   &quot;012&quot;    &quot;03&quot;   &quot;022&quot;   &quot;022&quot;   &quot;022&quot;   &quot;013&quot;   &quot;011&quot;    &quot;03&quot; 
#&gt;   O9-E2   O9-E4   O9-E5   O9-E7   O9-E8   O9-E9   O9-F1  O9-F10  O9-F12   O9-F2   O9-F4   O9-F5 
#&gt;   &quot;021&quot;    &quot;03&quot;   &quot;013&quot;   &quot;013&quot;   &quot;011&quot;   &quot;013&quot;   &quot;021&quot;   &quot;013&quot;   &quot;013&quot;   &quot;013&quot;   &quot;021&quot;   &quot;021&quot; 
#&gt;   O9-F9  O9-G10  O9-G12   O9-G2   O9-G3   O9-G4   O9-G6   O9-G7   O9-G9  O9-H12   O9-H3   O9-H5 
#&gt;    &quot;03&quot;   &quot;011&quot;   &quot;013&quot;   &quot;013&quot;    &quot;03&quot;   &quot;011&quot;   &quot;013&quot;    &quot;03&quot;   &quot;021&quot;   &quot;022&quot;   &quot;013&quot;   &quot;022&quot; 
#&gt;   O9-H9  S37-A1 S37-A10  S37-A2  S37-A3  S37-A4  S37-A5  S37-A6  S37-A7  S37-A8  S37-A9  S37-B1 
#&gt;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;   &quot;022&quot;   &quot;012&quot;    &quot;03&quot;   &quot;021&quot;   &quot;021&quot;    &quot;03&quot;   &quot;022&quot;   &quot;011&quot;   &quot;013&quot; 
#&gt; S37-B10 S37-B11 S37-B12  S37-B2  S37-B3  S37-B4  S37-B5  S37-B6  S37-B7  S37-B9 S37-C10 S37-C12 
#&gt;   &quot;012&quot;   &quot;021&quot;   &quot;022&quot;   &quot;012&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;   &quot;012&quot;    &quot;03&quot;   &quot;012&quot;   &quot;022&quot; 
#&gt;  S37-C3  S37-C4  S37-C6  S37-C7  S37-C8  S37-C9 S37-D10 S37-D11 S37-D12  S37-D2  S37-D4  S37-D6 
#&gt;   &quot;012&quot;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;   &quot;021&quot;   &quot;011&quot;   &quot;012&quot;    &quot;03&quot;   &quot;021&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot; 
#&gt;  S37-D8  S37-D9  S37-E1 S37-E10 S37-E11  S37-E2  S37-E3  S37-E5  S37-E6  S37-E7  S37-E8  S37-E9 
#&gt;   &quot;012&quot;    &quot;03&quot;   &quot;012&quot;    &quot;03&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;    &quot;03&quot;   &quot;013&quot;   &quot;021&quot;   &quot;021&quot;   &quot;012&quot; 
#&gt;  S37-F1 S37-F10 S37-F12  S37-F2  S37-F3  S37-F5  S37-F7  S37-F9  S37-G1 S37-G10 S37-G12  S37-G2 
#&gt;   &quot;011&quot;   &quot;011&quot;   &quot;013&quot;   &quot;012&quot;   &quot;013&quot;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;022&quot; 
#&gt;  S37-G3  S37-G4  S37-G5  S37-G6  S37-G7  S37-G8  S37-H2  S37-H4  S37-H7  S37-H8  S37-H9  S38-A1 
#&gt;   &quot;012&quot;   &quot;022&quot;    &quot;03&quot;   &quot;012&quot;   &quot;012&quot;    &quot;03&quot;   &quot;011&quot;    &quot;03&quot;   &quot;012&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt; S38-A10 S38-A12  S38-A2  S38-A3  S38-A5  S38-A9 S38-B10 S38-B11  S38-B2  S38-B6  S38-B7  S38-B8 
#&gt;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot;    &quot;03&quot;   &quot;021&quot;   &quot;021&quot;    &quot;03&quot;   &quot;013&quot;   &quot;021&quot;   &quot;011&quot;   &quot;021&quot;   &quot;021&quot; 
#&gt;  S38-B9  S38-C1 S38-C10 S38-C11  S38-C3  S38-C4  S38-C5  S38-C6  S38-C7  S38-C9  S38-D1 S38-D10 
#&gt;   &quot;022&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;021&quot;   &quot;012&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;022&quot;   &quot;021&quot;    &quot;03&quot; 
#&gt; S38-D11 S38-D12  S38-D2  S38-D4  S38-D5  S38-D6  S38-D7  S38-D8  S38-D9  S38-E1  S38-E2  S38-E3 
#&gt;    &quot;03&quot;   &quot;011&quot;   &quot;013&quot;    &quot;03&quot;   &quot;021&quot;   &quot;021&quot;    &quot;03&quot;   &quot;011&quot;   &quot;012&quot;   &quot;021&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;  S38-E4  S38-E5  S38-E6  S38-E7  S38-E8  S38-E9 S38-F10 S38-F11  S38-F2  S38-F3  S38-F5  S38-F6 
#&gt;   &quot;021&quot;   &quot;022&quot;   &quot;022&quot;   &quot;021&quot;    &quot;03&quot;   &quot;022&quot;   &quot;013&quot;   &quot;012&quot;   &quot;021&quot;   &quot;011&quot;   &quot;022&quot;   &quot;021&quot; 
#&gt;  S38-F7  S38-F8  S38-F9 S38-G10 S38-G12  S38-G4  S38-G5  S38-G6  S38-G7  S38-G8  S38-G9  S38-H1 
#&gt;    &quot;03&quot;   &quot;021&quot;   &quot;013&quot;   &quot;011&quot;   &quot;011&quot;   &quot;021&quot;   &quot;021&quot;   &quot;022&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;011&quot; 
#&gt; S38-H11  S38-H2  S38-H3  S38-H4  S38-H5  S38-H6  S38-H8 
#&gt;   &quot;013&quot;   &quot;021&quot;    &quot;03&quot;   &quot;012&quot;   &quot;022&quot;   &quot;021&quot;   &quot;012&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-2-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-2-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-3'>
<p><a id='tab-get-classes-from-hierarchical-partition-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 1046))
</code></pre>

<pre><code>#&gt;  O10-A1 O10-A11 O10-A12  O10-A3  O10-A4  O10-A5  O10-A7  O10-A8  O10-A9  O10-B1 O10-B10 O10-B11 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;   &quot;012&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;   &quot;011&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt; O10-B12  O10-B2  O10-B5  O10-B6  O10-B7  O10-B9 O10-C10 O10-C11 O10-C12  O10-C2  O10-C4  O10-C5 
#&gt;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt;  O10-C6  O10-C9  O10-D1 O10-D11 O10-D12  O10-D2  O10-D3  O10-D5  O10-D6  O10-D8  O10-D9  O10-E1 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;012&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt; O10-E11 O10-E12  O10-E6  O10-E8  O10-E9  O10-F1 O10-F11 O10-F12  O10-F2  O10-F4  O10-F5  O10-F6 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;012&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot;   &quot;012&quot;    &quot;03&quot;    &quot;02&quot; 
#&gt;  O10-F8  O10-F9  O10-G1 O10-G12  O10-G2  O10-G3  O10-G4  O10-G5  O10-G7  O10-H1  O10-H4  O10-H5 
#&gt;   &quot;012&quot;    &quot;03&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt;  O10-H6  O10-H7  O12-A1 O12-A12  O12-A2  O12-A3  O12-B1 O12-B12  O12-C1  O12-D1  O12-D2  O12-E1 
#&gt;   &quot;011&quot;   &quot;011&quot;    &quot;02&quot;    &quot;02&quot;   &quot;012&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt; O12-E12 O12-F12  O12-F2  O12-G2  O12-H1  O12-H2   O7-A3   O7-A5   O7-A7   O7-A8   O7-A9   O7-B1 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot; 
#&gt;  O7-B11  O7-B12   O7-B6   O7-B7   O7-B8   O7-B9   O7-C1  O7-C11   O7-C2   O7-C3   O7-C6   O7-C7 
#&gt;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt;   O7-D1  O7-D11  O7-D12   O7-D2   O7-D3   O7-D4   O7-D5  O7-E10  O7-E11   O7-E5   O7-E6   O7-F1 
#&gt;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;   &quot;012&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;  O7-F10  O7-F11   O7-F2   O7-F3   O7-F4   O7-F7   O7-F9  O7-G12   O7-G3   O7-G9   O7-H3   O7-H4 
#&gt;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;   &quot;011&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;   O7-H8   O8-A2   O8-A3   O8-A5   O8-A8   O8-A9   O8-B1   O8-B2   O8-B4   O8-B6   O8-B7   O8-B9 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;  O8-C11  O8-C12   O8-C2   O8-C3   O8-C4   O8-C5   O8-C6   O8-C8   O8-D3   O8-D5   O8-D6   O8-D9 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;   O8-E1  O8-E11   O8-E2   O8-E3   O8-E5   O8-E7   O8-E9  O8-F10  O8-F11  O8-F12   O8-F2   O8-F3 
#&gt;   &quot;012&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;   &quot;011&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt;   O8-F6   O8-F8   O8-F9   O8-G1  O8-G12   O8-G5   O8-G9   O8-H1  O8-H11   O8-H2   O8-H6  O9-A10 
#&gt;    &quot;02&quot;    &quot;02&quot;   &quot;011&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;011&quot;   &quot;011&quot;    &quot;02&quot;    &quot;02&quot;   &quot;012&quot;    &quot;03&quot; 
#&gt;  O9-A12   O9-A2   O9-A3   O9-A5   O9-A6   O9-A8   O9-A9  O9-B10  O9-B11  O9-B12   O9-B2   O9-B3 
#&gt;    &quot;02&quot;   &quot;013&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;   &quot;013&quot;   &quot;013&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;   &quot;013&quot; 
#&gt;   O9-B5   O9-B6   O9-B7   O9-B8  O9-C10   O9-C2   O9-C3   O9-C4   O9-C5   O9-C6   O9-C9   O9-D1 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;   &quot;013&quot;   &quot;013&quot; 
#&gt;  O9-D10  O9-D11  O9-D12   O9-D2   O9-D5   O9-D6   O9-D7   O9-D8   O9-E1  O9-E10  O9-E11  O9-E12 
#&gt;   &quot;011&quot;   &quot;013&quot;    &quot;03&quot;    &quot;02&quot;   &quot;012&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;   &quot;013&quot;   &quot;011&quot;    &quot;03&quot; 
#&gt;   O9-E2   O9-E4   O9-E5   O9-E7   O9-E8   O9-E9   O9-F1  O9-F10  O9-F12   O9-F2   O9-F4   O9-F5 
#&gt;    &quot;02&quot;    &quot;03&quot;   &quot;013&quot;   &quot;013&quot;   &quot;011&quot;   &quot;013&quot;    &quot;02&quot;   &quot;013&quot;   &quot;013&quot;   &quot;013&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;   O9-F9  O9-G10  O9-G12   O9-G2   O9-G3   O9-G4   O9-G6   O9-G7   O9-G9  O9-H12   O9-H3   O9-H5 
#&gt;    &quot;03&quot;   &quot;011&quot;   &quot;013&quot;   &quot;013&quot;    &quot;03&quot;   &quot;011&quot;   &quot;013&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;   &quot;013&quot;    &quot;02&quot; 
#&gt;   O9-H9  S37-A1 S37-A10  S37-A2  S37-A3  S37-A4  S37-A5  S37-A6  S37-A7  S37-A8  S37-A9  S37-B1 
#&gt;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;    &quot;02&quot;   &quot;012&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;   &quot;011&quot;   &quot;013&quot; 
#&gt; S37-B10 S37-B11 S37-B12  S37-B2  S37-B3  S37-B4  S37-B5  S37-B6  S37-B7  S37-B9 S37-C10 S37-C12 
#&gt;   &quot;012&quot;    &quot;02&quot;    &quot;02&quot;   &quot;012&quot;    &quot;03&quot;   &quot;013&quot;    &quot;03&quot;    &quot;03&quot;   &quot;012&quot;    &quot;03&quot;   &quot;012&quot;    &quot;02&quot; 
#&gt;  S37-C3  S37-C4  S37-C6  S37-C7  S37-C8  S37-C9 S37-D10 S37-D11 S37-D12  S37-D2  S37-D4  S37-D6 
#&gt;   &quot;012&quot;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;    &quot;02&quot;   &quot;011&quot;   &quot;012&quot;    &quot;03&quot;    &quot;02&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot; 
#&gt;  S37-D8  S37-D9  S37-E1 S37-E10 S37-E11  S37-E2  S37-E3  S37-E5  S37-E6  S37-E7  S37-E8  S37-E9 
#&gt;   &quot;012&quot;    &quot;03&quot;   &quot;012&quot;    &quot;03&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;    &quot;03&quot;   &quot;013&quot;    &quot;02&quot;    &quot;02&quot;   &quot;012&quot; 
#&gt;  S37-F1 S37-F10 S37-F12  S37-F2  S37-F3  S37-F5  S37-F7  S37-F9  S37-G1 S37-G10 S37-G12  S37-G2 
#&gt;   &quot;011&quot;   &quot;011&quot;   &quot;013&quot;   &quot;012&quot;   &quot;013&quot;   &quot;011&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;   &quot;012&quot;    &quot;02&quot; 
#&gt;  S37-G3  S37-G4  S37-G5  S37-G6  S37-G7  S37-G8  S37-H2  S37-H4  S37-H7  S37-H8  S37-H9  S38-A1 
#&gt;   &quot;012&quot;    &quot;02&quot;    &quot;03&quot;   &quot;012&quot;   &quot;012&quot;    &quot;03&quot;   &quot;011&quot;    &quot;03&quot;   &quot;012&quot;   &quot;011&quot;   &quot;011&quot;   &quot;011&quot; 
#&gt; S38-A10 S38-A12  S38-A2  S38-A3  S38-A5  S38-A9 S38-B10 S38-B11  S38-B2  S38-B6  S38-B7  S38-B8 
#&gt;   &quot;011&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;   &quot;013&quot;    &quot;02&quot;   &quot;011&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;  S38-B9  S38-C1 S38-C10 S38-C11  S38-C3  S38-C4  S38-C5  S38-C6  S38-C7  S38-C9  S38-D1 S38-D10 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;   &quot;012&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt; S38-D11 S38-D12  S38-D2  S38-D4  S38-D5  S38-D6  S38-D7  S38-D8  S38-D9  S38-E1  S38-E2  S38-E3 
#&gt;    &quot;03&quot;   &quot;011&quot;   &quot;013&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;   &quot;011&quot;   &quot;012&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;  S38-E4  S38-E5  S38-E6  S38-E7  S38-E8  S38-E9 S38-F10 S38-F11  S38-F2  S38-F3  S38-F5  S38-F6 
#&gt;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;   &quot;013&quot;   &quot;012&quot;    &quot;02&quot;   &quot;011&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;  S38-F7  S38-F8  S38-F9 S38-G10 S38-G12  S38-G4  S38-G5  S38-G6  S38-G7  S38-G8  S38-G9  S38-H1 
#&gt;    &quot;03&quot;    &quot;02&quot;   &quot;013&quot;   &quot;011&quot;   &quot;011&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;   &quot;011&quot; 
#&gt; S38-H11  S38-H2  S38-H3  S38-H4  S38-H5  S38-H6  S38-H8 
#&gt;   &quot;013&quot;    &quot;02&quot;    &quot;03&quot;   &quot;012&quot;    &quot;02&quot;    &quot;02&quot;   &quot;012&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-3-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-3-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-4'>
<p><a id='tab-get-classes-from-hierarchical-partition-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 4074))
</code></pre>

<pre><code>#&gt;  O10-A1 O10-A11 O10-A12  O10-A3  O10-A4  O10-A5  O10-A7  O10-A8  O10-A9  O10-B1 O10-B10 O10-B11 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt; O10-B12  O10-B2  O10-B5  O10-B6  O10-B7  O10-B9 O10-C10 O10-C11 O10-C12  O10-C2  O10-C4  O10-C5 
#&gt;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt;  O10-C6  O10-C9  O10-D1 O10-D11 O10-D12  O10-D2  O10-D3  O10-D5  O10-D6  O10-D8  O10-D9  O10-E1 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt; O10-E11 O10-E12  O10-E6  O10-E8  O10-E9  O10-F1 O10-F11 O10-F12  O10-F2  O10-F4  O10-F5  O10-F6 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;02&quot; 
#&gt;  O10-F8  O10-F9  O10-G1 O10-G12  O10-G2  O10-G3  O10-G4  O10-G5  O10-G7  O10-H1  O10-H4  O10-H5 
#&gt;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot; 
#&gt;  O10-H6  O10-H7  O12-A1 O12-A12  O12-A2  O12-A3  O12-B1 O12-B12  O12-C1  O12-D1  O12-D2  O12-E1 
#&gt;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot; 
#&gt; O12-E12 O12-F12  O12-F2  O12-G2  O12-H1  O12-H2   O7-A3   O7-A5   O7-A7   O7-A8   O7-A9   O7-B1 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot; 
#&gt;  O7-B11  O7-B12   O7-B6   O7-B7   O7-B8   O7-B9   O7-C1  O7-C11   O7-C2   O7-C3   O7-C6   O7-C7 
#&gt;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt;   O7-D1  O7-D11  O7-D12   O7-D2   O7-D3   O7-D4   O7-D5  O7-E10  O7-E11   O7-E5   O7-E6   O7-F1 
#&gt;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;  O7-F10  O7-F11   O7-F2   O7-F3   O7-F4   O7-F7   O7-F9  O7-G12   O7-G3   O7-G9   O7-H3   O7-H4 
#&gt;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;   O7-H8   O8-A2   O8-A3   O8-A5   O8-A8   O8-A9   O8-B1   O8-B2   O8-B4   O8-B6   O8-B7   O8-B9 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;  O8-C11  O8-C12   O8-C2   O8-C3   O8-C4   O8-C5   O8-C6   O8-C8   O8-D3   O8-D5   O8-D6   O8-D9 
#&gt;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;   O8-E1  O8-E11   O8-E2   O8-E3   O8-E5   O8-E7   O8-E9  O8-F10  O8-F11  O8-F12   O8-F2   O8-F3 
#&gt;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt;   O8-F6   O8-F8   O8-F9   O8-G1  O8-G12   O8-G5   O8-G9   O8-H1  O8-H11   O8-H2   O8-H6  O9-A10 
#&gt;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot; 
#&gt;  O9-A12   O9-A2   O9-A3   O9-A5   O9-A6   O9-A8   O9-A9  O9-B10  O9-B11  O9-B12   O9-B2   O9-B3 
#&gt;    &quot;02&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot; 
#&gt;   O9-B5   O9-B6   O9-B7   O9-B8  O9-C10   O9-C2   O9-C3   O9-C4   O9-C5   O9-C6   O9-C9   O9-D1 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot; 
#&gt;  O9-D10  O9-D11  O9-D12   O9-D2   O9-D5   O9-D6   O9-D7   O9-D8   O9-E1  O9-E10  O9-E11  O9-E12 
#&gt;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot; 
#&gt;   O9-E2   O9-E4   O9-E5   O9-E7   O9-E8   O9-E9   O9-F1  O9-F10  O9-F12   O9-F2   O9-F4   O9-F5 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;   O9-F9  O9-G10  O9-G12   O9-G2   O9-G3   O9-G4   O9-G6   O9-G7   O9-G9  O9-H12   O9-H3   O9-H5 
#&gt;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;02&quot; 
#&gt;   O9-H9  S37-A1 S37-A10  S37-A2  S37-A3  S37-A4  S37-A5  S37-A6  S37-A7  S37-A8  S37-A9  S37-B1 
#&gt;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot; 
#&gt; S37-B10 S37-B11 S37-B12  S37-B2  S37-B3  S37-B4  S37-B5  S37-B6  S37-B7  S37-B9 S37-C10 S37-C12 
#&gt;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;02&quot; 
#&gt;  S37-C3  S37-C4  S37-C6  S37-C7  S37-C8  S37-C9 S37-D10 S37-D11 S37-D12  S37-D2  S37-D4  S37-D6 
#&gt;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot; 
#&gt;  S37-D8  S37-D9  S37-E1 S37-E10 S37-E11  S37-E2  S37-E3  S37-E5  S37-E6  S37-E7  S37-E8  S37-E9 
#&gt;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot; 
#&gt;  S37-F1 S37-F10 S37-F12  S37-F2  S37-F3  S37-F5  S37-F7  S37-F9  S37-G1 S37-G10 S37-G12  S37-G2 
#&gt;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot; 
#&gt;  S37-G3  S37-G4  S37-G5  S37-G6  S37-G7  S37-G8  S37-H2  S37-H4  S37-H7  S37-H8  S37-H9  S38-A1 
#&gt;    &quot;01&quot;    &quot;02&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot; 
#&gt; S38-A10 S38-A12  S38-A2  S38-A3  S38-A5  S38-A9 S38-B10 S38-B11  S38-B2  S38-B6  S38-B7  S38-B8 
#&gt;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;01&quot;    &quot;02&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;  S38-B9  S38-C1 S38-C10 S38-C11  S38-C3  S38-C4  S38-C5  S38-C6  S38-C7  S38-C9  S38-D1 S38-D10 
#&gt;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot; 
#&gt; S38-D11 S38-D12  S38-D2  S38-D4  S38-D5  S38-D6  S38-D7  S38-D8  S38-D9  S38-E1  S38-E2  S38-E3 
#&gt;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;03&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot; 
#&gt;  S38-E4  S38-E5  S38-E6  S38-E7  S38-E8  S38-E9 S38-F10 S38-F11  S38-F2  S38-F3  S38-F5  S38-F6 
#&gt;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot; 
#&gt;  S38-F7  S38-F8  S38-F9 S38-G10 S38-G12  S38-G4  S38-G5  S38-G6  S38-G7  S38-G8  S38-G9  S38-H1 
#&gt;    &quot;03&quot;    &quot;02&quot;    &quot;01&quot;    &quot;01&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;02&quot;    &quot;03&quot;    &quot;03&quot;    &quot;03&quot;    &quot;01&quot; 
#&gt; S38-H11  S38-H2  S38-H3  S38-H4  S38-H5  S38-H6  S38-H8 
#&gt;    &quot;01&quot;    &quot;02&quot;    &quot;03&quot;    &quot;01&quot;    &quot;02&quot;    &quot;02&quot;    &quot;01&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-4-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-4-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>



### Top rows heatmap

Heatmaps of the top rows:





```r
top_rows_heatmap(res_rh)
```

![plot of chunk top-rows-heatmap](figure_cola/top-rows-heatmap-1.png)

Top rows on each node:


```r
top_rows_overlap(res_rh, method = "upset")
```

![plot of chunk top-rows-overlap](figure_cola/top-rows-overlap-1.png)


### UMAP plot

UMAP plot which shows how samples are separated.




<script>
$( function() {
	$( '#tabs-dimension-reduction-by-depth' ).tabs();
} );
</script>
<div id='tabs-dimension-reduction-by-depth'>
<ul>
<li><a href='#tab-dimension-reduction-by-depth-1'>n_signatures ≥ 329</a></li>
<li><a href='#tab-dimension-reduction-by-depth-2'>n_signatures ≥ 597</a></li>
<li><a href='#tab-dimension-reduction-by-depth-3'>n_signatures ≥ 1046</a></li>
<li><a href='#tab-dimension-reduction-by-depth-4'>n_signatures ≥ 4074</a></li>
</ul>
<div id='tab-dimension-reduction-by-depth-1'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 329),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1000, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 329),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1000, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-1-1.png" title="plot of chunk tab-dimension-reduction-by-depth-1" alt="plot of chunk tab-dimension-reduction-by-depth-1" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-2'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 597),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1000, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 597),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1000, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-2-1.png" title="plot of chunk tab-dimension-reduction-by-depth-2" alt="plot of chunk tab-dimension-reduction-by-depth-2" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-3'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 1046),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1000, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 1046),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1000, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-3-1.png" title="plot of chunk tab-dimension-reduction-by-depth-3" alt="plot of chunk tab-dimension-reduction-by-depth-3" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-4'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 4074),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1000, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 4074),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1000, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-4-1.png" title="plot of chunk tab-dimension-reduction-by-depth-4" alt="plot of chunk tab-dimension-reduction-by-depth-4" width="100%" /></p>

</div>
</div>




### Signature heatmap

Signatures on the heatmap are the union of all signatures found on every node
on the hierarchy. The number of k-means on rows are automatically selected by the function.




<script>
$( function() {
	$( '#tabs-get-signatures-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-get-signatures-from-hierarchical-partition'>
<ul>
<li><a href='#tab-get-signatures-from-hierarchical-partition-1'>n_signatures ≥ 329</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-2'>n_signatures ≥ 597</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-3'>n_signatures ≥ 1046</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-4'>n_signatures ≥ 4074</a></li>
</ul>
<div id='tab-get-signatures-from-hierarchical-partition-1'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 329))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-1-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-1"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-2'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 597))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-2-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-2"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-3'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 1046))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-3-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-3"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-4'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 4074))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-4-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-4"/></p>

</div>
</div>




Compare signatures from different nodes:


```r
compare_signatures(res_rh, verbose = FALSE)
```

![plot of chunk unnamed-chunk-24](figure_cola/unnamed-chunk-24-1.png)

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs. Note it only works on every node and the final signatures
are the union of all signatures of all nodes.


```r
# code only for demonstration
# e.g. to show the top 500 most significant rows on each node.
tb = get_signature(res_rh, top_signatures = 500)
```


### Test to known annotations

Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.




<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-hierarchical-partition'>
<ul>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-1'>n_signatures ≥ 329</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-2'>n_signatures ≥ 597</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-3'>n_signatures ≥ 1046</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-4'>n_signatures ≥ 4074</a></li>
</ul>
<div id='tab-test-to-known-factors-from-hierarchical-partition-1'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 329))
</code></pre>

<pre><code>#&gt;            Age Inferred.Cell.Type
#&gt; class 6.34e-09          1.03e-117
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-2'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 597))
</code></pre>

<pre><code>#&gt;            Age Inferred.Cell.Type
#&gt; class 5.21e-08          2.42e-119
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-3'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 1046))
</code></pre>

<pre><code>#&gt;            Age Inferred.Cell.Type
#&gt; class 2.87e-08          2.67e-119
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-4'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 4074))
</code></pre>

<pre><code>#&gt;            Age Inferred.Cell.Type
#&gt; class 0.000943           1.49e-80
</code></pre>

</div>
</div>



## Results for each node


---------------------------------------------------




### Node0


Child nodes: 
                [Node01](#Node01)
        ,
                [Node02](#Node02)
        ,
                [Node03](#Node03)
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["0"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 9277 rows and 367 columns.
#>   Top rows (928) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-0-collect-plots](figure_cola/node-0-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-0-select-partition-number](figure_cola/node-0-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.987       0.995         0.4332 0.566   0.566
#> 3 3 1.000           0.977       0.990         0.5409 0.751   0.567
#> 4 4 0.978           0.946       0.960         0.0779 0.943   0.829
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-0-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-0-get-classes'>
<ul>
<li><a href='#tab-node-0-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-0-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-0-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-0-get-classes-1'>
<p><a id='tab-node-0-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2
#&gt; O10-A1      2   0.000      0.990 0.00 1.00
#&gt; O10-A11     1   0.000      0.997 1.00 0.00
#&gt; O10-A12     2   0.000      0.990 0.00 1.00
#&gt; O10-A3      1   0.000      0.997 1.00 0.00
#&gt; O10-A4      2   0.000      0.990 0.00 1.00
#&gt; O10-A5      1   0.000      0.997 1.00 0.00
#&gt; O10-A7      1   0.000      0.997 1.00 0.00
#&gt; O10-A8      1   0.000      0.997 1.00 0.00
#&gt; O10-A9      1   0.000      0.997 1.00 0.00
#&gt; O10-B1      1   0.000      0.997 1.00 0.00
#&gt; O10-B10     1   0.000      0.997 1.00 0.00
#&gt; O10-B11     1   0.000      0.997 1.00 0.00
#&gt; O10-B12     1   0.000      0.997 1.00 0.00
#&gt; O10-B2      1   0.000      0.997 1.00 0.00
#&gt; O10-B5      1   0.000      0.997 1.00 0.00
#&gt; O10-B6      1   0.000      0.997 1.00 0.00
#&gt; O10-B7      1   0.000      0.997 1.00 0.00
#&gt; O10-B9      1   0.000      0.997 1.00 0.00
#&gt; O10-C10     1   0.000      0.997 1.00 0.00
#&gt; O10-C11     1   0.000      0.997 1.00 0.00
#&gt; O10-C12     1   0.000      0.997 1.00 0.00
#&gt; O10-C2      1   0.000      0.997 1.00 0.00
#&gt; O10-C4      2   0.000      0.990 0.00 1.00
#&gt; O10-C5      1   0.000      0.997 1.00 0.00
#&gt; O10-C6      1   0.000      0.997 1.00 0.00
#&gt; O10-C9      1   0.000      0.997 1.00 0.00
#&gt; O10-D1      2   0.855      0.617 0.28 0.72
#&gt; O10-D11     1   0.000      0.997 1.00 0.00
#&gt; O10-D12     1   0.000      0.997 1.00 0.00
#&gt; O10-D2      1   0.000      0.997 1.00 0.00
#&gt; O10-D3      2   0.000      0.990 0.00 1.00
#&gt; O10-D5      1   0.000      0.997 1.00 0.00
#&gt; O10-D6      1   0.000      0.997 1.00 0.00
#&gt; O10-D8      1   0.000      0.997 1.00 0.00
#&gt; O10-D9      1   0.000      0.997 1.00 0.00
#&gt; O10-E1      1   0.000      0.997 1.00 0.00
#&gt; O10-E11     1   0.000      0.997 1.00 0.00
#&gt; O10-E12     1   0.000      0.997 1.00 0.00
#&gt; O10-E6      1   0.000      0.997 1.00 0.00
#&gt; O10-E8      1   0.000      0.997 1.00 0.00
#&gt; O10-E9      1   0.000      0.997 1.00 0.00
#&gt; O10-F1      1   0.000      0.997 1.00 0.00
#&gt; O10-F11     1   0.000      0.997 1.00 0.00
#&gt; O10-F12     1   0.000      0.997 1.00 0.00
#&gt; O10-F2      1   0.000      0.997 1.00 0.00
#&gt; O10-F4      1   0.000      0.997 1.00 0.00
#&gt; O10-F5      1   0.000      0.997 1.00 0.00
#&gt; O10-F6      2   0.000      0.990 0.00 1.00
#&gt; O10-F8      1   0.000      0.997 1.00 0.00
#&gt; O10-F9      1   0.000      0.997 1.00 0.00
#&gt; O10-G1      1   0.000      0.997 1.00 0.00
#&gt; O10-G12     1   0.000      0.997 1.00 0.00
#&gt; O10-G2      1   0.000      0.997 1.00 0.00
#&gt; O10-G3      1   0.925      0.479 0.66 0.34
#&gt; O10-G4      1   0.000      0.997 1.00 0.00
#&gt; O10-G5      1   0.000      0.997 1.00 0.00
#&gt; O10-G7      1   0.000      0.997 1.00 0.00
#&gt; O10-H1      1   0.000      0.997 1.00 0.00
#&gt; O10-H4      1   0.000      0.997 1.00 0.00
#&gt; O10-H5      1   0.000      0.997 1.00 0.00
#&gt; O10-H6      1   0.000      0.997 1.00 0.00
#&gt; O10-H7      1   0.000      0.997 1.00 0.00
#&gt; O12-A1      2   0.000      0.990 0.00 1.00
#&gt; O12-A12     2   0.000      0.990 0.00 1.00
#&gt; O12-A2      1   0.000      0.997 1.00 0.00
#&gt; O12-A3      2   0.000      0.990 0.00 1.00
#&gt; O12-B1      2   0.000      0.990 0.00 1.00
#&gt; O12-B12     1   0.000      0.997 1.00 0.00
#&gt; O12-C1      2   0.000      0.990 0.00 1.00
#&gt; O12-D1      2   0.000      0.990 0.00 1.00
#&gt; O12-D2      1   0.000      0.997 1.00 0.00
#&gt; O12-E1      1   0.000      0.997 1.00 0.00
#&gt; O12-E12     2   0.000      0.990 0.00 1.00
#&gt; O12-F12     1   0.000      0.997 1.00 0.00
#&gt; O12-F2      2   0.000      0.990 0.00 1.00
#&gt; O12-G2      2   0.000      0.990 0.00 1.00
#&gt; O12-H1      2   0.000      0.990 0.00 1.00
#&gt; O12-H2      2   0.000      0.990 0.00 1.00
#&gt; O7-A3       1   0.000      0.997 1.00 0.00
#&gt; O7-A5       2   0.000      0.990 0.00 1.00
#&gt; O7-A7       2   0.000      0.990 0.00 1.00
#&gt; O7-A8       2   0.000      0.990 0.00 1.00
#&gt; O7-A9       1   0.000      0.997 1.00 0.00
#&gt; O7-B1       2   0.000      0.990 0.00 1.00
#&gt; O7-B11      1   0.000      0.997 1.00 0.00
#&gt; O7-B12      2   0.000      0.990 0.00 1.00
#&gt; O7-B6       1   0.000      0.997 1.00 0.00
#&gt; O7-B7       1   0.000      0.997 1.00 0.00
#&gt; O7-B8       2   0.000      0.990 0.00 1.00
#&gt; O7-B9       2   0.000      0.990 0.00 1.00
#&gt; O7-C1       1   0.000      0.997 1.00 0.00
#&gt; O7-C11      1   0.000      0.997 1.00 0.00
#&gt; O7-C2       2   0.000      0.990 0.00 1.00
#&gt; O7-C3       2   0.000      0.990 0.00 1.00
#&gt; O7-C6       2   0.000      0.990 0.00 1.00
#&gt; O7-C7       1   0.000      0.997 1.00 0.00
#&gt; O7-D1       2   0.000      0.990 0.00 1.00
#&gt; O7-D11      2   0.000      0.990 0.00 1.00
#&gt; O7-D12      2   0.000      0.990 0.00 1.00
#&gt; O7-D2       2   0.000      0.990 0.00 1.00
#&gt; O7-D3       1   0.000      0.997 1.00 0.00
#&gt; O7-D4       1   0.000      0.997 1.00 0.00
#&gt; O7-D5       1   0.000      0.997 1.00 0.00
#&gt; O7-E10      1   0.000      0.997 1.00 0.00
#&gt; O7-E11      1   0.327      0.934 0.94 0.06
#&gt; O7-E5       2   0.000      0.990 0.00 1.00
#&gt; O7-E6       2   0.000      0.990 0.00 1.00
#&gt; O7-F1       2   0.000      0.990 0.00 1.00
#&gt; O7-F10      2   0.000      0.990 0.00 1.00
#&gt; O7-F11      2   0.000      0.990 0.00 1.00
#&gt; O7-F2       1   0.000      0.997 1.00 0.00
#&gt; O7-F3       1   0.000      0.997 1.00 0.00
#&gt; O7-F4       2   0.000      0.990 0.00 1.00
#&gt; O7-F7       1   0.000      0.997 1.00 0.00
#&gt; O7-F9       1   0.000      0.997 1.00 0.00
#&gt; O7-G12      2   0.000      0.990 0.00 1.00
#&gt; O7-G3       1   0.000      0.997 1.00 0.00
#&gt; O7-G9       2   0.000      0.990 0.00 1.00
#&gt; O7-H3       2   0.000      0.990 0.00 1.00
#&gt; O7-H4       2   0.000      0.990 0.00 1.00
#&gt; O7-H8       2   0.000      0.990 0.00 1.00
#&gt; O8-A2       1   0.000      0.997 1.00 0.00
#&gt; O8-A3       1   0.000      0.997 1.00 0.00
#&gt; O8-A5       2   0.000      0.990 0.00 1.00
#&gt; O8-A8       1   0.000      0.997 1.00 0.00
#&gt; O8-A9       2   0.000      0.990 0.00 1.00
#&gt; O8-B1       2   0.000      0.990 0.00 1.00
#&gt; O8-B2       1   0.000      0.997 1.00 0.00
#&gt; O8-B4       1   0.000      0.997 1.00 0.00
#&gt; O8-B6       1   0.000      0.997 1.00 0.00
#&gt; O8-B7       1   0.000      0.997 1.00 0.00
#&gt; O8-B9       1   0.000      0.997 1.00 0.00
#&gt; O8-C11      1   0.000      0.997 1.00 0.00
#&gt; O8-C12      1   0.000      0.997 1.00 0.00
#&gt; O8-C2       2   0.000      0.990 0.00 1.00
#&gt; O8-C3       1   0.000      0.997 1.00 0.00
#&gt; O8-C4       1   0.000      0.997 1.00 0.00
#&gt; O8-C5       1   0.141      0.977 0.98 0.02
#&gt; O8-C6       1   0.000      0.997 1.00 0.00
#&gt; O8-C8       1   0.000      0.997 1.00 0.00
#&gt; O8-D3       1   0.000      0.997 1.00 0.00
#&gt; O8-D5       1   0.000      0.997 1.00 0.00
#&gt; O8-D6       1   0.000      0.997 1.00 0.00
#&gt; O8-D9       1   0.000      0.997 1.00 0.00
#&gt; O8-E1       1   0.000      0.997 1.00 0.00
#&gt; O8-E11      1   0.000      0.997 1.00 0.00
#&gt; O8-E2       1   0.000      0.997 1.00 0.00
#&gt; O8-E3       2   0.000      0.990 0.00 1.00
#&gt; O8-E5       1   0.000      0.997 1.00 0.00
#&gt; O8-E7       2   0.000      0.990 0.00 1.00
#&gt; O8-E9       1   0.000      0.997 1.00 0.00
#&gt; O8-F10      1   0.000      0.997 1.00 0.00
#&gt; O8-F11      1   0.000      0.997 1.00 0.00
#&gt; O8-F12      1   0.000      0.997 1.00 0.00
#&gt; O8-F2       2   0.000      0.990 0.00 1.00
#&gt; O8-F3       1   0.000      0.997 1.00 0.00
#&gt; O8-F6       2   0.000      0.990 0.00 1.00
#&gt; O8-F8       2   0.000      0.990 0.00 1.00
#&gt; O8-F9       1   0.000      0.997 1.00 0.00
#&gt; O8-G1       1   0.000      0.997 1.00 0.00
#&gt; O8-G12      1   0.000      0.997 1.00 0.00
#&gt; O8-G5       1   0.000      0.997 1.00 0.00
#&gt; O8-G9       1   0.000      0.997 1.00 0.00
#&gt; O8-H1       1   0.000      0.997 1.00 0.00
#&gt; O8-H11      2   0.000      0.990 0.00 1.00
#&gt; O8-H2       2   0.000      0.990 0.00 1.00
#&gt; O8-H6       1   0.000      0.997 1.00 0.00
#&gt; O9-A10      1   0.000      0.997 1.00 0.00
#&gt; O9-A12      2   0.000      0.990 0.00 1.00
#&gt; O9-A2       1   0.000      0.997 1.00 0.00
#&gt; O9-A3       2   0.000      0.990 0.00 1.00
#&gt; O9-A5       2   0.000      0.990 0.00 1.00
#&gt; O9-A6       2   0.000      0.990 0.00 1.00
#&gt; O9-A8       1   0.000      0.997 1.00 0.00
#&gt; O9-A9       1   0.000      0.997 1.00 0.00
#&gt; O9-B10      1   0.000      0.997 1.00 0.00
#&gt; O9-B11      1   0.000      0.997 1.00 0.00
#&gt; O9-B12      1   0.000      0.997 1.00 0.00
#&gt; O9-B2       1   0.000      0.997 1.00 0.00
#&gt; O9-B3       1   0.000      0.997 1.00 0.00
#&gt; O9-B5       2   0.000      0.990 0.00 1.00
#&gt; O9-B6       1   0.000      0.997 1.00 0.00
#&gt; O9-B7       1   0.000      0.997 1.00 0.00
#&gt; O9-B8       2   0.000      0.990 0.00 1.00
#&gt; O9-C10      1   0.000      0.997 1.00 0.00
#&gt; O9-C2       1   0.000      0.997 1.00 0.00
#&gt; O9-C3       1   0.000      0.997 1.00 0.00
#&gt; O9-C4       1   0.141      0.977 0.98 0.02
#&gt; O9-C5       1   0.000      0.997 1.00 0.00
#&gt; O9-C6       1   0.000      0.997 1.00 0.00
#&gt; O9-C9       1   0.000      0.997 1.00 0.00
#&gt; O9-D1       1   0.000      0.997 1.00 0.00
#&gt; O9-D10      1   0.000      0.997 1.00 0.00
#&gt; O9-D11      1   0.000      0.997 1.00 0.00
#&gt; O9-D12      2   0.925      0.492 0.34 0.66
#&gt; O9-D2       2   0.000      0.990 0.00 1.00
#&gt; O9-D5       1   0.000      0.997 1.00 0.00
#&gt; O9-D6       1   0.000      0.997 1.00 0.00
#&gt; O9-D7       2   0.000      0.990 0.00 1.00
#&gt; O9-D8       2   0.000      0.990 0.00 1.00
#&gt; O9-E1       2   0.000      0.990 0.00 1.00
#&gt; O9-E10      1   0.000      0.997 1.00 0.00
#&gt; O9-E11      1   0.000      0.997 1.00 0.00
#&gt; O9-E12      1   0.000      0.997 1.00 0.00
#&gt; O9-E2       2   0.000      0.990 0.00 1.00
#&gt; O9-E4       1   0.000      0.997 1.00 0.00
#&gt; O9-E5       1   0.000      0.997 1.00 0.00
#&gt; O9-E7       1   0.000      0.997 1.00 0.00
#&gt; O9-E8       1   0.000      0.997 1.00 0.00
#&gt; O9-E9       1   0.000      0.997 1.00 0.00
#&gt; O9-F1       2   0.000      0.990 0.00 1.00
#&gt; O9-F10      1   0.000      0.997 1.00 0.00
#&gt; O9-F12      1   0.000      0.997 1.00 0.00
#&gt; O9-F2       1   0.000      0.997 1.00 0.00
#&gt; O9-F4       2   0.000      0.990 0.00 1.00
#&gt; O9-F5       2   0.000      0.990 0.00 1.00
#&gt; O9-F9       1   0.000      0.997 1.00 0.00
#&gt; O9-G10      1   0.000      0.997 1.00 0.00
#&gt; O9-G12      1   0.000      0.997 1.00 0.00
#&gt; O9-G2       1   0.000      0.997 1.00 0.00
#&gt; O9-G3       1   0.000      0.997 1.00 0.00
#&gt; O9-G4       1   0.000      0.997 1.00 0.00
#&gt; O9-G6       1   0.000      0.997 1.00 0.00
#&gt; O9-G7       1   0.000      0.997 1.00 0.00
#&gt; O9-G9       2   0.000      0.990 0.00 1.00
#&gt; O9-H12      2   0.000      0.990 0.00 1.00
#&gt; O9-H3       1   0.000      0.997 1.00 0.00
#&gt; O9-H5       2   0.000      0.990 0.00 1.00
#&gt; O9-H9       1   0.000      0.997 1.00 0.00
#&gt; S37-A1      1   0.000      0.997 1.00 0.00
#&gt; S37-A10     1   0.000      0.997 1.00 0.00
#&gt; S37-A2      2   0.000      0.990 0.00 1.00
#&gt; S37-A3      1   0.000      0.997 1.00 0.00
#&gt; S37-A4      1   0.000      0.997 1.00 0.00
#&gt; S37-A5      2   0.000      0.990 0.00 1.00
#&gt; S37-A6      2   0.000      0.990 0.00 1.00
#&gt; S37-A7      1   0.000      0.997 1.00 0.00
#&gt; S37-A8      2   0.000      0.990 0.00 1.00
#&gt; S37-A9      1   0.000      0.997 1.00 0.00
#&gt; S37-B1      1   0.000      0.997 1.00 0.00
#&gt; S37-B10     1   0.000      0.997 1.00 0.00
#&gt; S37-B11     2   0.000      0.990 0.00 1.00
#&gt; S37-B12     2   0.000      0.990 0.00 1.00
#&gt; S37-B2      1   0.000      0.997 1.00 0.00
#&gt; S37-B3      1   0.000      0.997 1.00 0.00
#&gt; S37-B4      1   0.000      0.997 1.00 0.00
#&gt; S37-B5      1   0.000      0.997 1.00 0.00
#&gt; S37-B6      1   0.000      0.997 1.00 0.00
#&gt; S37-B7      1   0.000      0.997 1.00 0.00
#&gt; S37-B9      1   0.000      0.997 1.00 0.00
#&gt; S37-C10     1   0.000      0.997 1.00 0.00
#&gt; S37-C12     2   0.000      0.990 0.00 1.00
#&gt; S37-C3      1   0.000      0.997 1.00 0.00
#&gt; S37-C4      1   0.000      0.997 1.00 0.00
#&gt; S37-C6      1   0.000      0.997 1.00 0.00
#&gt; S37-C7      1   0.000      0.997 1.00 0.00
#&gt; S37-C8      2   0.000      0.990 0.00 1.00
#&gt; S37-C9      1   0.000      0.997 1.00 0.00
#&gt; S37-D10     1   0.000      0.997 1.00 0.00
#&gt; S37-D11     1   0.000      0.997 1.00 0.00
#&gt; S37-D12     2   0.000      0.990 0.00 1.00
#&gt; S37-D2      1   0.000      0.997 1.00 0.00
#&gt; S37-D4      1   0.000      0.997 1.00 0.00
#&gt; S37-D6      1   0.000      0.997 1.00 0.00
#&gt; S37-D8      1   0.000      0.997 1.00 0.00
#&gt; S37-D9      1   0.000      0.997 1.00 0.00
#&gt; S37-E1      1   0.000      0.997 1.00 0.00
#&gt; S37-E10     1   0.000      0.997 1.00 0.00
#&gt; S37-E11     1   0.000      0.997 1.00 0.00
#&gt; S37-E2      1   0.000      0.997 1.00 0.00
#&gt; S37-E3      1   0.000      0.997 1.00 0.00
#&gt; S37-E5      1   0.000      0.997 1.00 0.00
#&gt; S37-E6      1   0.000      0.997 1.00 0.00
#&gt; S37-E7      2   0.000      0.990 0.00 1.00
#&gt; S37-E8      2   0.000      0.990 0.00 1.00
#&gt; S37-E9      1   0.000      0.997 1.00 0.00
#&gt; S37-F1      1   0.000      0.997 1.00 0.00
#&gt; S37-F10     1   0.000      0.997 1.00 0.00
#&gt; S37-F12     1   0.000      0.997 1.00 0.00
#&gt; S37-F2      1   0.000      0.997 1.00 0.00
#&gt; S37-F3      1   0.000      0.997 1.00 0.00
#&gt; S37-F5      1   0.000      0.997 1.00 0.00
#&gt; S37-F7      1   0.000      0.997 1.00 0.00
#&gt; S37-F9      1   0.000      0.997 1.00 0.00
#&gt; S37-G1      1   0.000      0.997 1.00 0.00
#&gt; S37-G10     1   0.000      0.997 1.00 0.00
#&gt; S37-G12     1   0.000      0.997 1.00 0.00
#&gt; S37-G2      2   0.000      0.990 0.00 1.00
#&gt; S37-G3      2   0.722      0.751 0.20 0.80
#&gt; S37-G4      2   0.000      0.990 0.00 1.00
#&gt; S37-G5      1   0.000      0.997 1.00 0.00
#&gt; S37-G6      1   0.000      0.997 1.00 0.00
#&gt; S37-G7      1   0.000      0.997 1.00 0.00
#&gt; S37-G8      1   0.000      0.997 1.00 0.00
#&gt; S37-H2      1   0.000      0.997 1.00 0.00
#&gt; S37-H4      1   0.000      0.997 1.00 0.00
#&gt; S37-H7      1   0.000      0.997 1.00 0.00
#&gt; S37-H8      1   0.000      0.997 1.00 0.00
#&gt; S37-H9      1   0.000      0.997 1.00 0.00
#&gt; S38-A1      1   0.000      0.997 1.00 0.00
#&gt; S38-A10     1   0.000      0.997 1.00 0.00
#&gt; S38-A12     2   0.000      0.990 0.00 1.00
#&gt; S38-A2      2   0.000      0.990 0.00 1.00
#&gt; S38-A3      1   0.000      0.997 1.00 0.00
#&gt; S38-A5      2   0.000      0.990 0.00 1.00
#&gt; S38-A9      2   0.000      0.990 0.00 1.00
#&gt; S38-B10     1   0.000      0.997 1.00 0.00
#&gt; S38-B11     1   0.000      0.997 1.00 0.00
#&gt; S38-B2      2   0.000      0.990 0.00 1.00
#&gt; S38-B6      1   0.000      0.997 1.00 0.00
#&gt; S38-B7      2   0.000      0.990 0.00 1.00
#&gt; S38-B8      2   0.000      0.990 0.00 1.00
#&gt; S38-B9      2   0.000      0.990 0.00 1.00
#&gt; S38-C1      1   0.904      0.524 0.68 0.32
#&gt; S38-C10     1   0.000      0.997 1.00 0.00
#&gt; S38-C11     1   0.000      0.997 1.00 0.00
#&gt; S38-C3      2   0.000      0.990 0.00 1.00
#&gt; S38-C4      1   0.000      0.997 1.00 0.00
#&gt; S38-C5      1   0.000      0.997 1.00 0.00
#&gt; S38-C6      1   0.000      0.997 1.00 0.00
#&gt; S38-C7      1   0.000      0.997 1.00 0.00
#&gt; S38-C9      2   0.000      0.990 0.00 1.00
#&gt; S38-D1      2   0.000      0.990 0.00 1.00
#&gt; S38-D10     1   0.000      0.997 1.00 0.00
#&gt; S38-D11     1   0.000      0.997 1.00 0.00
#&gt; S38-D12     1   0.000      0.997 1.00 0.00
#&gt; S38-D2      1   0.000      0.997 1.00 0.00
#&gt; S38-D4      2   0.958      0.394 0.38 0.62
#&gt; S38-D5      2   0.000      0.990 0.00 1.00
#&gt; S38-D6      2   0.000      0.990 0.00 1.00
#&gt; S38-D7      1   0.000      0.997 1.00 0.00
#&gt; S38-D8      1   0.000      0.997 1.00 0.00
#&gt; S38-D9      1   0.000      0.997 1.00 0.00
#&gt; S38-E1      2   0.000      0.990 0.00 1.00
#&gt; S38-E2      1   0.000      0.997 1.00 0.00
#&gt; S38-E3      1   0.000      0.997 1.00 0.00
#&gt; S38-E4      2   0.000      0.990 0.00 1.00
#&gt; S38-E5      2   0.000      0.990 0.00 1.00
#&gt; S38-E6      2   0.000      0.990 0.00 1.00
#&gt; S38-E7      2   0.000      0.990 0.00 1.00
#&gt; S38-E8      1   0.000      0.997 1.00 0.00
#&gt; S38-E9      2   0.000      0.990 0.00 1.00
#&gt; S38-F10     1   0.000      0.997 1.00 0.00
#&gt; S38-F11     1   0.000      0.997 1.00 0.00
#&gt; S38-F2      2   0.000      0.990 0.00 1.00
#&gt; S38-F3      1   0.000      0.997 1.00 0.00
#&gt; S38-F5      2   0.000      0.990 0.00 1.00
#&gt; S38-F6      2   0.000      0.990 0.00 1.00
#&gt; S38-F7      1   0.000      0.997 1.00 0.00
#&gt; S38-F8      2   0.000      0.990 0.00 1.00
#&gt; S38-F9      1   0.000      0.997 1.00 0.00
#&gt; S38-G10     1   0.000      0.997 1.00 0.00
#&gt; S38-G12     1   0.000      0.997 1.00 0.00
#&gt; S38-G4      2   0.000      0.990 0.00 1.00
#&gt; S38-G5      2   0.000      0.990 0.00 1.00
#&gt; S38-G6      2   0.000      0.990 0.00 1.00
#&gt; S38-G7      1   0.000      0.997 1.00 0.00
#&gt; S38-G8      1   0.000      0.997 1.00 0.00
#&gt; S38-G9      1   0.000      0.997 1.00 0.00
#&gt; S38-H1      1   0.000      0.997 1.00 0.00
#&gt; S38-H11     1   0.000      0.997 1.00 0.00
#&gt; S38-H2      2   0.000      0.990 0.00 1.00
#&gt; S38-H3      1   0.000      0.997 1.00 0.00
#&gt; S38-H4      1   0.000      0.997 1.00 0.00
#&gt; S38-H5      2   0.000      0.990 0.00 1.00
#&gt; S38-H6      2   0.000      0.990 0.00 1.00
#&gt; S38-H8      1   0.000      0.997 1.00 0.00
</code></pre>

<script>
$('#tab-node-0-get-classes-1-a').parent().next().next().hide();
$('#tab-node-0-get-classes-1-a').click(function(){
  $('#tab-node-0-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-0-get-classes-2'>
<p><a id='tab-node-0-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3
#&gt; O10-A1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O10-A11     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-A12     2  0.0000      1.000 0.00 1.00 0.00
#&gt; O10-A3      1  0.1529      0.950 0.96 0.00 0.04
#&gt; O10-A4      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O10-A5      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-A7      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-A8      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-A9      1  0.4555      0.750 0.80 0.00 0.20
#&gt; O10-B1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-B10     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-B11     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-B12     1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-B2      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-B5      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-B6      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-B7      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-B9      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-C10     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-C11     1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-C12     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-C2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-C4      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O10-C5      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-C6      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-C9      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-D1      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-D11     3  0.0892      0.965 0.02 0.00 0.98
#&gt; O10-D12     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-D2      1  0.5216      0.646 0.74 0.00 0.26
#&gt; O10-D3      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O10-D5      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-D6      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-D8      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-D9      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-E1      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-E11     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-E12     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-E6      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-E8      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-E9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-F1      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-F11     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-F12     3  0.0892      0.965 0.02 0.00 0.98
#&gt; O10-F2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-F4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-F5      3  0.1529      0.948 0.04 0.00 0.96
#&gt; O10-F6      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O10-F8      1  0.3686      0.834 0.86 0.00 0.14
#&gt; O10-F9      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O10-G1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-G12     1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-G2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-G3      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-G4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-G5      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-G7      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-H1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-H4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-H5      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-H6      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O10-H7      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O12-A1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-A12     2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-A2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O12-A3      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-B1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-B12     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O12-C1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-D1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-D2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O12-E1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O12-E12     2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-F12     3  0.0000      0.981 0.00 0.00 1.00
#&gt; O12-F2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-G2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-H1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O12-H2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-A3       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-A5       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-A7       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-A8       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-A9       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-B1       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-B11      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-B12      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-B6       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-B7       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-B8       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-B9       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-C1       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-C11      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-C2       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-C3       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-C6       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-C7       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-D1       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-D11      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-D12      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-D2       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-D3       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O7-D4       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-D5       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O7-E10      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-E11      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-E5       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-E6       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-F1       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-F10      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-F11      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-F2       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-F3       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-F4       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-F7       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-F9       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O7-G12      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-G3       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O7-G9       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-H3       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-H4       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O7-H8       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-A2       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-A3       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-A5       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-A8       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-A9       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-B1       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-B2       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-B4       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-B6       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-B7       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-B9       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-C11      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-C12      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-C2       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-C3       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-C4       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-C5       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-C6       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-C8       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-D3       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-D5       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-D6       3  0.4291      0.789 0.18 0.00 0.82
#&gt; O8-D9       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-E1       1  0.4796      0.716 0.78 0.00 0.22
#&gt; O8-E11      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-E2       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-E3       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-E5       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-E7       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-E9       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-F10      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O8-F11      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-F12      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-F2       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-F3       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-F6       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-F8       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-F9       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O8-G1       3  0.2959      0.890 0.10 0.00 0.90
#&gt; O8-G12      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-G5       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O8-G9       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O8-H1       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O8-H11      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-H2       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O8-H6       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-A10      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-A12      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-A2       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-A3       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-A5       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-A6       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-A8       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-A9       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-B10      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-B11      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-B12      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-B2       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-B3       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-B5       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-B6       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-B7       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-B8       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-C10      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-C2       3  0.3340      0.867 0.12 0.00 0.88
#&gt; O9-C3       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-C4       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-C5       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-C6       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-C9       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-D1       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-D10      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-D11      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-D12      3  0.1529      0.946 0.00 0.04 0.96
#&gt; O9-D2       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-D5       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-D6       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-D7       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-D8       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-E1       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-E10      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-E11      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-E12      3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-E2       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-E4       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-E5       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-E7       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-E8       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-E9       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-F1       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-F10      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-F12      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-F2       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-F4       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-F5       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-F9       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-G10      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-G12      1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-G2       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-G3       3  0.5560      0.587 0.30 0.00 0.70
#&gt; O9-G4       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-G6       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-G7       3  0.0000      0.981 0.00 0.00 1.00
#&gt; O9-G9       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-H12      2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-H3       1  0.0000      0.989 1.00 0.00 0.00
#&gt; O9-H5       2  0.0000      1.000 0.00 1.00 0.00
#&gt; O9-H9       1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-A1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-A10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-A2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-A3      1  0.0892      0.970 0.98 0.00 0.02
#&gt; S37-A4      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-A5      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-A6      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-A7      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-A8      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-A9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-B1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-B10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-B11     2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-B12     2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-B2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-B3      3  0.5397      0.627 0.28 0.00 0.72
#&gt; S37-B4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-B5      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-B6      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-B7      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-B9      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-C10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-C12     2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-C3      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-C4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-C6      1  0.6126      0.321 0.60 0.00 0.40
#&gt; S37-C7      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-C8      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-C9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-D10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-D11     3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-D12     2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-D2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-D4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-D6      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-D8      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-D9      3  0.4291      0.789 0.18 0.00 0.82
#&gt; S37-E1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-E10     3  0.5397      0.625 0.28 0.00 0.72
#&gt; S37-E11     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-E2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-E3      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-E5      3  0.1529      0.948 0.04 0.00 0.96
#&gt; S37-E6      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-E7      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-E8      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-E9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-F1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-F10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-F12     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-F2      1  0.0892      0.970 0.98 0.00 0.02
#&gt; S37-F3      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-F5      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-F7      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-F9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-G1      1  0.2959      0.885 0.90 0.00 0.10
#&gt; S37-G10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-G12     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-G2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-G3      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-G4      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S37-G5      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-G6      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-G7      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-G8      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S37-H2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-H4      3  0.2959      0.889 0.10 0.00 0.90
#&gt; S37-H7      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-H8      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S37-H9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-A1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-A10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-A12     2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-A2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-A3      3  0.1529      0.948 0.04 0.00 0.96
#&gt; S38-A5      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-A9      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-B10     3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-B11     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-B2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-B6      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-B7      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-B8      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-B9      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-C1      3  0.2066      0.927 0.00 0.06 0.94
#&gt; S38-C10     3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-C11     3  0.2959      0.889 0.10 0.00 0.90
#&gt; S38-C3      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-C4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-C5      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-C6      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-C7      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-C9      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-D1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-D10     3  0.2959      0.889 0.10 0.00 0.90
#&gt; S38-D11     3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-D12     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-D2      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-D4      3  0.4291      0.780 0.00 0.18 0.82
#&gt; S38-D5      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-D6      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-D7      3  0.4291      0.789 0.18 0.00 0.82
#&gt; S38-D8      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-D9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-E1      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-E2      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-E3      3  0.0892      0.965 0.02 0.00 0.98
#&gt; S38-E4      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-E5      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-E6      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-E7      2  0.1529      0.958 0.00 0.96 0.04
#&gt; S38-E8      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-E9      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-F10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-F11     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-F2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-F3      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-F5      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-F6      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-F7      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-F8      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-F9      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-G10     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-G12     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-G4      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-G5      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-G6      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-G7      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-G8      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-G9      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-H1      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-H11     1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-H2      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-H3      3  0.0000      0.981 0.00 0.00 1.00
#&gt; S38-H4      1  0.0000      0.989 1.00 0.00 0.00
#&gt; S38-H5      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-H6      2  0.0000      1.000 0.00 1.00 0.00
#&gt; S38-H8      1  0.0000      0.989 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-0-get-classes-2-a').parent().next().next().hide();
$('#tab-node-0-get-classes-2-a').click(function(){
  $('#tab-node-0-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-0-get-classes-3'>
<p><a id='tab-node-0-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3   p4
#&gt; O10-A1      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O10-A11     3  0.1411      0.945 0.02 0.00 0.96 0.02
#&gt; O10-A12     4  0.2011      0.975 0.00 0.08 0.00 0.92
#&gt; O10-A3      1  0.1411      0.937 0.96 0.00 0.02 0.02
#&gt; O10-A4      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O10-A5      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-A7      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-A8      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O10-A9      1  0.4936      0.563 0.70 0.00 0.28 0.02
#&gt; O10-B1      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O10-B10     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-B11     3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O10-B12     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O10-B2      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-B5      3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O10-B6      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-B7      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-B9      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-C10     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-C11     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O10-C12     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-C2      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O10-C4      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O10-C5      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-C6      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-C9      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-D1      3  0.4790      0.366 0.00 0.38 0.62 0.00
#&gt; O10-D11     3  0.2335      0.925 0.06 0.00 0.92 0.02
#&gt; O10-D12     3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O10-D2      1  0.4642      0.640 0.74 0.00 0.24 0.02
#&gt; O10-D3      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O10-D5      3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O10-D6      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-D8      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O10-D9      3  0.1411      0.945 0.02 0.00 0.96 0.02
#&gt; O10-E1      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-E11     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-E12     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O10-E6      3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O10-E8      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O10-E9      1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O10-F1      3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O10-F11     3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O10-F12     3  0.2335      0.925 0.06 0.00 0.92 0.02
#&gt; O10-F2      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O10-F4      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-F5      3  0.2706      0.909 0.08 0.00 0.90 0.02
#&gt; O10-F6      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O10-F8      1  0.3606      0.801 0.84 0.00 0.14 0.02
#&gt; O10-F9      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O10-G1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-G12     1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-G2      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-G3      1  0.5713      0.475 0.62 0.34 0.00 0.04
#&gt; O10-G4      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-G5      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-G7      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-H1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-H4      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-H5      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-H6      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O10-H7      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O12-A1      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O12-A12     2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O12-A2      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O12-A3      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O12-B1      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O12-B12     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O12-C1      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O12-D1      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O12-D2      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O12-E1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O12-E12     4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O12-F12     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O12-F2      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O12-G2      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O12-H1      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O12-H2      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-A3       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-A5       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-A7       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-A8       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-A9       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-B1       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-B11      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-B12      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-B6       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-B7       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-B8       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-B9       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-C1       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-C11      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-C2       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-C3       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-C6       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-C7       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-D1       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-D11      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-D12      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-D2       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-D3       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O7-D4       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-D5       1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O7-E10      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-E11      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-E5       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-E6       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-F1       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-F10      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-F11      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-F2       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-F3       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-F4       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-F7       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-F9       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O7-G12      4  0.2011      0.975 0.00 0.08 0.00 0.92
#&gt; O7-G3       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O7-G9       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-H3       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O7-H4       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O7-H8       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O8-A2       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-A3       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-A5       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O8-A8       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-A9       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O8-B1       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O8-B2       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-B4       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-B6       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-B7       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-B9       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-C11      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-C12      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-C2       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O8-C3       3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O8-C4       3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O8-C5       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-C6       3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O8-C8       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-D3       3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O8-D5       3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O8-D6       3  0.4472      0.752 0.22 0.00 0.76 0.02
#&gt; O8-D9       3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O8-E1       1  0.4642      0.642 0.74 0.00 0.24 0.02
#&gt; O8-E11      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O8-E2       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-E3       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O8-E5       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-E7       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O8-E9       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-F10      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O8-F11      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-F12      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-F2       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O8-F3       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-F6       4  0.2011      0.975 0.00 0.08 0.00 0.92
#&gt; O8-F8       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O8-F9       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O8-G1       3  0.3606      0.849 0.14 0.00 0.84 0.02
#&gt; O8-G12      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-G5       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O8-G9       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O8-H1       1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; O8-H11      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O8-H2       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O8-H6       1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; O9-A10      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O9-A12      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-A2       1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O9-A3       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-A5       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-A6       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-A8       3  0.1411      0.945 0.02 0.00 0.96 0.02
#&gt; O9-A9       1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O9-B10      1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O9-B11      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-B12      3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O9-B2       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O9-B3       1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O9-B5       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-B6       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O9-B7       3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O9-B8       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-C10      1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; O9-C2       3  0.3335      0.871 0.12 0.00 0.86 0.02
#&gt; O9-C3       1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-C4       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O9-C5       1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-C6       3  0.2335      0.925 0.06 0.00 0.92 0.02
#&gt; O9-C9       1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; O9-D1       1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-D10      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-D11      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-D12      2  0.4277      0.625 0.00 0.72 0.28 0.00
#&gt; O9-D2       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-D5       1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-D6       3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; O9-D7       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-D8       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-E1       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-E10      1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O9-E11      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-E12      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O9-E2       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-E4       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O9-E5       1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; O9-E7       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-E8       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-E9       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-F1       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-F10      1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O9-F12      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-F2       1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; O9-F4       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-F5       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-F9       3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; O9-G10      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-G12      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-G2       1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; O9-G3       3  0.5271      0.535 0.34 0.00 0.64 0.02
#&gt; O9-G4       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-G6       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; O9-G7       3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; O9-G9       2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; O9-H12      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-H3       1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; O9-H5       4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; O9-H9       1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-A1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-A10     1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; S37-A2      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S37-A3      1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; S37-A4      3  0.2335      0.926 0.06 0.00 0.92 0.02
#&gt; S37-A5      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S37-A6      4  0.2345      0.955 0.00 0.10 0.00 0.90
#&gt; S37-A7      3  0.1411      0.945 0.02 0.00 0.96 0.02
#&gt; S37-A8      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S37-A9      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-B1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-B10     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-B11     4  0.2011      0.975 0.00 0.08 0.00 0.92
#&gt; S37-B12     4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S37-B2      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-B3      3  0.5062      0.619 0.30 0.00 0.68 0.02
#&gt; S37-B4      1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; S37-B5      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S37-B6      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S37-B7      1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; S37-B9      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; S37-C10     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-C12     4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S37-C3      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-C4      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-C6      1  0.5173      0.466 0.66 0.00 0.32 0.02
#&gt; S37-C7      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-C8      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S37-C9      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-D10     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-D11     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; S37-D12     2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S37-D2      1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; S37-D4      1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; S37-D6      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-D8      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-D9      3  0.4284      0.776 0.20 0.00 0.78 0.02
#&gt; S37-E1      1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; S37-E10     3  0.5355      0.488 0.36 0.00 0.62 0.02
#&gt; S37-E11     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-E2      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-E3      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-E5      3  0.3853      0.827 0.16 0.00 0.82 0.02
#&gt; S37-E6      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-E7      4  0.4134      0.735 0.00 0.26 0.00 0.74
#&gt; S37-E8      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S37-E9      1  0.0707      0.964 0.98 0.00 0.00 0.02
#&gt; S37-F1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-F10     1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-F12     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-F2      1  0.1411      0.937 0.96 0.00 0.02 0.02
#&gt; S37-F3      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-F5      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-F7      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-F9      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-G1      1  0.3335      0.826 0.86 0.00 0.12 0.02
#&gt; S37-G10     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-G12     1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; S37-G2      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S37-G3      4  0.0707      0.917 0.02 0.00 0.00 0.98
#&gt; S37-G4      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S37-G5      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S37-G6      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-G7      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S37-G8      3  0.1411      0.945 0.02 0.00 0.96 0.02
#&gt; S37-H2      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-H4      3  0.3335      0.871 0.12 0.00 0.86 0.02
#&gt; S37-H7      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-H8      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S37-H9      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-A1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-A10     1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-A12     4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-A2      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-A3      3  0.2706      0.909 0.08 0.00 0.90 0.02
#&gt; S38-A5      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-A9      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-B10     3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-B11     1  0.1411      0.937 0.96 0.00 0.02 0.02
#&gt; S38-B2      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-B6      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-B7      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-B8      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-B9      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-C1      2  0.4907      0.293 0.00 0.58 0.42 0.00
#&gt; S38-C10     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; S38-C11     3  0.3853      0.827 0.16 0.00 0.82 0.02
#&gt; S38-C3      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-C4      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-C5      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-C6      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-C7      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-C9      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-D1      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-D10     3  0.4079      0.803 0.18 0.00 0.80 0.02
#&gt; S38-D11     3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; S38-D12     1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-D2      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S38-D4      2  0.2345      0.865 0.00 0.90 0.10 0.00
#&gt; S38-D5      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-D6      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-D7      3  0.4936      0.654 0.28 0.00 0.70 0.02
#&gt; S38-D8      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-D9      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S38-E1      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-E2      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-E3      3  0.3853      0.827 0.16 0.00 0.82 0.02
#&gt; S38-E4      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-E5      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-E6      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-E7      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-E8      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-E9      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-F10     1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S38-F11     1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; S38-F2      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-F3      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-F5      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-F6      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-F7      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-F8      4  0.2011      0.975 0.00 0.08 0.00 0.92
#&gt; S38-F9      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S38-G10     1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-G12     1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-G4      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-G5      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-G6      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-G7      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-G8      3  0.0000      0.954 0.00 0.00 1.00 0.00
#&gt; S38-G9      3  0.1913      0.937 0.04 0.00 0.94 0.02
#&gt; S38-H1      1  0.1211      0.963 0.96 0.00 0.00 0.04
#&gt; S38-H11     1  0.0707      0.952 0.98 0.00 0.00 0.02
#&gt; S38-H2      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-H3      3  0.0707      0.951 0.00 0.00 0.98 0.02
#&gt; S38-H4      1  0.0000      0.963 1.00 0.00 0.00 0.00
#&gt; S38-H5      4  0.1637      0.992 0.00 0.06 0.00 0.94
#&gt; S38-H6      2  0.0000      0.983 0.00 1.00 0.00 0.00
#&gt; S38-H8      1  0.0000      0.963 1.00 0.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-0-get-classes-3-a').parent().next().next().hide();
$('#tab-node-0-get-classes-3-a').click(function(){
  $('#tab-node-0-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-0-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-0-consensus-heatmap'>
<ul>
<li><a href='#tab-node-0-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-0-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-0-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-0-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-0-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-0-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-0-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-0-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-0-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-0-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-0-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-0-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-0-membership-heatmap'>
<ul>
<li><a href='#tab-node-0-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-0-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-0-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-0-membership-heatmap-1-1.png" alt="plot of chunk tab-node-0-membership-heatmap-1"/></p>

</div>
<div id='tab-node-0-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-0-membership-heatmap-2-1.png" alt="plot of chunk tab-node-0-membership-heatmap-2"/></p>

</div>
<div id='tab-node-0-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-0-membership-heatmap-3-1.png" alt="plot of chunk tab-node-0-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-0-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-0-get-signatures'>
<ul>
<li><a href='#tab-node-0-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-0-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-0-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-1-1.png" alt="plot of chunk tab-node-0-get-signatures-1"/></p>

</div>
<div id='tab-node-0-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-2-1.png" alt="plot of chunk tab-node-0-get-signatures-2"/></p>

</div>
<div id='tab-node-0-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-3-1.png" alt="plot of chunk tab-node-0-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-0-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-0-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-0-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-0-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-0-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-0-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-0-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-0-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-0-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-0-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-0-signature_compare](figure_cola/node-0-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-0-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-0-dimension-reduction'>
<ul>
<li><a href='#tab-node-0-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-0-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-0-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-0-dimension-reduction-1-1.png" alt="plot of chunk tab-node-0-dimension-reduction-1"/></p>

</div>
<div id='tab-node-0-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-0-dimension-reduction-2-1.png" alt="plot of chunk tab-node-0-dimension-reduction-2"/></p>

</div>
<div id='tab-node-0-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-0-dimension-reduction-3-1.png" alt="plot of chunk tab-node-0-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-0-collect-classes](figure_cola/node-0-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Age(p-value) Inferred.Cell.Type(p-value) k
#> ATC:skmeans      364      1.00000                    4.93e-66 2
#> ATC:skmeans      366      0.00122                    1.47e-80 3
#> ATC:skmeans      362      0.00174                    1.36e-79 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node01


Parent node: [Node0](#Node0).
Child nodes: 
                Node011-leaf
        ,
                Node012-leaf
        ,
                Node013-leaf
        ,
                Node021-leaf
        ,
                Node022-leaf
        ,
                Node031-leaf
        ,
                Node032-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["01"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 9180 rows and 129 columns.
#>   Top rows (578) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-01-collect-plots](figure_cola/node-01-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-01-select-partition-number](figure_cola/node-01-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2  1.00           0.976       0.990         0.4796 0.518   0.518
#> 3 3  1.00           0.981       0.992         0.3993 0.744   0.536
#> 4 4  0.79           0.758       0.871         0.0882 0.984   0.953
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-01-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-01-get-classes'>
<ul>
<li><a href='#tab-node-01-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-01-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-01-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-01-get-classes-1'>
<p><a id='tab-node-01-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2
#&gt; O10-A3      2   0.000      0.996 0.00 1.00
#&gt; O10-A9      2   0.000      0.996 0.00 1.00
#&gt; O10-B1      1   0.000      0.979 1.00 0.00
#&gt; O10-B12     2   0.000      0.996 0.00 1.00
#&gt; O10-C11     2   0.000      0.996 0.00 1.00
#&gt; O10-C2      2   0.000      0.996 0.00 1.00
#&gt; O10-D2      2   0.000      0.996 0.00 1.00
#&gt; O10-D8      2   0.000      0.996 0.00 1.00
#&gt; O10-E8      2   0.000      0.996 0.00 1.00
#&gt; O10-E9      2   0.000      0.996 0.00 1.00
#&gt; O10-F2      2   0.000      0.996 0.00 1.00
#&gt; O10-F4      1   0.680      0.786 0.82 0.18
#&gt; O10-F8      2   0.000      0.996 0.00 1.00
#&gt; O10-G1      1   0.000      0.979 1.00 0.00
#&gt; O10-G12     1   0.000      0.979 1.00 0.00
#&gt; O10-G2      1   0.000      0.979 1.00 0.00
#&gt; O10-G3      1   0.000      0.979 1.00 0.00
#&gt; O10-G4      1   0.000      0.979 1.00 0.00
#&gt; O10-G5      1   0.000      0.979 1.00 0.00
#&gt; O10-G7      1   0.000      0.979 1.00 0.00
#&gt; O10-H1      1   0.000      0.979 1.00 0.00
#&gt; O10-H4      1   0.000      0.979 1.00 0.00
#&gt; O10-H5      1   0.000      0.979 1.00 0.00
#&gt; O10-H6      1   0.000      0.979 1.00 0.00
#&gt; O10-H7      1   0.000      0.979 1.00 0.00
#&gt; O12-A2      1   0.000      0.979 1.00 0.00
#&gt; O12-D2      1   0.000      0.979 1.00 0.00
#&gt; O12-E1      1   0.000      0.979 1.00 0.00
#&gt; O7-D3       2   0.827      0.639 0.26 0.74
#&gt; O7-D5       2   0.000      0.996 0.00 1.00
#&gt; O7-G3       1   0.000      0.979 1.00 0.00
#&gt; O8-E1       2   0.000      0.996 0.00 1.00
#&gt; O8-F10      1   0.000      0.979 1.00 0.00
#&gt; O8-F9       1   0.000      0.979 1.00 0.00
#&gt; O8-G9       1   0.000      0.979 1.00 0.00
#&gt; O8-H1       1   0.000      0.979 1.00 0.00
#&gt; O8-H6       2   0.000      0.996 0.00 1.00
#&gt; O9-A2       2   0.000      0.996 0.00 1.00
#&gt; O9-A9       2   0.000      0.996 0.00 1.00
#&gt; O9-B10      2   0.000      0.996 0.00 1.00
#&gt; O9-B11      2   0.000      0.996 0.00 1.00
#&gt; O9-B3       2   0.000      0.996 0.00 1.00
#&gt; O9-C10      2   0.000      0.996 0.00 1.00
#&gt; O9-C3       2   0.000      0.996 0.00 1.00
#&gt; O9-C5       2   0.000      0.996 0.00 1.00
#&gt; O9-C9       2   0.000      0.996 0.00 1.00
#&gt; O9-D1       1   0.242      0.944 0.96 0.04
#&gt; O9-D10      1   0.000      0.979 1.00 0.00
#&gt; O9-D11      2   0.000      0.996 0.00 1.00
#&gt; O9-D5       2   0.000      0.996 0.00 1.00
#&gt; O9-E10      2   0.000      0.996 0.00 1.00
#&gt; O9-E11      1   0.000      0.979 1.00 0.00
#&gt; O9-E5       2   0.000      0.996 0.00 1.00
#&gt; O9-E7       2   0.000      0.996 0.00 1.00
#&gt; O9-E8       1   0.000      0.979 1.00 0.00
#&gt; O9-E9       2   0.000      0.996 0.00 1.00
#&gt; O9-F10      2   0.000      0.996 0.00 1.00
#&gt; O9-F12      2   0.000      0.996 0.00 1.00
#&gt; O9-F2       2   0.000      0.996 0.00 1.00
#&gt; O9-G10      1   0.000      0.979 1.00 0.00
#&gt; O9-G12      2   0.000      0.996 0.00 1.00
#&gt; O9-G2       2   0.000      0.996 0.00 1.00
#&gt; O9-G4       1   0.000      0.979 1.00 0.00
#&gt; O9-G6       2   0.000      0.996 0.00 1.00
#&gt; O9-H3       1   0.943      0.456 0.64 0.36
#&gt; O9-H9       1   0.000      0.979 1.00 0.00
#&gt; S37-A1      2   0.000      0.996 0.00 1.00
#&gt; S37-A10     2   0.000      0.996 0.00 1.00
#&gt; S37-A3      2   0.000      0.996 0.00 1.00
#&gt; S37-A9      1   0.000      0.979 1.00 0.00
#&gt; S37-B1      1   0.722      0.757 0.80 0.20
#&gt; S37-B10     2   0.000      0.996 0.00 1.00
#&gt; S37-B2      2   0.000      0.996 0.00 1.00
#&gt; S37-B4      2   0.000      0.996 0.00 1.00
#&gt; S37-B7      2   0.000      0.996 0.00 1.00
#&gt; S37-C10     2   0.000      0.996 0.00 1.00
#&gt; S37-C3      2   0.000      0.996 0.00 1.00
#&gt; S37-C4      1   0.000      0.979 1.00 0.00
#&gt; S37-C6      2   0.000      0.996 0.00 1.00
#&gt; S37-C7      2   0.000      0.996 0.00 1.00
#&gt; S37-C9      1   0.000      0.979 1.00 0.00
#&gt; S37-D10     2   0.000      0.996 0.00 1.00
#&gt; S37-D2      2   0.141      0.976 0.02 0.98
#&gt; S37-D4      2   0.000      0.996 0.00 1.00
#&gt; S37-D6      2   0.000      0.996 0.00 1.00
#&gt; S37-D8      2   0.000      0.996 0.00 1.00
#&gt; S37-E1      2   0.000      0.996 0.00 1.00
#&gt; S37-E11     2   0.000      0.996 0.00 1.00
#&gt; S37-E2      1   0.827      0.661 0.74 0.26
#&gt; S37-E3      2   0.000      0.996 0.00 1.00
#&gt; S37-E6      2   0.000      0.996 0.00 1.00
#&gt; S37-E9      2   0.000      0.996 0.00 1.00
#&gt; S37-F1      1   0.000      0.979 1.00 0.00
#&gt; S37-F10     1   0.000      0.979 1.00 0.00
#&gt; S37-F12     2   0.000      0.996 0.00 1.00
#&gt; S37-F2      2   0.000      0.996 0.00 1.00
#&gt; S37-F3      2   0.000      0.996 0.00 1.00
#&gt; S37-F5      1   0.000      0.979 1.00 0.00
#&gt; S37-F7      2   0.000      0.996 0.00 1.00
#&gt; S37-F9      2   0.000      0.996 0.00 1.00
#&gt; S37-G1      2   0.000      0.996 0.00 1.00
#&gt; S37-G10     2   0.000      0.996 0.00 1.00
#&gt; S37-G12     2   0.000      0.996 0.00 1.00
#&gt; S37-G3      2   0.000      0.996 0.00 1.00
#&gt; S37-G6      2   0.000      0.996 0.00 1.00
#&gt; S37-G7      2   0.000      0.996 0.00 1.00
#&gt; S37-H2      1   0.000      0.979 1.00 0.00
#&gt; S37-H7      1   0.141      0.962 0.98 0.02
#&gt; S37-H8      1   0.000      0.979 1.00 0.00
#&gt; S37-H9      1   0.000      0.979 1.00 0.00
#&gt; S38-A1      1   0.000      0.979 1.00 0.00
#&gt; S38-A10     1   0.000      0.979 1.00 0.00
#&gt; S38-B11     2   0.000      0.996 0.00 1.00
#&gt; S38-B6      1   0.000      0.979 1.00 0.00
#&gt; S38-C4      2   0.000      0.996 0.00 1.00
#&gt; S38-D12     1   0.000      0.979 1.00 0.00
#&gt; S38-D2      2   0.000      0.996 0.00 1.00
#&gt; S38-D8      1   0.000      0.979 1.00 0.00
#&gt; S38-D9      2   0.000      0.996 0.00 1.00
#&gt; S38-F10     2   0.000      0.996 0.00 1.00
#&gt; S38-F11     2   0.000      0.996 0.00 1.00
#&gt; S38-F3      1   0.000      0.979 1.00 0.00
#&gt; S38-F9      2   0.000      0.996 0.00 1.00
#&gt; S38-G10     1   0.000      0.979 1.00 0.00
#&gt; S38-G12     1   0.000      0.979 1.00 0.00
#&gt; S38-H1      1   0.000      0.979 1.00 0.00
#&gt; S38-H11     2   0.000      0.996 0.00 1.00
#&gt; S38-H4      2   0.000      0.996 0.00 1.00
#&gt; S38-H8      2   0.000      0.996 0.00 1.00
</code></pre>

<script>
$('#tab-node-01-get-classes-1-a').parent().next().next().hide();
$('#tab-node-01-get-classes-1-a').click(function(){
  $('#tab-node-01-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-01-get-classes-2'>
<p><a id='tab-node-01-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3
#&gt; O10-A3      2  0.0000      0.993 0.00 1.00 0.00
#&gt; O10-A9      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O10-B1      1  0.4291      0.776 0.82 0.00 0.18
#&gt; O10-B12     3  0.0000      0.988 0.00 0.00 1.00
#&gt; O10-C11     3  0.0000      0.988 0.00 0.00 1.00
#&gt; O10-C2      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O10-D2      2  0.0000      0.993 0.00 1.00 0.00
#&gt; O10-D8      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O10-E8      2  0.0000      0.993 0.00 1.00 0.00
#&gt; O10-E9      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O10-F2      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O10-F4      2  0.0000      0.993 0.00 1.00 0.00
#&gt; O10-F8      2  0.0000      0.993 0.00 1.00 0.00
#&gt; O10-G1      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-G12     1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-G2      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-G3      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-G4      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-G5      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-G7      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-H1      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-H4      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-H5      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-H6      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O10-H7      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O12-A2      2  0.4002      0.810 0.16 0.84 0.00
#&gt; O12-D2      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O12-E1      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O7-D3       2  0.5334      0.811 0.06 0.82 0.12
#&gt; O7-D5       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O7-G3       1  0.0000      0.996 1.00 0.00 0.00
#&gt; O8-E1       2  0.0000      0.993 0.00 1.00 0.00
#&gt; O8-F10      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O8-F9       1  0.0000      0.996 1.00 0.00 0.00
#&gt; O8-G9       1  0.0000      0.996 1.00 0.00 0.00
#&gt; O8-H1       1  0.0000      0.996 1.00 0.00 0.00
#&gt; O8-H6       2  0.0000      0.993 0.00 1.00 0.00
#&gt; O9-A2       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-A9       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-B10      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-B11      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-B3       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-C10      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-C3       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-C5       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-C9       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-D1       3  0.6280      0.139 0.46 0.00 0.54
#&gt; O9-D10      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O9-D11      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-D5       2  0.0000      0.993 0.00 1.00 0.00
#&gt; O9-E10      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-E11      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O9-E5       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-E7       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-E8       1  0.0000      0.996 1.00 0.00 0.00
#&gt; O9-E9       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-F10      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-F12      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-F2       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-G10      1  0.0000      0.996 1.00 0.00 0.00
#&gt; O9-G12      3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-G2       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-G4       1  0.0000      0.996 1.00 0.00 0.00
#&gt; O9-G6       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-H3       3  0.0000      0.988 0.00 0.00 1.00
#&gt; O9-H9       1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-A1      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-A10     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-A3      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-A9      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-B1      3  0.0892      0.968 0.02 0.00 0.98
#&gt; S37-B10     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-B2      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-B4      3  0.0000      0.988 0.00 0.00 1.00
#&gt; S37-B7      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-C10     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-C3      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-C4      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-C6      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-C7      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-C9      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-D10     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-D2      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-D4      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-D6      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-D8      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-E1      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-E11     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-E2      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-E3      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-E6      3  0.0000      0.988 0.00 0.00 1.00
#&gt; S37-E9      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-F1      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-F10     1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-F12     3  0.0000      0.988 0.00 0.00 1.00
#&gt; S37-F2      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-F3      3  0.0000      0.988 0.00 0.00 1.00
#&gt; S37-F5      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-F7      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-F9      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-G1      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-G10     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-G12     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-G3      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-G6      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-G7      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-H2      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-H7      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S37-H8      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S37-H9      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-A1      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-A10     1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-B11     3  0.0000      0.988 0.00 0.00 1.00
#&gt; S38-B6      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-C4      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S38-D12     1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-D2      3  0.0000      0.988 0.00 0.00 1.00
#&gt; S38-D8      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-D9      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S38-F10     3  0.0000      0.988 0.00 0.00 1.00
#&gt; S38-F11     2  0.0000      0.993 0.00 1.00 0.00
#&gt; S38-F3      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-F9      3  0.0000      0.988 0.00 0.00 1.00
#&gt; S38-G10     1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-G12     1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-H1      1  0.0000      0.996 1.00 0.00 0.00
#&gt; S38-H11     3  0.0000      0.988 0.00 0.00 1.00
#&gt; S38-H4      2  0.0000      0.993 0.00 1.00 0.00
#&gt; S38-H8      2  0.0000      0.993 0.00 1.00 0.00
</code></pre>

<script>
$('#tab-node-01-get-classes-2-a').parent().next().next().hide();
$('#tab-node-01-get-classes-2-a').click(function(){
  $('#tab-node-01-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-01-get-classes-3'>
<p><a id='tab-node-01-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3   p4
#&gt; O10-A3      2  0.4855      0.474 0.00 0.60 0.00 0.40
#&gt; O10-A9      3  0.4855      0.578 0.00 0.00 0.60 0.40
#&gt; O10-B1      4  0.6262      0.597 0.06 0.00 0.40 0.54
#&gt; O10-B12     3  0.4907      0.560 0.00 0.00 0.58 0.42
#&gt; O10-C11     3  0.4907      0.560 0.00 0.00 0.58 0.42
#&gt; O10-C2      3  0.4907      0.560 0.00 0.00 0.58 0.42
#&gt; O10-D2      2  0.4855      0.448 0.00 0.60 0.00 0.40
#&gt; O10-D8      3  0.4907      0.560 0.00 0.00 0.58 0.42
#&gt; O10-E8      2  0.4790      0.478 0.00 0.62 0.00 0.38
#&gt; O10-E9      3  0.4907      0.560 0.00 0.00 0.58 0.42
#&gt; O10-F2      3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O10-F4      2  0.2345      0.820 0.00 0.90 0.00 0.10
#&gt; O10-F8      2  0.2647      0.782 0.00 0.88 0.00 0.12
#&gt; O10-G1      1  0.2921      0.852 0.86 0.00 0.00 0.14
#&gt; O10-G12     1  0.4134      0.743 0.74 0.00 0.00 0.26
#&gt; O10-G2      1  0.4134      0.743 0.74 0.00 0.00 0.26
#&gt; O10-G3      1  0.2921      0.853 0.86 0.00 0.00 0.14
#&gt; O10-G4      1  0.2011      0.884 0.92 0.00 0.00 0.08
#&gt; O10-G5      1  0.4134      0.743 0.74 0.00 0.00 0.26
#&gt; O10-G7      1  0.4134      0.743 0.74 0.00 0.00 0.26
#&gt; O10-H1      1  0.3172      0.838 0.84 0.00 0.00 0.16
#&gt; O10-H4      1  0.3172      0.838 0.84 0.00 0.00 0.16
#&gt; O10-H5      1  0.3172      0.838 0.84 0.00 0.00 0.16
#&gt; O10-H6      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; O10-H7      1  0.1637      0.893 0.94 0.00 0.00 0.06
#&gt; O12-A2      2  0.4977      0.179 0.00 0.54 0.00 0.46
#&gt; O12-D2      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; O12-E1      1  0.1637      0.894 0.94 0.00 0.00 0.06
#&gt; O7-D3       4  0.7010      0.107 0.02 0.34 0.08 0.56
#&gt; O7-D5       3  0.0707      0.781 0.00 0.00 0.98 0.02
#&gt; O7-G3       1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; O8-E1       2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; O8-F10      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; O8-F9       1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; O8-G9       1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; O8-H1       1  0.4977      0.431 0.54 0.00 0.00 0.46
#&gt; O8-H6       2  0.4277      0.680 0.00 0.72 0.00 0.28
#&gt; O9-A2       3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-A9       3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-B10      3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-B11      3  0.1637      0.752 0.00 0.00 0.94 0.06
#&gt; O9-B3       3  0.3975      0.395 0.00 0.00 0.76 0.24
#&gt; O9-C10      3  0.0707      0.781 0.00 0.00 0.98 0.02
#&gt; O9-C3       3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-C5       3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-C9       3  0.3172      0.735 0.00 0.00 0.84 0.16
#&gt; O9-D1       4  0.6262      0.598 0.06 0.00 0.40 0.54
#&gt; O9-D10      1  0.2647      0.851 0.88 0.00 0.00 0.12
#&gt; O9-D11      3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-D5       2  0.0707      0.848 0.00 0.98 0.00 0.02
#&gt; O9-E10      3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-E11      1  0.2647      0.850 0.88 0.00 0.00 0.12
#&gt; O9-E5       3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-E7       3  0.0707      0.765 0.00 0.00 0.98 0.02
#&gt; O9-E8       1  0.1637      0.890 0.94 0.00 0.00 0.06
#&gt; O9-E9       3  0.2011      0.771 0.00 0.00 0.92 0.08
#&gt; O9-F10      3  0.0707      0.781 0.00 0.00 0.98 0.02
#&gt; O9-F12      3  0.3610      0.713 0.00 0.00 0.80 0.20
#&gt; O9-F2       3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-G10      1  0.3172      0.838 0.84 0.00 0.00 0.16
#&gt; O9-G12      3  0.1211      0.779 0.00 0.00 0.96 0.04
#&gt; O9-G2       3  0.0000      0.778 0.00 0.00 1.00 0.00
#&gt; O9-G4       1  0.4332      0.776 0.80 0.00 0.04 0.16
#&gt; O9-G6       3  0.0707      0.781 0.00 0.00 0.98 0.02
#&gt; O9-H3       3  0.4472      0.407 0.02 0.00 0.76 0.22
#&gt; O9-H9       1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S37-A1      2  0.0707      0.847 0.00 0.98 0.00 0.02
#&gt; S37-A10     2  0.2011      0.835 0.00 0.92 0.00 0.08
#&gt; S37-A3      2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; S37-A9      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S37-B1      3  0.3335      0.681 0.02 0.00 0.86 0.12
#&gt; S37-B10     2  0.1211      0.844 0.00 0.96 0.00 0.04
#&gt; S37-B2      2  0.2647      0.809 0.00 0.88 0.00 0.12
#&gt; S37-B4      3  0.2921      0.730 0.00 0.00 0.86 0.14
#&gt; S37-B7      2  0.2345      0.825 0.00 0.90 0.00 0.10
#&gt; S37-C10     2  0.0707      0.847 0.00 0.98 0.00 0.02
#&gt; S37-C3      2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; S37-C4      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S37-C6      2  0.2345      0.823 0.00 0.90 0.00 0.10
#&gt; S37-C7      2  0.1211      0.844 0.00 0.96 0.00 0.04
#&gt; S37-C9      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S37-D10     2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; S37-D2      2  0.3975      0.674 0.00 0.76 0.00 0.24
#&gt; S37-D4      2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; S37-D6      2  0.1637      0.840 0.00 0.94 0.00 0.06
#&gt; S37-D8      2  0.2647      0.808 0.00 0.88 0.00 0.12
#&gt; S37-E1      2  0.2011      0.837 0.00 0.92 0.00 0.08
#&gt; S37-E11     2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; S37-E2      2  0.1637      0.841 0.00 0.94 0.00 0.06
#&gt; S37-E3      2  0.1211      0.838 0.00 0.96 0.00 0.04
#&gt; S37-E6      3  0.2011      0.770 0.00 0.00 0.92 0.08
#&gt; S37-E9      2  0.0707      0.847 0.00 0.98 0.00 0.02
#&gt; S37-F1      1  0.0707      0.901 0.98 0.00 0.00 0.02
#&gt; S37-F10     1  0.2345      0.853 0.90 0.00 0.00 0.10
#&gt; S37-F12     3  0.0707      0.781 0.00 0.00 0.98 0.02
#&gt; S37-F2      2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; S37-F3      3  0.0707      0.779 0.00 0.00 0.98 0.02
#&gt; S37-F5      1  0.1637      0.884 0.94 0.00 0.00 0.06
#&gt; S37-F7      2  0.3172      0.774 0.00 0.84 0.00 0.16
#&gt; S37-F9      2  0.2011      0.834 0.00 0.92 0.00 0.08
#&gt; S37-G1      2  0.1211      0.839 0.00 0.96 0.00 0.04
#&gt; S37-G10     2  0.0707      0.845 0.00 0.98 0.00 0.02
#&gt; S37-G12     2  0.0000      0.849 0.00 1.00 0.00 0.00
#&gt; S37-G3      2  0.5957      0.330 0.00 0.54 0.04 0.42
#&gt; S37-G6      2  0.0707      0.845 0.00 0.98 0.00 0.02
#&gt; S37-G7      2  0.2345      0.824 0.00 0.90 0.00 0.10
#&gt; S37-H2      1  0.6323      0.263 0.50 0.06 0.00 0.44
#&gt; S37-H7      2  0.2921      0.793 0.00 0.86 0.00 0.14
#&gt; S37-H8      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S37-H9      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-A1      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-A10     1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-B11     3  0.4522      0.630 0.00 0.00 0.68 0.32
#&gt; S38-B6      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-C4      2  0.4907      0.414 0.00 0.58 0.00 0.42
#&gt; S38-D12     1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-D2      3  0.4713      0.609 0.00 0.00 0.64 0.36
#&gt; S38-D8      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-D9      2  0.4855      0.448 0.00 0.60 0.00 0.40
#&gt; S38-F10     3  0.4855      0.578 0.00 0.00 0.60 0.40
#&gt; S38-F11     2  0.4790      0.478 0.00 0.62 0.00 0.38
#&gt; S38-F3      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-F9      3  0.4907      0.560 0.00 0.00 0.58 0.42
#&gt; S38-G10     1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-G12     1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-H1      1  0.0000      0.909 1.00 0.00 0.00 0.00
#&gt; S38-H11     3  0.4855      0.578 0.00 0.00 0.60 0.40
#&gt; S38-H4      2  0.1211      0.846 0.00 0.96 0.00 0.04
#&gt; S38-H8      2  0.0707      0.845 0.00 0.98 0.00 0.02
</code></pre>

<script>
$('#tab-node-01-get-classes-3-a').parent().next().next().hide();
$('#tab-node-01-get-classes-3-a').click(function(){
  $('#tab-node-01-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-01-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-01-consensus-heatmap'>
<ul>
<li><a href='#tab-node-01-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-01-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-01-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-01-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-01-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-01-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-01-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-01-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-01-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-01-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-01-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-01-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-01-membership-heatmap'>
<ul>
<li><a href='#tab-node-01-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-01-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-01-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-01-membership-heatmap-1-1.png" alt="plot of chunk tab-node-01-membership-heatmap-1"/></p>

</div>
<div id='tab-node-01-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-01-membership-heatmap-2-1.png" alt="plot of chunk tab-node-01-membership-heatmap-2"/></p>

</div>
<div id='tab-node-01-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-01-membership-heatmap-3-1.png" alt="plot of chunk tab-node-01-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-01-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-01-get-signatures'>
<ul>
<li><a href='#tab-node-01-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-01-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-01-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-1-1.png" alt="plot of chunk tab-node-01-get-signatures-1"/></p>

</div>
<div id='tab-node-01-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-2-1.png" alt="plot of chunk tab-node-01-get-signatures-2"/></p>

</div>
<div id='tab-node-01-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-3-1.png" alt="plot of chunk tab-node-01-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-01-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-01-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-01-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-01-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-01-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-01-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-01-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-01-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-01-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-01-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-01-signature_compare](figure_cola/node-01-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-01-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-01-dimension-reduction'>
<ul>
<li><a href='#tab-node-01-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-01-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-01-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-01-dimension-reduction-1-1.png" alt="plot of chunk tab-node-01-dimension-reduction-1"/></p>

</div>
<div id='tab-node-01-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-01-dimension-reduction-2-1.png" alt="plot of chunk tab-node-01-dimension-reduction-2"/></p>

</div>
<div id='tab-node-01-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-01-dimension-reduction-3-1.png" alt="plot of chunk tab-node-01-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-01-collect-classes](figure_cola/node-01-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Age(p-value) Inferred.Cell.Type(p-value) k
#> ATC:skmeans      128     2.60e-01                    4.38e-03 2
#> ATC:skmeans      128     4.61e-06                    1.88e-19 3
#> ATC:skmeans      116     1.53e-06                    4.78e-18 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node02


Parent node: [Node0](#Node0).
Child nodes: 
                Node011-leaf
        ,
                Node012-leaf
        ,
                Node013-leaf
        ,
                Node021-leaf
        ,
                Node022-leaf
        ,
                Node031-leaf
        ,
                Node032-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["02"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 9164 rows and 112 columns.
#>   Top rows (877) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-02-collect-plots](figure_cola/node-02-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-02-select-partition-number](figure_cola/node-02-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.972       0.988          0.498 0.506   0.506
#> 3 3 0.711           0.822       0.899          0.289 0.819   0.653
#> 4 4 0.759           0.805       0.908          0.137 0.847   0.613
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-02-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-02-get-classes'>
<ul>
<li><a href='#tab-node-02-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-02-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-02-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-02-get-classes-1'>
<p><a id='tab-node-02-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2
#&gt; O10-A1      2   0.000      1.000 0.00 1.00
#&gt; O10-A12     2   0.000      1.000 0.00 1.00
#&gt; O10-A4      1   0.000      0.980 1.00 0.00
#&gt; O10-C4      2   0.000      1.000 0.00 1.00
#&gt; O10-D3      1   0.000      0.980 1.00 0.00
#&gt; O10-F6      1   0.000      0.980 1.00 0.00
#&gt; O12-A1      1   0.000      0.980 1.00 0.00
#&gt; O12-A12     1   0.000      0.980 1.00 0.00
#&gt; O12-A3      1   0.000      0.980 1.00 0.00
#&gt; O12-B1      2   0.000      1.000 0.00 1.00
#&gt; O12-C1      2   0.000      1.000 0.00 1.00
#&gt; O12-D1      2   0.141      0.979 0.02 0.98
#&gt; O12-E12     1   0.327      0.924 0.94 0.06
#&gt; O12-F2      1   0.000      0.980 1.00 0.00
#&gt; O12-G2      2   0.000      1.000 0.00 1.00
#&gt; O12-H1      1   0.995      0.159 0.54 0.46
#&gt; O12-H2      2   0.000      1.000 0.00 1.00
#&gt; O7-A5       2   0.000      1.000 0.00 1.00
#&gt; O7-A7       2   0.000      1.000 0.00 1.00
#&gt; O7-A8       1   0.000      0.980 1.00 0.00
#&gt; O7-B1       1   0.000      0.980 1.00 0.00
#&gt; O7-B12      2   0.000      1.000 0.00 1.00
#&gt; O7-B8       2   0.000      1.000 0.00 1.00
#&gt; O7-B9       1   0.000      0.980 1.00 0.00
#&gt; O7-C2       2   0.000      1.000 0.00 1.00
#&gt; O7-C3       2   0.000      1.000 0.00 1.00
#&gt; O7-C6       1   0.881      0.586 0.70 0.30
#&gt; O7-D1       1   0.000      0.980 1.00 0.00
#&gt; O7-D11      1   0.000      0.980 1.00 0.00
#&gt; O7-D12      1   0.242      0.944 0.96 0.04
#&gt; O7-D2       1   0.000      0.980 1.00 0.00
#&gt; O7-E5       1   0.000      0.980 1.00 0.00
#&gt; O7-E6       2   0.000      1.000 0.00 1.00
#&gt; O7-F1       1   0.000      0.980 1.00 0.00
#&gt; O7-F10      2   0.000      1.000 0.00 1.00
#&gt; O7-F11      1   0.000      0.980 1.00 0.00
#&gt; O7-F4       1   0.000      0.980 1.00 0.00
#&gt; O7-G12      2   0.000      1.000 0.00 1.00
#&gt; O7-G9       2   0.000      1.000 0.00 1.00
#&gt; O7-H3       1   0.000      0.980 1.00 0.00
#&gt; O7-H4       2   0.000      1.000 0.00 1.00
#&gt; O7-H8       2   0.000      1.000 0.00 1.00
#&gt; O8-A5       2   0.000      1.000 0.00 1.00
#&gt; O8-A9       1   0.000      0.980 1.00 0.00
#&gt; O8-B1       2   0.000      1.000 0.00 1.00
#&gt; O8-C2       1   0.000      0.980 1.00 0.00
#&gt; O8-E3       1   0.000      0.980 1.00 0.00
#&gt; O8-E7       2   0.000      1.000 0.00 1.00
#&gt; O8-F2       2   0.000      1.000 0.00 1.00
#&gt; O8-F6       2   0.000      1.000 0.00 1.00
#&gt; O8-F8       1   0.000      0.980 1.00 0.00
#&gt; O8-H11      2   0.000      1.000 0.00 1.00
#&gt; O8-H2       1   0.904      0.546 0.68 0.32
#&gt; O9-A12      2   0.000      1.000 0.00 1.00
#&gt; O9-A3       2   0.000      1.000 0.00 1.00
#&gt; O9-A5       1   0.000      0.980 1.00 0.00
#&gt; O9-A6       1   0.000      0.980 1.00 0.00
#&gt; O9-B5       1   0.469      0.881 0.90 0.10
#&gt; O9-B8       1   0.000      0.980 1.00 0.00
#&gt; O9-D2       1   0.000      0.980 1.00 0.00
#&gt; O9-D7       2   0.000      1.000 0.00 1.00
#&gt; O9-D8       2   0.000      1.000 0.00 1.00
#&gt; O9-E1       2   0.000      1.000 0.00 1.00
#&gt; O9-E2       1   0.000      0.980 1.00 0.00
#&gt; O9-F1       1   0.000      0.980 1.00 0.00
#&gt; O9-F4       1   0.000      0.980 1.00 0.00
#&gt; O9-F5       1   0.000      0.980 1.00 0.00
#&gt; O9-G9       1   0.000      0.980 1.00 0.00
#&gt; O9-H12      2   0.000      1.000 0.00 1.00
#&gt; O9-H5       2   0.000      1.000 0.00 1.00
#&gt; S37-A2      2   0.000      1.000 0.00 1.00
#&gt; S37-A5      1   0.000      0.980 1.00 0.00
#&gt; S37-A6      1   0.000      0.980 1.00 0.00
#&gt; S37-A8      2   0.000      1.000 0.00 1.00
#&gt; S37-B11     1   0.000      0.980 1.00 0.00
#&gt; S37-B12     2   0.000      1.000 0.00 1.00
#&gt; S37-C12     2   0.000      1.000 0.00 1.00
#&gt; S37-C8      1   0.000      0.980 1.00 0.00
#&gt; S37-D12     1   0.000      0.980 1.00 0.00
#&gt; S37-E7      1   0.000      0.980 1.00 0.00
#&gt; S37-E8      1   0.000      0.980 1.00 0.00
#&gt; S37-G2      2   0.000      1.000 0.00 1.00
#&gt; S37-G4      2   0.000      1.000 0.00 1.00
#&gt; S38-A12     2   0.000      1.000 0.00 1.00
#&gt; S38-A2      1   0.000      0.980 1.00 0.00
#&gt; S38-A5      1   0.000      0.980 1.00 0.00
#&gt; S38-A9      1   0.000      0.980 1.00 0.00
#&gt; S38-B2      1   0.000      0.980 1.00 0.00
#&gt; S38-B7      1   0.000      0.980 1.00 0.00
#&gt; S38-B8      1   0.000      0.980 1.00 0.00
#&gt; S38-B9      2   0.000      1.000 0.00 1.00
#&gt; S38-C3      1   0.000      0.980 1.00 0.00
#&gt; S38-C9      2   0.000      1.000 0.00 1.00
#&gt; S38-D1      1   0.000      0.980 1.00 0.00
#&gt; S38-D5      1   0.000      0.980 1.00 0.00
#&gt; S38-D6      1   0.000      0.980 1.00 0.00
#&gt; S38-E1      1   0.000      0.980 1.00 0.00
#&gt; S38-E4      1   0.000      0.980 1.00 0.00
#&gt; S38-E5      2   0.000      1.000 0.00 1.00
#&gt; S38-E6      2   0.000      1.000 0.00 1.00
#&gt; S38-E7      1   0.000      0.980 1.00 0.00
#&gt; S38-E9      2   0.000      1.000 0.00 1.00
#&gt; S38-F2      1   0.000      0.980 1.00 0.00
#&gt; S38-F5      2   0.000      1.000 0.00 1.00
#&gt; S38-F6      1   0.000      0.980 1.00 0.00
#&gt; S38-F8      1   0.000      0.980 1.00 0.00
#&gt; S38-G4      1   0.000      0.980 1.00 0.00
#&gt; S38-G5      1   0.000      0.980 1.00 0.00
#&gt; S38-G6      2   0.000      1.000 0.00 1.00
#&gt; S38-H2      1   0.000      0.980 1.00 0.00
#&gt; S38-H5      2   0.000      1.000 0.00 1.00
#&gt; S38-H6      1   0.000      0.980 1.00 0.00
</code></pre>

<script>
$('#tab-node-02-get-classes-1-a').parent().next().next().hide();
$('#tab-node-02-get-classes-1-a').click(function(){
  $('#tab-node-02-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-02-get-classes-2'>
<p><a id='tab-node-02-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3
#&gt; O10-A1      3  0.0892     0.7458 0.00 0.02 0.98
#&gt; O10-A12     3  0.2537     0.6988 0.00 0.08 0.92
#&gt; O10-A4      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O10-C4      2  0.2959     0.8623 0.00 0.90 0.10
#&gt; O10-D3      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O10-F6      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O12-A1      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O12-A12     1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O12-A3      3  0.4002     0.7621 0.16 0.00 0.84
#&gt; O12-B1      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O12-C1      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O12-D1      2  0.3686     0.6969 0.14 0.86 0.00
#&gt; O12-E12     1  0.4796     0.6878 0.78 0.22 0.00
#&gt; O12-F2      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O12-G2      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O12-H1      1  0.5560     0.5660 0.70 0.30 0.00
#&gt; O12-H2      2  0.5706     0.7607 0.00 0.68 0.32
#&gt; O7-A5       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-A7       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-A8       3  0.5560     0.6345 0.30 0.00 0.70
#&gt; O7-B1       3  0.4796     0.7224 0.22 0.00 0.78
#&gt; O7-B12      2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-B8       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-B9       3  0.3340     0.7782 0.12 0.00 0.88
#&gt; O7-C2       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-C3       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-C6       3  0.0000     0.7563 0.00 0.00 1.00
#&gt; O7-D1       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O7-D11      3  0.2959     0.7775 0.10 0.00 0.90
#&gt; O7-D12      3  0.0000     0.7563 0.00 0.00 1.00
#&gt; O7-D2       1  0.2959     0.8723 0.90 0.00 0.10
#&gt; O7-E5       1  0.5397     0.5866 0.72 0.00 0.28
#&gt; O7-E6       2  0.2537     0.8617 0.00 0.92 0.08
#&gt; O7-F1       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O7-F10      2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-F11      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O7-F4       3  0.5397     0.6647 0.28 0.00 0.72
#&gt; O7-G12      3  0.2537     0.6988 0.00 0.08 0.92
#&gt; O7-G9       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O7-H3       1  0.3340     0.8500 0.88 0.00 0.12
#&gt; O7-H4       2  0.1529     0.8612 0.00 0.96 0.04
#&gt; O7-H8       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O8-A5       2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O8-A9       1  0.2066     0.9079 0.94 0.00 0.06
#&gt; O8-B1       2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O8-C2       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O8-E3       1  0.1529     0.9200 0.96 0.00 0.04
#&gt; O8-E7       2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O8-F2       2  0.2537     0.8629 0.00 0.92 0.08
#&gt; O8-F6       3  0.2066     0.7167 0.00 0.06 0.94
#&gt; O8-F8       3  0.5216     0.6872 0.26 0.00 0.74
#&gt; O8-H11      2  0.4002     0.8569 0.00 0.84 0.16
#&gt; O8-H2       3  0.0000     0.7563 0.00 0.00 1.00
#&gt; O9-A12      2  0.4002     0.8569 0.00 0.84 0.16
#&gt; O9-A3       2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O9-A5       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O9-A6       1  0.2066     0.9079 0.94 0.00 0.06
#&gt; O9-B5       3  0.0000     0.7563 0.00 0.00 1.00
#&gt; O9-B8       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O9-D2       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O9-D7       2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O9-D8       2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O9-E1       2  0.0000     0.8567 0.00 1.00 0.00
#&gt; O9-E2       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O9-F1       1  0.4796     0.6852 0.78 0.22 0.00
#&gt; O9-F4       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O9-F5       1  0.1529     0.9211 0.96 0.00 0.04
#&gt; O9-G9       1  0.0000     0.9404 1.00 0.00 0.00
#&gt; O9-H12      2  0.5216     0.8285 0.00 0.74 0.26
#&gt; O9-H5       2  0.0000     0.8567 0.00 1.00 0.00
#&gt; S37-A2      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; S37-A5      1  0.2537     0.8918 0.92 0.00 0.08
#&gt; S37-A6      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S37-A8      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; S37-B11     1  0.4551     0.7883 0.84 0.02 0.14
#&gt; S37-B12     2  0.4291     0.8531 0.00 0.82 0.18
#&gt; S37-C12     2  0.3340     0.8617 0.00 0.88 0.12
#&gt; S37-C8      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S37-D12     1  0.2537     0.8918 0.92 0.00 0.08
#&gt; S37-E7      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S37-E8      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S37-G2      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; S37-G4      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; S38-A12     2  0.5216     0.8285 0.00 0.74 0.26
#&gt; S38-A2      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-A5      1  0.1529     0.9209 0.96 0.00 0.04
#&gt; S38-A9      1  0.0892     0.9314 0.98 0.00 0.02
#&gt; S38-B2      1  0.2066     0.9079 0.94 0.00 0.06
#&gt; S38-B7      3  0.5216     0.6872 0.26 0.00 0.74
#&gt; S38-B8      1  0.6302    -0.0649 0.52 0.00 0.48
#&gt; S38-B9      3  0.6309    -0.4318 0.00 0.50 0.50
#&gt; S38-C3      1  0.2066     0.9079 0.94 0.00 0.06
#&gt; S38-C9      3  0.2537     0.6988 0.00 0.08 0.92
#&gt; S38-D1      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-D5      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-D6      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-E1      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-E4      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-E5      2  0.4796     0.8417 0.00 0.78 0.22
#&gt; S38-E6      2  0.5216     0.8285 0.00 0.74 0.26
#&gt; S38-E7      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-E9      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; S38-F2      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-F5      2  0.0000     0.8567 0.00 1.00 0.00
#&gt; S38-F6      3  0.5216     0.6872 0.26 0.00 0.74
#&gt; S38-F8      3  0.5835     0.5793 0.34 0.00 0.66
#&gt; S38-G4      3  0.5397     0.6647 0.28 0.00 0.72
#&gt; S38-G5      1  0.0000     0.9404 1.00 0.00 0.00
#&gt; S38-G6      2  0.5216     0.8285 0.00 0.74 0.26
#&gt; S38-H2      1  0.0892     0.9314 0.98 0.00 0.02
#&gt; S38-H5      3  0.4291     0.5481 0.00 0.18 0.82
#&gt; S38-H6      1  0.0000     0.9404 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-02-get-classes-2-a').parent().next().next().hide();
$('#tab-node-02-get-classes-2-a').click(function(){
  $('#tab-node-02-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-02-get-classes-3'>
<p><a id='tab-node-02-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3   p4
#&gt; O10-A1      3  0.5860      0.266 0.00 0.38 0.58 0.04
#&gt; O10-A12     2  0.3606      0.784 0.00 0.84 0.14 0.02
#&gt; O10-A4      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O10-C4      2  0.3610      0.730 0.00 0.80 0.00 0.20
#&gt; O10-D3      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O10-F6      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O12-A1      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O12-A12     1  0.0707      0.897 0.98 0.00 0.02 0.00
#&gt; O12-A3      3  0.0707      0.897 0.00 0.00 0.98 0.02
#&gt; O12-B1      4  0.3975      0.663 0.00 0.24 0.00 0.76
#&gt; O12-C1      4  0.0707      0.885 0.00 0.02 0.00 0.98
#&gt; O12-D1      4  0.0707      0.873 0.02 0.00 0.00 0.98
#&gt; O12-E12     4  0.4977      0.120 0.46 0.00 0.00 0.54
#&gt; O12-F2      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O12-G2      4  0.2921      0.794 0.00 0.14 0.00 0.86
#&gt; O12-H1      4  0.0707      0.873 0.02 0.00 0.00 0.98
#&gt; O12-H2      2  0.0707      0.884 0.00 0.98 0.02 0.00
#&gt; O7-A5       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-A7       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-A8       3  0.1211      0.879 0.04 0.00 0.96 0.00
#&gt; O7-B1       3  0.0000      0.899 0.00 0.00 1.00 0.00
#&gt; O7-B12      2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-B8       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-B9       3  0.0000      0.899 0.00 0.00 1.00 0.00
#&gt; O7-C2       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-C3       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-C6       3  0.0707      0.897 0.00 0.00 0.98 0.02
#&gt; O7-D1       1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O7-D11      3  0.0707      0.897 0.00 0.00 0.98 0.02
#&gt; O7-D12      3  0.0000      0.899 0.00 0.00 1.00 0.00
#&gt; O7-D2       1  0.4994      0.218 0.52 0.00 0.48 0.00
#&gt; O7-E5       3  0.3975      0.625 0.24 0.00 0.76 0.00
#&gt; O7-E6       4  0.4713      0.425 0.00 0.36 0.00 0.64
#&gt; O7-F1       1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O7-F10      2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-F11      1  0.2647      0.843 0.88 0.00 0.12 0.00
#&gt; O7-F4       3  0.0707      0.891 0.02 0.00 0.98 0.00
#&gt; O7-G12      2  0.3037      0.823 0.00 0.88 0.10 0.02
#&gt; O7-G9       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O7-H3       1  0.4907      0.408 0.58 0.00 0.42 0.00
#&gt; O7-H4       2  0.4948      0.228 0.00 0.56 0.00 0.44
#&gt; O7-H8       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O8-A5       4  0.1211      0.881 0.00 0.04 0.00 0.96
#&gt; O8-A9       1  0.3975      0.737 0.76 0.00 0.24 0.00
#&gt; O8-B1       4  0.1211      0.881 0.00 0.04 0.00 0.96
#&gt; O8-C2       1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O8-E3       1  0.3801      0.750 0.78 0.00 0.22 0.00
#&gt; O8-E7       2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O8-F2       2  0.4134      0.639 0.00 0.74 0.00 0.26
#&gt; O8-F6       2  0.5487      0.328 0.00 0.58 0.40 0.02
#&gt; O8-F8       3  0.0000      0.899 0.00 0.00 1.00 0.00
#&gt; O8-H11      2  0.1637      0.862 0.00 0.94 0.00 0.06
#&gt; O8-H2       3  0.0707      0.897 0.00 0.00 0.98 0.02
#&gt; O9-A12      2  0.2647      0.816 0.00 0.88 0.00 0.12
#&gt; O9-A3       4  0.0707      0.885 0.00 0.02 0.00 0.98
#&gt; O9-A5       1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O9-A6       1  0.4134      0.711 0.74 0.00 0.26 0.00
#&gt; O9-B5       3  0.0707      0.897 0.00 0.00 0.98 0.02
#&gt; O9-B8       1  0.0707      0.897 0.98 0.00 0.02 0.00
#&gt; O9-D2       1  0.0707      0.897 0.98 0.00 0.02 0.00
#&gt; O9-D7       2  0.4855      0.348 0.00 0.60 0.00 0.40
#&gt; O9-D8       2  0.4790      0.404 0.00 0.62 0.00 0.38
#&gt; O9-E1       4  0.0707      0.885 0.00 0.02 0.00 0.98
#&gt; O9-E2       1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O9-F1       4  0.2921      0.762 0.14 0.00 0.00 0.86
#&gt; O9-F4       1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O9-F5       1  0.3400      0.796 0.82 0.00 0.18 0.00
#&gt; O9-G9       1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; O9-H12      2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; O9-H5       4  0.0707      0.885 0.00 0.02 0.00 0.98
#&gt; S37-A2      4  0.0707      0.885 0.00 0.02 0.00 0.98
#&gt; S37-A5      1  0.3975      0.731 0.76 0.00 0.24 0.00
#&gt; S37-A6      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S37-A8      4  0.0707      0.885 0.00 0.02 0.00 0.98
#&gt; S37-B11     1  0.4894      0.722 0.78 0.00 0.12 0.10
#&gt; S37-B12     2  0.1637      0.863 0.00 0.94 0.00 0.06
#&gt; S37-C12     2  0.2011      0.848 0.00 0.92 0.00 0.08
#&gt; S37-C8      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S37-D12     1  0.4790      0.483 0.62 0.00 0.38 0.00
#&gt; S37-E7      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S37-E8      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S37-G2      4  0.4522      0.517 0.00 0.32 0.00 0.68
#&gt; S37-G4      4  0.0707      0.885 0.00 0.02 0.00 0.98
#&gt; S38-A12     2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; S38-A2      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-A5      1  0.4624      0.568 0.66 0.00 0.34 0.00
#&gt; S38-A9      1  0.3172      0.813 0.84 0.00 0.16 0.00
#&gt; S38-B2      1  0.4277      0.673 0.72 0.00 0.28 0.00
#&gt; S38-B7      3  0.0000      0.899 0.00 0.00 1.00 0.00
#&gt; S38-B8      3  0.3801      0.669 0.22 0.00 0.78 0.00
#&gt; S38-B9      2  0.0707      0.882 0.00 0.98 0.00 0.02
#&gt; S38-C3      1  0.3975      0.735 0.76 0.00 0.24 0.00
#&gt; S38-C9      2  0.3335      0.805 0.00 0.86 0.12 0.02
#&gt; S38-D1      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-D5      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-D6      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-E1      1  0.0707      0.897 0.98 0.00 0.02 0.00
#&gt; S38-E4      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-E5      2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; S38-E6      2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; S38-E7      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-E9      4  0.1211      0.881 0.00 0.04 0.00 0.96
#&gt; S38-F2      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-F5      4  0.1211      0.881 0.00 0.04 0.00 0.96
#&gt; S38-F6      3  0.0707      0.897 0.00 0.00 0.98 0.02
#&gt; S38-F8      3  0.4936      0.604 0.28 0.00 0.70 0.02
#&gt; S38-G4      3  0.1637      0.867 0.06 0.00 0.94 0.00
#&gt; S38-G5      1  0.0000      0.904 1.00 0.00 0.00 0.00
#&gt; S38-G6      2  0.0000      0.891 0.00 1.00 0.00 0.00
#&gt; S38-H2      1  0.2011      0.868 0.92 0.00 0.08 0.00
#&gt; S38-H5      2  0.1913      0.864 0.00 0.94 0.04 0.02
#&gt; S38-H6      1  0.0000      0.904 1.00 0.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-02-get-classes-3-a').parent().next().next().hide();
$('#tab-node-02-get-classes-3-a').click(function(){
  $('#tab-node-02-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-02-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-02-consensus-heatmap'>
<ul>
<li><a href='#tab-node-02-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-02-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-02-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-02-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-02-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-02-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-02-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-02-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-02-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-02-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-02-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-02-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-02-membership-heatmap'>
<ul>
<li><a href='#tab-node-02-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-02-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-02-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-02-membership-heatmap-1-1.png" alt="plot of chunk tab-node-02-membership-heatmap-1"/></p>

</div>
<div id='tab-node-02-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-02-membership-heatmap-2-1.png" alt="plot of chunk tab-node-02-membership-heatmap-2"/></p>

</div>
<div id='tab-node-02-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-02-membership-heatmap-3-1.png" alt="plot of chunk tab-node-02-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-02-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-02-get-signatures'>
<ul>
<li><a href='#tab-node-02-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-02-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-02-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-1-1.png" alt="plot of chunk tab-node-02-get-signatures-1"/></p>

</div>
<div id='tab-node-02-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-2-1.png" alt="plot of chunk tab-node-02-get-signatures-2"/></p>

</div>
<div id='tab-node-02-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-3-1.png" alt="plot of chunk tab-node-02-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-02-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-02-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-02-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-02-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-02-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-02-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-02-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-02-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-02-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-02-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-02-signature_compare](figure_cola/node-02-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-02-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-02-dimension-reduction'>
<ul>
<li><a href='#tab-node-02-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-02-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-02-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-02-dimension-reduction-1-1.png" alt="plot of chunk tab-node-02-dimension-reduction-1"/></p>

</div>
<div id='tab-node-02-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-02-dimension-reduction-2-1.png" alt="plot of chunk tab-node-02-dimension-reduction-2"/></p>

</div>
<div id='tab-node-02-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-02-dimension-reduction-3-1.png" alt="plot of chunk tab-node-02-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-02-collect-classes](figure_cola/node-02-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Age(p-value) Inferred.Cell.Type(p-value) k
#> ATC:skmeans      111        0.293                    6.16e-03 2
#> ATC:skmeans      110        0.192                    1.33e-03 3
#> ATC:skmeans      102        0.309                    4.29e-08 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node03


Parent node: [Node0](#Node0).
Child nodes: 
                Node011-leaf
        ,
                Node012-leaf
        ,
                Node013-leaf
        ,
                Node021-leaf
        ,
                Node022-leaf
        ,
                Node031-leaf
        ,
                Node032-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["03"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 9177 rows and 126 columns.
#>   Top rows (644) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-03-collect-plots](figure_cola/node-03-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-03-select-partition-number](figure_cola/node-03-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.999           0.968       0.986          0.504 0.496   0.496
#> 3 3 0.848           0.867       0.940          0.251 0.851   0.707
#> 4 4 0.698           0.763       0.888          0.127 0.890   0.720
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-03-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-03-get-classes'>
<ul>
<li><a href='#tab-node-03-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-03-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-03-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-03-get-classes-1'>
<p><a id='tab-node-03-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2
#&gt; O10-A11     1   0.000      0.981 1.00 0.00
#&gt; O10-A5      2   0.000      0.990 0.00 1.00
#&gt; O10-A7      2   0.000      0.990 0.00 1.00
#&gt; O10-A8      2   0.000      0.990 0.00 1.00
#&gt; O10-B10     2   0.000      0.990 0.00 1.00
#&gt; O10-B11     2   0.000      0.990 0.00 1.00
#&gt; O10-B2      2   0.000      0.990 0.00 1.00
#&gt; O10-B5      2   0.000      0.990 0.00 1.00
#&gt; O10-B6      1   0.327      0.924 0.94 0.06
#&gt; O10-B7      2   0.000      0.990 0.00 1.00
#&gt; O10-B9      2   0.000      0.990 0.00 1.00
#&gt; O10-C10     2   0.000      0.990 0.00 1.00
#&gt; O10-C12     2   0.000      0.990 0.00 1.00
#&gt; O10-C5      2   0.000      0.990 0.00 1.00
#&gt; O10-C6      2   0.000      0.990 0.00 1.00
#&gt; O10-C9      2   0.000      0.990 0.00 1.00
#&gt; O10-D1      2   0.000      0.990 0.00 1.00
#&gt; O10-D11     2   0.000      0.990 0.00 1.00
#&gt; O10-D12     2   0.000      0.990 0.00 1.00
#&gt; O10-D5      2   0.000      0.990 0.00 1.00
#&gt; O10-D6      2   0.000      0.990 0.00 1.00
#&gt; O10-D9      2   0.000      0.990 0.00 1.00
#&gt; O10-E1      2   0.000      0.990 0.00 1.00
#&gt; O10-E11     2   0.000      0.990 0.00 1.00
#&gt; O10-E12     2   0.000      0.990 0.00 1.00
#&gt; O10-E6      2   0.000      0.990 0.00 1.00
#&gt; O10-F1      1   0.000      0.981 1.00 0.00
#&gt; O10-F11     1   0.000      0.981 1.00 0.00
#&gt; O10-F12     2   0.141      0.973 0.02 0.98
#&gt; O10-F5      1   0.000      0.981 1.00 0.00
#&gt; O10-F9      2   0.000      0.990 0.00 1.00
#&gt; O12-B12     1   0.000      0.981 1.00 0.00
#&gt; O12-F12     1   0.000      0.981 1.00 0.00
#&gt; O7-A3       2   0.000      0.990 0.00 1.00
#&gt; O7-A9       1   0.000      0.981 1.00 0.00
#&gt; O7-B11      2   0.000      0.990 0.00 1.00
#&gt; O7-B6       2   0.000      0.990 0.00 1.00
#&gt; O7-B7       2   0.000      0.990 0.00 1.00
#&gt; O7-C1       1   0.000      0.981 1.00 0.00
#&gt; O7-C11      2   0.000      0.990 0.00 1.00
#&gt; O7-C7       2   0.000      0.990 0.00 1.00
#&gt; O7-D4       1   0.000      0.981 1.00 0.00
#&gt; O7-E10      2   0.000      0.990 0.00 1.00
#&gt; O7-E11      2   0.000      0.990 0.00 1.00
#&gt; O7-F2       1   0.000      0.981 1.00 0.00
#&gt; O7-F3       1   0.000      0.981 1.00 0.00
#&gt; O7-F7       2   0.000      0.990 0.00 1.00
#&gt; O7-F9       2   0.000      0.990 0.00 1.00
#&gt; O8-A2       1   0.000      0.981 1.00 0.00
#&gt; O8-A3       2   0.827      0.649 0.26 0.74
#&gt; O8-A8       2   0.242      0.955 0.04 0.96
#&gt; O8-B2       1   0.000      0.981 1.00 0.00
#&gt; O8-B4       2   0.000      0.990 0.00 1.00
#&gt; O8-B6       2   0.327      0.935 0.06 0.94
#&gt; O8-B7       2   0.000      0.990 0.00 1.00
#&gt; O8-B9       2   0.000      0.990 0.00 1.00
#&gt; O8-C11      1   0.904      0.543 0.68 0.32
#&gt; O8-C12      2   0.000      0.990 0.00 1.00
#&gt; O8-C3       1   0.827      0.662 0.74 0.26
#&gt; O8-C4       1   0.000      0.981 1.00 0.00
#&gt; O8-C5       2   0.000      0.990 0.00 1.00
#&gt; O8-C6       2   0.141      0.973 0.02 0.98
#&gt; O8-C8       2   0.000      0.990 0.00 1.00
#&gt; O8-D3       1   0.000      0.981 1.00 0.00
#&gt; O8-D5       1   0.000      0.981 1.00 0.00
#&gt; O8-D6       1   0.000      0.981 1.00 0.00
#&gt; O8-D9       1   0.000      0.981 1.00 0.00
#&gt; O8-E11      2   0.242      0.955 0.04 0.96
#&gt; O8-E2       1   0.000      0.981 1.00 0.00
#&gt; O8-E5       1   0.000      0.981 1.00 0.00
#&gt; O8-E9       2   0.000      0.990 0.00 1.00
#&gt; O8-F11      2   0.000      0.990 0.00 1.00
#&gt; O8-F12      2   0.000      0.990 0.00 1.00
#&gt; O8-F3       2   0.000      0.990 0.00 1.00
#&gt; O8-G1       1   0.000      0.981 1.00 0.00
#&gt; O8-G12      2   0.000      0.990 0.00 1.00
#&gt; O8-G5       2   0.000      0.990 0.00 1.00
#&gt; O9-A10      1   0.000      0.981 1.00 0.00
#&gt; O9-A8       1   0.000      0.981 1.00 0.00
#&gt; O9-B12      1   0.000      0.981 1.00 0.00
#&gt; O9-B2       1   0.000      0.981 1.00 0.00
#&gt; O9-B6       1   0.000      0.981 1.00 0.00
#&gt; O9-B7       1   0.000      0.981 1.00 0.00
#&gt; O9-C2       1   0.000      0.981 1.00 0.00
#&gt; O9-C4       1   0.000      0.981 1.00 0.00
#&gt; O9-C6       1   0.000      0.981 1.00 0.00
#&gt; O9-D12      1   0.000      0.981 1.00 0.00
#&gt; O9-D6       1   0.000      0.981 1.00 0.00
#&gt; O9-E12      1   0.000      0.981 1.00 0.00
#&gt; O9-E4       1   0.000      0.981 1.00 0.00
#&gt; O9-F9       1   0.000      0.981 1.00 0.00
#&gt; O9-G3       1   0.000      0.981 1.00 0.00
#&gt; O9-G7       1   0.000      0.981 1.00 0.00
#&gt; S37-A4      1   0.000      0.981 1.00 0.00
#&gt; S37-A7      1   0.000      0.981 1.00 0.00
#&gt; S37-B3      1   0.000      0.981 1.00 0.00
#&gt; S37-B5      1   0.000      0.981 1.00 0.00
#&gt; S37-B6      1   0.000      0.981 1.00 0.00
#&gt; S37-B9      1   0.000      0.981 1.00 0.00
#&gt; S37-D11     1   0.000      0.981 1.00 0.00
#&gt; S37-D9      1   0.000      0.981 1.00 0.00
#&gt; S37-E10     1   0.000      0.981 1.00 0.00
#&gt; S37-E5      1   0.000      0.981 1.00 0.00
#&gt; S37-G5      1   0.000      0.981 1.00 0.00
#&gt; S37-G8      1   0.000      0.981 1.00 0.00
#&gt; S37-H4      1   0.000      0.981 1.00 0.00
#&gt; S38-A3      1   0.000      0.981 1.00 0.00
#&gt; S38-B10     2   0.327      0.934 0.06 0.94
#&gt; S38-C1      2   0.469      0.889 0.10 0.90
#&gt; S38-C10     2   0.000      0.990 0.00 1.00
#&gt; S38-C11     2   0.000      0.990 0.00 1.00
#&gt; S38-C5      2   0.000      0.990 0.00 1.00
#&gt; S38-C6      1   0.000      0.981 1.00 0.00
#&gt; S38-C7      1   0.000      0.981 1.00 0.00
#&gt; S38-D10     2   0.000      0.990 0.00 1.00
#&gt; S38-D11     2   0.000      0.990 0.00 1.00
#&gt; S38-D4      1   0.000      0.981 1.00 0.00
#&gt; S38-D7      1   0.000      0.981 1.00 0.00
#&gt; S38-E2      1   0.000      0.981 1.00 0.00
#&gt; S38-E3      1   0.855      0.622 0.72 0.28
#&gt; S38-E8      1   0.000      0.981 1.00 0.00
#&gt; S38-F7      1   0.827      0.661 0.74 0.26
#&gt; S38-G7      2   0.000      0.990 0.00 1.00
#&gt; S38-G8      1   0.000      0.981 1.00 0.00
#&gt; S38-G9      2   0.000      0.990 0.00 1.00
#&gt; S38-H3      1   0.000      0.981 1.00 0.00
</code></pre>

<script>
$('#tab-node-03-get-classes-1-a').parent().next().next().hide();
$('#tab-node-03-get-classes-1-a').click(function(){
  $('#tab-node-03-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-03-get-classes-2'>
<p><a id='tab-node-03-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3
#&gt; O10-A11     1  0.1529      0.925 0.96 0.00 0.04
#&gt; O10-A5      2  0.5016      0.679 0.00 0.76 0.24
#&gt; O10-A7      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-A8      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-B10     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-B11     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-B2      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-B5      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-B6      1  0.5948      0.445 0.64 0.36 0.00
#&gt; O10-B7      2  0.0892      0.927 0.00 0.98 0.02
#&gt; O10-B9      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-C10     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-C12     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-C5      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-C6      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-C9      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-D1      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-D11     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-D12     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-D5      2  0.3340      0.838 0.00 0.88 0.12
#&gt; O10-D6      2  0.0892      0.927 0.00 0.98 0.02
#&gt; O10-D9      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-E1      2  0.0892      0.927 0.00 0.98 0.02
#&gt; O10-E11     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-E12     2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-E6      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O10-F1      1  0.0000      0.951 1.00 0.00 0.00
#&gt; O10-F11     1  0.0000      0.951 1.00 0.00 0.00
#&gt; O10-F12     2  0.4555      0.735 0.00 0.80 0.20
#&gt; O10-F5      1  0.0000      0.951 1.00 0.00 0.00
#&gt; O10-F9      3  0.2537      0.858 0.00 0.08 0.92
#&gt; O12-B12     1  0.0000      0.951 1.00 0.00 0.00
#&gt; O12-F12     1  0.0000      0.951 1.00 0.00 0.00
#&gt; O7-A3       2  0.3572      0.871 0.04 0.90 0.06
#&gt; O7-A9       1  0.0892      0.941 0.98 0.00 0.02
#&gt; O7-B11      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O7-B6       2  0.5216      0.631 0.00 0.74 0.26
#&gt; O7-B7       3  0.5835      0.524 0.00 0.34 0.66
#&gt; O7-C1       1  0.0892      0.941 0.98 0.00 0.02
#&gt; O7-C11      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O7-C7       2  0.1529      0.920 0.00 0.96 0.04
#&gt; O7-D4       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O7-E10      3  0.6302      0.124 0.00 0.48 0.52
#&gt; O7-E11      2  0.0000      0.939 0.00 1.00 0.00
#&gt; O7-F2       1  0.5397      0.613 0.72 0.00 0.28
#&gt; O7-F3       1  0.0892      0.941 0.98 0.00 0.02
#&gt; O7-F7       2  0.0892      0.927 0.00 0.98 0.02
#&gt; O7-F9       2  0.2959      0.860 0.00 0.90 0.10
#&gt; O8-A2       3  0.6244      0.158 0.44 0.00 0.56
#&gt; O8-A3       3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-A8       3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-B2       1  0.4555      0.747 0.80 0.00 0.20
#&gt; O8-B4       3  0.2959      0.845 0.00 0.10 0.90
#&gt; O8-B6       3  0.6229      0.631 0.02 0.28 0.70
#&gt; O8-B7       3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-B9       2  0.0000      0.939 0.00 1.00 0.00
#&gt; O8-C11      3  0.0892      0.869 0.02 0.00 0.98
#&gt; O8-C12      3  0.2066      0.866 0.00 0.06 0.94
#&gt; O8-C3       1  0.9593     -0.160 0.42 0.20 0.38
#&gt; O8-C4       1  0.4291      0.773 0.82 0.00 0.18
#&gt; O8-C5       3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-C6       3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-C8       3  0.5016      0.709 0.00 0.24 0.76
#&gt; O8-D3       3  0.0892      0.869 0.02 0.00 0.98
#&gt; O8-D5       3  0.0892      0.869 0.02 0.00 0.98
#&gt; O8-D6       1  0.2537      0.890 0.92 0.00 0.08
#&gt; O8-D9       1  0.0892      0.939 0.98 0.00 0.02
#&gt; O8-E11      3  0.1529      0.875 0.00 0.04 0.96
#&gt; O8-E2       1  0.0892      0.940 0.98 0.00 0.02
#&gt; O8-E5       1  0.2066      0.909 0.94 0.00 0.06
#&gt; O8-E9       3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-F11      2  0.6045      0.357 0.00 0.62 0.38
#&gt; O8-F12      3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-F3       3  0.4291      0.773 0.00 0.18 0.82
#&gt; O8-G1       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O8-G12      3  0.0892      0.880 0.00 0.02 0.98
#&gt; O8-G5       2  0.5016      0.673 0.00 0.76 0.24
#&gt; O9-A10      1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-A8       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-B12      1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-B2       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-B6       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-B7       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-C2       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-C4       1  0.0892      0.941 0.98 0.00 0.02
#&gt; O9-C6       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-D12      1  0.0892      0.941 0.98 0.00 0.02
#&gt; O9-D6       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-E12      1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-E4       1  0.0892      0.941 0.98 0.00 0.02
#&gt; O9-F9       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-G3       1  0.0000      0.951 1.00 0.00 0.00
#&gt; O9-G7       1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-A4      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-A7      1  0.1529      0.925 0.96 0.00 0.04
#&gt; S37-B3      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-B5      1  0.1529      0.925 0.96 0.00 0.04
#&gt; S37-B6      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-B9      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-D11     1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-D9      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-E10     1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-E5      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-G5      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-G8      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S37-H4      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S38-A3      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S38-B10     2  0.2414      0.894 0.04 0.94 0.02
#&gt; S38-C1      2  0.7884      0.490 0.10 0.64 0.26
#&gt; S38-C10     2  0.2066      0.909 0.00 0.94 0.06
#&gt; S38-C11     2  0.0000      0.939 0.00 1.00 0.00
#&gt; S38-C5      2  0.0000      0.939 0.00 1.00 0.00
#&gt; S38-C6      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S38-C7      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S38-D10     2  0.0000      0.939 0.00 1.00 0.00
#&gt; S38-D11     2  0.0892      0.927 0.00 0.98 0.02
#&gt; S38-D4      1  0.0892      0.941 0.98 0.00 0.02
#&gt; S38-D7      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S38-E2      1  0.3340      0.826 0.88 0.12 0.00
#&gt; S38-E3      2  0.4551      0.758 0.14 0.84 0.02
#&gt; S38-E8      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S38-F7      1  0.7464      0.268 0.56 0.40 0.04
#&gt; S38-G7      2  0.0892      0.927 0.00 0.98 0.02
#&gt; S38-G8      1  0.0000      0.951 1.00 0.00 0.00
#&gt; S38-G9      2  0.0000      0.939 0.00 1.00 0.00
#&gt; S38-H3      1  0.0892      0.941 0.98 0.00 0.02
</code></pre>

<script>
$('#tab-node-03-get-classes-2-a').parent().next().next().hide();
$('#tab-node-03-get-classes-2-a').click(function(){
  $('#tab-node-03-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-03-get-classes-3'>
<p><a id='tab-node-03-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;         class entropy silhouette   p1   p2   p3   p4
#&gt; O10-A11     1  0.1637     0.8735 0.94 0.00 0.06 0.00
#&gt; O10-A5      2  0.2706     0.8385 0.00 0.90 0.08 0.02
#&gt; O10-A7      2  0.2411     0.8593 0.00 0.92 0.04 0.04
#&gt; O10-A8      2  0.1211     0.8719 0.00 0.96 0.00 0.04
#&gt; O10-B10     2  0.0707     0.8783 0.00 0.98 0.00 0.02
#&gt; O10-B11     2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-B2      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-B5      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-B6      1  0.7004     0.2275 0.58 0.22 0.00 0.20
#&gt; O10-B7      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-B9      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-C10     2  0.0707     0.8766 0.00 0.98 0.02 0.00
#&gt; O10-C12     2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-C5      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-C6      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-C9      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-D1      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-D11     2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-D12     2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-D5      2  0.2647     0.8140 0.00 0.88 0.12 0.00
#&gt; O10-D6      2  0.1211     0.8703 0.00 0.96 0.04 0.00
#&gt; O10-D9      2  0.0707     0.8783 0.00 0.98 0.00 0.02
#&gt; O10-E1      2  0.2345     0.8425 0.00 0.90 0.00 0.10
#&gt; O10-E11     2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-E12     2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-E6      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O10-F1      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O10-F11     1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; O10-F12     2  0.3972     0.8012 0.00 0.84 0.08 0.08
#&gt; O10-F5      1  0.0707     0.8958 0.98 0.00 0.00 0.02
#&gt; O10-F9      3  0.5677     0.6731 0.00 0.14 0.72 0.14
#&gt; O12-B12     1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; O12-F12     1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; O7-A3       4  0.1411     0.7027 0.00 0.02 0.02 0.96
#&gt; O7-A9       4  0.4797     0.6797 0.26 0.00 0.02 0.72
#&gt; O7-B11      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O7-B6       2  0.2921     0.7982 0.00 0.86 0.14 0.00
#&gt; O7-B7       2  0.6336    -0.0826 0.00 0.48 0.46 0.06
#&gt; O7-C1       4  0.0707     0.7183 0.02 0.00 0.00 0.98
#&gt; O7-C11      2  0.0707     0.8783 0.00 0.98 0.00 0.02
#&gt; O7-C7       2  0.5570     0.3024 0.00 0.54 0.02 0.44
#&gt; O7-D4       1  0.2345     0.8428 0.90 0.00 0.00 0.10
#&gt; O7-E10      3  0.7310     0.3307 0.00 0.36 0.48 0.16
#&gt; O7-E11      2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O7-F2       1  0.5661     0.5697 0.70 0.00 0.22 0.08
#&gt; O7-F3       4  0.4642     0.6901 0.24 0.00 0.02 0.74
#&gt; O7-F7       2  0.3400     0.7845 0.00 0.82 0.00 0.18
#&gt; O7-F9       2  0.2345     0.8317 0.00 0.90 0.10 0.00
#&gt; O8-A2       3  0.6714     0.1640 0.36 0.00 0.54 0.10
#&gt; O8-A3       3  0.0000     0.8155 0.00 0.00 1.00 0.00
#&gt; O8-A8       3  0.0000     0.8155 0.00 0.00 1.00 0.00
#&gt; O8-B2       1  0.4642     0.6397 0.74 0.00 0.24 0.02
#&gt; O8-B4       3  0.3801     0.7038 0.00 0.22 0.78 0.00
#&gt; O8-B6       3  0.7168     0.5399 0.04 0.28 0.60 0.08
#&gt; O8-B7       3  0.0000     0.8155 0.00 0.00 1.00 0.00
#&gt; O8-B9       2  0.0000     0.8833 0.00 1.00 0.00 0.00
#&gt; O8-C11      3  0.1411     0.8026 0.02 0.00 0.96 0.02
#&gt; O8-C12      3  0.2345     0.7908 0.00 0.10 0.90 0.00
#&gt; O8-C3       1  0.7664     0.3133 0.58 0.14 0.24 0.04
#&gt; O8-C4       1  0.5327     0.6176 0.72 0.00 0.22 0.06
#&gt; O8-C5       3  0.0000     0.8155 0.00 0.00 1.00 0.00
#&gt; O8-C6       3  0.5291     0.6916 0.00 0.08 0.74 0.18
#&gt; O8-C8       3  0.4936     0.6289 0.00 0.28 0.70 0.02
#&gt; O8-D3       3  0.0707     0.8081 0.02 0.00 0.98 0.00
#&gt; O8-D5       3  0.0000     0.8155 0.00 0.00 1.00 0.00
#&gt; O8-D6       1  0.2011     0.8580 0.92 0.00 0.08 0.00
#&gt; O8-D9       1  0.3247     0.8344 0.88 0.00 0.06 0.06
#&gt; O8-E11      3  0.3522     0.7945 0.02 0.06 0.88 0.04
#&gt; O8-E2       1  0.1913     0.8784 0.94 0.00 0.04 0.02
#&gt; O8-E5       1  0.2011     0.8550 0.92 0.00 0.08 0.00
#&gt; O8-E9       3  0.0707     0.8176 0.00 0.02 0.98 0.00
#&gt; O8-F11      2  0.3400     0.7482 0.00 0.82 0.18 0.00
#&gt; O8-F12      3  0.1637     0.8106 0.00 0.06 0.94 0.00
#&gt; O8-F3       2  0.5606    -0.0443 0.00 0.50 0.48 0.02
#&gt; O8-G1       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O8-G12      3  0.0707     0.8176 0.00 0.02 0.98 0.00
#&gt; O8-G5       2  0.4949     0.6970 0.00 0.76 0.18 0.06
#&gt; O9-A10      1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; O9-A8       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-B12      1  0.2011     0.8563 0.92 0.00 0.00 0.08
#&gt; O9-B2       1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; O9-B6       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-B7       1  0.1637     0.8711 0.94 0.00 0.00 0.06
#&gt; O9-C2       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-C4       4  0.5606     0.2056 0.48 0.00 0.02 0.50
#&gt; O9-C6       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-D12      1  0.4994    -0.1250 0.52 0.00 0.00 0.48
#&gt; O9-D6       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-E12      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-E4       4  0.4936     0.6589 0.28 0.00 0.02 0.70
#&gt; O9-F9       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-G3       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; O9-G7       1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S37-A4      1  0.4277     0.5654 0.72 0.00 0.00 0.28
#&gt; S37-A7      1  0.2011     0.8586 0.92 0.00 0.08 0.00
#&gt; S37-B3      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S37-B5      1  0.2706     0.8566 0.90 0.00 0.02 0.08
#&gt; S37-B6      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S37-B9      1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; S37-D11     1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S37-D9      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S37-E10     1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; S37-E5      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S37-G5      1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; S37-G8      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S37-H4      1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; S38-A3      1  0.2647     0.8113 0.88 0.00 0.00 0.12
#&gt; S38-B10     4  0.3606     0.6117 0.00 0.14 0.02 0.84
#&gt; S38-C1      4  0.0707     0.7046 0.00 0.00 0.02 0.98
#&gt; S38-C10     2  0.4977     0.2800 0.00 0.54 0.00 0.46
#&gt; S38-C11     2  0.4284     0.7448 0.00 0.78 0.02 0.20
#&gt; S38-C5      2  0.1211     0.8711 0.00 0.96 0.00 0.04
#&gt; S38-C6      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S38-C7      4  0.4522     0.5813 0.32 0.00 0.00 0.68
#&gt; S38-D10     2  0.4406     0.6406 0.00 0.70 0.00 0.30
#&gt; S38-D11     4  0.2345     0.6556 0.00 0.10 0.00 0.90
#&gt; S38-D4      4  0.2706     0.7334 0.08 0.00 0.02 0.90
#&gt; S38-D7      1  0.3801     0.6707 0.78 0.00 0.00 0.22
#&gt; S38-E2      4  0.3400     0.7058 0.18 0.00 0.00 0.82
#&gt; S38-E3      4  0.0000     0.7056 0.00 0.00 0.00 1.00
#&gt; S38-E8      1  0.0707     0.8999 0.98 0.00 0.00 0.02
#&gt; S38-F7      4  0.7896     0.4518 0.30 0.18 0.02 0.50
#&gt; S38-G7      2  0.4855     0.4669 0.00 0.60 0.00 0.40
#&gt; S38-G8      1  0.0000     0.9035 1.00 0.00 0.00 0.00
#&gt; S38-G9      2  0.2647     0.8299 0.00 0.88 0.00 0.12
#&gt; S38-H3      4  0.2921     0.7349 0.14 0.00 0.00 0.86
</code></pre>

<script>
$('#tab-node-03-get-classes-3-a').parent().next().next().hide();
$('#tab-node-03-get-classes-3-a').click(function(){
  $('#tab-node-03-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-03-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-03-consensus-heatmap'>
<ul>
<li><a href='#tab-node-03-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-03-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-03-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-03-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-03-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-03-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-03-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-03-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-03-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-03-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-03-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-03-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-03-membership-heatmap'>
<ul>
<li><a href='#tab-node-03-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-03-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-03-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-03-membership-heatmap-1-1.png" alt="plot of chunk tab-node-03-membership-heatmap-1"/></p>

</div>
<div id='tab-node-03-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-03-membership-heatmap-2-1.png" alt="plot of chunk tab-node-03-membership-heatmap-2"/></p>

</div>
<div id='tab-node-03-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-03-membership-heatmap-3-1.png" alt="plot of chunk tab-node-03-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-03-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-03-get-signatures'>
<ul>
<li><a href='#tab-node-03-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-03-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-03-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-1-1.png" alt="plot of chunk tab-node-03-get-signatures-1"/></p>

</div>
<div id='tab-node-03-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-2-1.png" alt="plot of chunk tab-node-03-get-signatures-2"/></p>

</div>
<div id='tab-node-03-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-3-1.png" alt="plot of chunk tab-node-03-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-03-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-03-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-03-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-03-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-03-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-03-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-03-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-03-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-03-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-03-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-03-signature_compare](figure_cola/node-03-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-03-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-03-dimension-reduction'>
<ul>
<li><a href='#tab-node-03-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-03-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-03-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-03-dimension-reduction-1-1.png" alt="plot of chunk tab-node-03-dimension-reduction-1"/></p>

</div>
<div id='tab-node-03-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-03-dimension-reduction-2-1.png" alt="plot of chunk tab-node-03-dimension-reduction-2"/></p>

</div>
<div id='tab-node-03-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-03-dimension-reduction-3-1.png" alt="plot of chunk tab-node-03-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-03-collect-classes](figure_cola/node-03-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Age(p-value) Inferred.Cell.Type(p-value) k
#> ATC:skmeans      126     6.32e-03                    1.53e-01 2
#> ATC:skmeans      119     1.93e-03                    1.07e-01 3
#> ATC:skmeans      114     3.72e-05                    2.01e-06 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 4.1.0 (2021-05-18)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS/LAPACK: /usr/lib64/libopenblas-r0.3.3.so
#> 
#> locale:
#>  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C               LC_TIME=en_US.UTF-8       
#>  [4] LC_COLLATE=en_US.UTF-8     LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
#>  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#>  [1] grid      parallel  stats4    stats     graphics  grDevices utils     datasets  methods  
#> [10] base     
#> 
#> other attached packages:
#>  [1] genefilter_1.74.0           ComplexHeatmap_2.8.0        markdown_1.1               
#>  [4] knitr_1.33                  scRNAseq_2.6.1              SingleCellExperiment_1.14.1
#>  [7] SummarizedExperiment_1.22.0 Biobase_2.52.0              GenomicRanges_1.44.0       
#> [10] GenomeInfoDb_1.28.1         IRanges_2.26.0              S4Vectors_0.30.0           
#> [13] BiocGenerics_0.38.0         MatrixGenerics_1.4.0        matrixStats_0.59.0         
#> [16] cola_1.9.4                 
#> 
#> loaded via a namespace (and not attached):
#>   [1] circlize_0.4.13               AnnotationHub_3.0.1           BiocFileCache_2.0.0          
#>   [4] lazyeval_0.2.2                polylabelr_0.2.0              splines_4.1.0                
#>   [7] Polychrome_1.3.1              BiocParallel_1.26.1           ggplot2_3.3.5                
#>  [10] digest_0.6.27                 foreach_1.5.1                 ensembldb_2.16.3             
#>  [13] htmltools_0.5.1.1             viridis_0.6.1                 fansi_0.5.0                  
#>  [16] magrittr_2.0.1                memoise_2.0.0                 cluster_2.1.2                
#>  [19] doParallel_1.0.16             Biostrings_2.60.1             annotate_1.70.0              
#>  [22] askpass_1.1                   prettyunits_1.1.1             colorspace_2.0-2             
#>  [25] blob_1.2.1                    rappdirs_0.3.3                xfun_0.24                    
#>  [28] dplyr_1.0.7                   crayon_1.4.1                  RCurl_1.98-1.3               
#>  [31] microbenchmark_1.4-7          jsonlite_1.7.2                impute_1.66.0                
#>  [34] brew_1.0-6                    survival_3.2-11               iterators_1.0.13             
#>  [37] glue_1.4.2                    polyclip_1.10-0               gtable_0.3.0                 
#>  [40] zlibbioc_1.38.0               XVector_0.32.0                GetoptLong_1.0.5             
#>  [43] DelayedArray_0.18.0           shape_1.4.6                   scales_1.1.1                 
#>  [46] data.tree_1.0.0               DBI_1.1.1                     Rcpp_1.0.7                   
#>  [49] viridisLite_0.4.0             xtable_1.8-4                  progress_1.2.2               
#>  [52] clue_0.3-59                   reticulate_1.20               bit_4.0.4                    
#>  [55] mclust_5.4.7                  umap_0.2.7.0                  httr_1.4.2                   
#>  [58] RColorBrewer_1.1-2            ellipsis_0.3.2                pkgconfig_2.0.3              
#>  [61] XML_3.99-0.6                  dbplyr_2.1.1                  utf8_1.2.1                   
#>  [64] tidyselect_1.1.1              rlang_0.4.11                  later_1.2.0                  
#>  [67] AnnotationDbi_1.54.1          munsell_0.5.0                 BiocVersion_3.13.1           
#>  [70] tools_4.1.0                   cachem_1.0.5                  generics_0.1.0               
#>  [73] RSQLite_2.2.7                 ExperimentHub_2.0.0           evaluate_0.14                
#>  [76] stringr_1.4.0                 fastmap_1.1.0                 yaml_2.2.1                   
#>  [79] bit64_4.0.5                   purrr_0.3.4                   dendextend_1.15.1            
#>  [82] KEGGREST_1.32.0               AnnotationFilter_1.16.0       mime_0.11                    
#>  [85] slam_0.1-48                   xml2_1.3.2                    biomaRt_2.48.2               
#>  [88] compiler_4.1.0                rstudioapi_0.13               filelock_1.0.2               
#>  [91] curl_4.3.2                    png_0.1-7                     interactiveDisplayBase_1.30.0
#>  [94] tibble_3.1.2                  stringi_1.7.3                 highr_0.9                    
#>  [97] GenomicFeatures_1.44.0        RSpectra_0.16-0               lattice_0.20-44              
#> [100] ProtGenerics_1.24.0           Matrix_1.3-4                  vctrs_0.3.8                  
#> [103] pillar_1.6.1                  lifecycle_1.0.0               BiocManager_1.30.16          
#> [106] eulerr_6.1.0                  GlobalOptions_0.1.2           bitops_1.0-7                 
#> [109] irlba_2.3.3                   httpuv_1.6.1                  rtracklayer_1.52.0           
#> [112] R6_2.5.0                      BiocIO_1.2.0                  promises_1.2.0.1             
#> [115] gridExtra_2.3                 codetools_0.2-18              assertthat_0.2.1             
#> [118] openssl_1.4.4                 rjson_0.2.20                  GenomicAlignments_1.28.0     
#> [121] Rsamtools_2.8.0               GenomeInfoDbData_1.2.6        hms_1.1.0                    
#> [124] skmeans_0.2-13                Cairo_1.5-12.2                scatterplot3d_0.3-41         
#> [127] shiny_1.6.0                   restfulr_0.0.13
```




