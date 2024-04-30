---
title: Gene Count Boxplots
author: Kesava Asam
date: '2024-04-30'
slug: []
categories:
  - plots
tags:
  - rnaseq
  - genecounts
subtitle: ''
excerpt: ''
draft: yes
series: ~
layout: single
---

# Packages

``` r
# Load required packages
library(tidyverse) # Data wrangling
library(janitor) # Df cleanup
library(gt) # for displaying data tables in a pdf
library(DESeq2) # Change data to matrix
library(here) # Create customized upset plot
```

# Preface

This document shows how to generate Boxplots for top DEG using the Variance stabilized counts from RNA seq results.

Required files for Gene counts Boxplots:

- Metadata/phenotype file --> metadata

- VST Normalized counts --> vst_counts

- DEG results --> deg_res (Note these are arranged based on Pvalue)

# Gene Count Boxplots

## Load

Load DEG results, VST stabilized gene counts, and metadata

``` r
# Loading example RData file that has required files
load(here("content/blog/2024-04-30-gene-count-boxplots/data/example_data.RData"))
```

## Checks

Check how the data is formatted

``` r
metadata %>% 
  head(5) %>% 
  gt() 
```

<div id="eqorpspgio" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#eqorpspgio table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#eqorpspgio thead, #eqorpspgio tbody, #eqorpspgio tfoot, #eqorpspgio tr, #eqorpspgio td, #eqorpspgio th {
  border-style: none;
}
&#10;#eqorpspgio p {
  margin: 0;
  padding: 0;
}
&#10;#eqorpspgio .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#eqorpspgio .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}
&#10;#eqorpspgio .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}
&#10;#eqorpspgio .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}
&#10;#eqorpspgio .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}
&#10;#eqorpspgio .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#eqorpspgio .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#eqorpspgio .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}
&#10;#eqorpspgio .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#eqorpspgio .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}
&#10;#eqorpspgio .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}
&#10;#eqorpspgio .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#eqorpspgio .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#eqorpspgio .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}
&#10;#eqorpspgio .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#eqorpspgio .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}
&#10;#eqorpspgio .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#eqorpspgio .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#eqorpspgio .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#eqorpspgio .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#eqorpspgio .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#eqorpspgio .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#eqorpspgio .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#eqorpspgio .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#eqorpspgio .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#eqorpspgio .gt_left {
  text-align: left;
}
&#10;#eqorpspgio .gt_center {
  text-align: center;
}
&#10;#eqorpspgio .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#eqorpspgio .gt_font_normal {
  font-weight: normal;
}
&#10;#eqorpspgio .gt_font_bold {
  font-weight: bold;
}
&#10;#eqorpspgio .gt_font_italic {
  font-style: italic;
}
&#10;#eqorpspgio .gt_super {
  font-size: 65%;
}
&#10;#eqorpspgio .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#eqorpspgio .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#eqorpspgio .gt_indent_1 {
  text-indent: 5px;
}
&#10;#eqorpspgio .gt_indent_2 {
  text-indent: 10px;
}
&#10;#eqorpspgio .gt_indent_3 {
  text-indent: 15px;
}
&#10;#eqorpspgio .gt_indent_4 {
  text-indent: 20px;
}
&#10;#eqorpspgio .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    &#10;    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="sample">sample</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="condition">condition</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="sample" class="gt_row gt_left">SRR17547999</td>
<td headers="condition" class="gt_row gt_center">normal</td></tr>
    <tr><td headers="sample" class="gt_row gt_left">SRR17548000</td>
<td headers="condition" class="gt_row gt_center">normal</td></tr>
    <tr><td headers="sample" class="gt_row gt_left">SRR17548001</td>
<td headers="condition" class="gt_row gt_center">normal</td></tr>
    <tr><td headers="sample" class="gt_row gt_left">SRR17548002</td>
<td headers="condition" class="gt_row gt_center">normal</td></tr>
    <tr><td headers="sample" class="gt_row gt_left">SRR17548003</td>
<td headers="condition" class="gt_row gt_center">normal</td></tr>
  </tbody>
  &#10;  
</table>
</div>

``` r
vst_counts
```

    ## class: DESeqTransform 
    ## dim: 13602 12 
    ## metadata(2): version annotation
    ## assays(1): ''
    ## rownames(13602): 57801 26155 ... 285093 3120
    ## rowData names(23): ENSEMBL baseMean ... maxCooks dispFit
    ## colnames(12): SRR17547999 SRR17548000 ... SRR17548012 SRR17548009
    ## colData names(5): Age BMI disease_state condition sizeFactor

vst_counts need to be converted to a tibble with rownames to column

``` r
deg_res[1:5, 1:5] %>% 
  gt() 
```

<div id="oagqiggmwh" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#oagqiggmwh table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#oagqiggmwh thead, #oagqiggmwh tbody, #oagqiggmwh tfoot, #oagqiggmwh tr, #oagqiggmwh td, #oagqiggmwh th {
  border-style: none;
}
&#10;#oagqiggmwh p {
  margin: 0;
  padding: 0;
}
&#10;#oagqiggmwh .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#oagqiggmwh .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}
&#10;#oagqiggmwh .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}
&#10;#oagqiggmwh .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}
&#10;#oagqiggmwh .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}
&#10;#oagqiggmwh .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#oagqiggmwh .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#oagqiggmwh .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}
&#10;#oagqiggmwh .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#oagqiggmwh .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}
&#10;#oagqiggmwh .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}
&#10;#oagqiggmwh .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#oagqiggmwh .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#oagqiggmwh .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}
&#10;#oagqiggmwh .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#oagqiggmwh .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}
&#10;#oagqiggmwh .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#oagqiggmwh .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#oagqiggmwh .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#oagqiggmwh .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#oagqiggmwh .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#oagqiggmwh .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#oagqiggmwh .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#oagqiggmwh .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#oagqiggmwh .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#oagqiggmwh .gt_left {
  text-align: left;
}
&#10;#oagqiggmwh .gt_center {
  text-align: center;
}
&#10;#oagqiggmwh .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#oagqiggmwh .gt_font_normal {
  font-weight: normal;
}
&#10;#oagqiggmwh .gt_font_bold {
  font-weight: bold;
}
&#10;#oagqiggmwh .gt_font_italic {
  font-style: italic;
}
&#10;#oagqiggmwh .gt_super {
  font-size: 65%;
}
&#10;#oagqiggmwh .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#oagqiggmwh .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#oagqiggmwh .gt_indent_1 {
  text-indent: 5px;
}
&#10;#oagqiggmwh .gt_indent_2 {
  text-indent: 10px;
}
&#10;#oagqiggmwh .gt_indent_3 {
  text-indent: 15px;
}
&#10;#oagqiggmwh .gt_indent_4 {
  text-indent: 20px;
}
&#10;#oagqiggmwh .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    &#10;    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="ENTREZID">ENTREZID</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="SYMBOL">SYMBOL</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="log2FoldChange">log2FoldChange</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="pvalue">pvalue</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="padj">padj</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="ENTREZID" class="gt_row gt_right">64428</td>
<td headers="SYMBOL" class="gt_row gt_left">CIAO3</td>
<td headers="log2FoldChange" class="gt_row gt_right">8.163592</td>
<td headers="pvalue" class="gt_row gt_right">5.372054e-14</td>
<td headers="padj" class="gt_row gt_right">4.050118e-10</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">677767</td>
<td headers="SYMBOL" class="gt_row gt_left">SCARNA7</td>
<td headers="log2FoldChange" class="gt_row gt_right">-1.885258</td>
<td headers="pvalue" class="gt_row gt_right">5.963071e-14</td>
<td headers="padj" class="gt_row gt_right">4.050118e-10</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">400720</td>
<td headers="SYMBOL" class="gt_row gt_left">ZNF772</td>
<td headers="log2FoldChange" class="gt_row gt_right">-1.435568</td>
<td headers="pvalue" class="gt_row gt_right">9.515986e-14</td>
<td headers="padj" class="gt_row gt_right">4.308838e-10</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">125875</td>
<td headers="SYMBOL" class="gt_row gt_left">CLDND2</td>
<td headers="log2FoldChange" class="gt_row gt_right">2.140854</td>
<td headers="pvalue" class="gt_row gt_right">6.641797e-12</td>
<td headers="padj" class="gt_row gt_right">2.255554e-08</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">124093</td>
<td headers="SYMBOL" class="gt_row gt_left">CCDC78</td>
<td headers="log2FoldChange" class="gt_row gt_right">8.249736</td>
<td headers="pvalue" class="gt_row gt_right">1.165447e-10</td>
<td headers="padj" class="gt_row gt_right">3.166287e-07</td></tr>
  </tbody>
  &#10;  
</table>
</div>
## Data prep

``` r
# convert the VST Normalized counts into a dataframe
vst_res_df <- 
  assay(vst_counts) %>% 
  as_tibble(rownames = "ENTREZID")

# make a df for expression plots
expr_df <- 
  deg_res %>% 
  # join the DEG with the variance stabilized counts using Entrez ID column
  inner_join(vst_res_df, by = "ENTREZID") %>% 
  # Remove unneccessary columns
  dplyr::select(-c(ENTREZID, log2FoldChange, pvalue, 
                   padj, qvalue, baseMean, lfcSE, GENENAME)) %>% 
  # Pick top 10 DEG
  head(10) %>% 
  # pivot longer to get all normalized counts to one column
  pivot_longer(!SYMBOL, names_to = "sample", values_to = "norm_counts") %>% 
  # Join with metadata using sample column
  inner_join(metadata, by = "sample")
```

## Rank genes

To show the genes based on the pvalue instead of alphabetical. Get the genes and relevel in the next step

``` r
# This will be used to relevel the gene list
gene_rank <- 
  deg_res %>% 
  head(10) %>% 
  pull(SYMBOL)
```

## Make Plot

``` r
gc_boxplot <- 
  expr_df %>% 
  mutate(SYMBOL = fct_relevel(SYMBOL, gene_rank)) %>% 
  mutate(condition = as.factor(condition)) %>%
  ggplot(aes(x = SYMBOL, y = norm_counts, fill = condition)) +
  
  geom_point(size = 3.5, alpha = 0.3, shape = 21, 
             position = position_dodge(0.55)) +
  
  stat_summary(fun.data = "mean_cl_boot", alpha = 0.8,
               geom = "crossbar", width = 0.5, color = "black",
               position = position_dodge(0.55)) +
  
  scale_color_manual(values = c("darkorchid4", "gold2")) +
  scale_fill_manual(values = c("darkorchid4", "gold2")) +
  
  labs(
    title = "Expression Box Plot of Top DEG", 
    subtitle = "Variance stabilized transformed gene counts were plotted",
    x = "", 
    y = "Counts"
  ) +
  theme_bw() +
  theme(legend.position = "bottom", 
        panel.grid.minor = element_line(linewidth =  0.1), 
        panel.grid.major = element_line(linewidth =  0.4, color = "gray85"))
```

``` r
gc_boxplot
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="960" />

``` r
sessionInfo()
```

    ## R version 4.3.0 (2023-04-21)
    ## Platform: aarch64-apple-darwin20 (64-bit)
    ## Running under: macOS 14.4.1
    ## 
    ## Matrix products: default
    ## BLAS:   /Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/lib/libRblas.0.dylib 
    ## LAPACK: /Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/lib/libRlapack.dylib;  LAPACK version 3.11.0
    ## 
    ## locale:
    ## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
    ## 
    ## time zone: Asia/Kolkata
    ## tzcode source: internal
    ## 
    ## attached base packages:
    ## [1] stats4    stats     graphics  grDevices utils     datasets  methods  
    ## [8] base     
    ## 
    ## other attached packages:
    ##  [1] here_1.0.1                  DESeq2_1.40.2              
    ##  [3] SummarizedExperiment_1.30.2 Biobase_2.60.0             
    ##  [5] MatrixGenerics_1.12.3       matrixStats_1.1.0          
    ##  [7] GenomicRanges_1.52.0        GenomeInfoDb_1.36.3        
    ##  [9] IRanges_2.34.1              S4Vectors_0.38.2           
    ## [11] BiocGenerics_0.46.0         gt_0.10.0                  
    ## [13] janitor_2.2.0               lubridate_1.9.3            
    ## [15] forcats_1.0.0               stringr_1.5.1              
    ## [17] dplyr_1.1.4                 purrr_1.0.2                
    ## [19] readr_2.1.4                 tidyr_1.3.0                
    ## [21] tibble_3.2.1                ggplot2_3.4.4              
    ## [23] tidyverse_2.0.0            
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] tidyselect_1.2.0        farver_2.1.1            bitops_1.0-7           
    ##  [4] fastmap_1.1.1           RCurl_1.98-1.13         blogdown_1.19          
    ##  [7] rpart_4.1.21            digest_0.6.33           timechange_0.2.0       
    ## [10] lifecycle_1.0.4         cluster_2.1.4           magrittr_2.0.3         
    ## [13] compiler_4.3.0          rlang_1.1.2             Hmisc_5.1-1            
    ## [16] sass_0.4.7              tools_4.3.0             utf8_1.2.4             
    ## [19] yaml_2.3.7              data.table_1.14.8       knitr_1.45             
    ## [22] labeling_0.4.3          htmlwidgets_1.6.2       S4Arrays_1.0.6         
    ## [25] DelayedArray_0.26.7     xml2_1.3.5              abind_1.4-5            
    ## [28] BiocParallel_1.34.2     withr_2.5.2             foreign_0.8-85         
    ## [31] nnet_7.3-19             grid_4.3.0              fansi_1.0.5            
    ## [34] colorspace_2.1-0        scales_1.2.1            cli_3.6.1              
    ## [37] rmarkdown_2.25          crayon_1.5.2            generics_0.1.3         
    ## [40] rstudioapi_0.15.0       tzdb_0.4.0              cachem_1.0.8           
    ## [43] zlibbioc_1.46.0         parallel_4.3.0          XVector_0.40.0         
    ## [46] base64enc_0.1-3         vctrs_0.6.4             Matrix_1.6-3           
    ## [49] jsonlite_1.8.7          bookdown_0.39           hms_1.1.3              
    ## [52] htmlTable_2.4.2         Formula_1.2-5           locfit_1.5-9.8         
    ## [55] jquerylib_0.1.4         glue_1.6.2              codetools_0.2-19       
    ## [58] stringi_1.8.1           gtable_0.3.4            munsell_0.5.0          
    ## [61] pillar_1.9.0            htmltools_0.5.7         GenomeInfoDbData_1.2.10
    ## [64] R6_2.5.1                rprojroot_2.0.4         evaluate_0.23          
    ## [67] lattice_0.22-5          highr_0.10              backports_1.4.1        
    ## [70] snakecase_0.11.1        bslib_0.5.1             Rcpp_1.0.11            
    ## [73] checkmate_2.3.0         gridExtra_2.3           xfun_0.43              
    ## [76] pkgconfig_2.0.3
