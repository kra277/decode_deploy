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
---

# Preface

When Differential gene expression between two groups is computed it may be good idea to visualize the top DEG gene count distribution between the groups.

This document shows how to generate Boxplots for the top DEG using the Variance stabilized counts from RNA seq results.

Required files for Gene counts Boxplots:

- Metadata/phenotype file --> metadata

- VST Normalized counts --> vst_counts

- DEG results --> deg_res (The top few differentially expressed genes)

``` r
# Load required packages
library(tidyverse) # Data wrangling and plotting
library(gt) # for displaying data tables in a pdf
library(here) # path requirements
```

## Load

Load DEG results, VST stabilized gene counts, and metadata

``` r
# get the working dir path
wrk_path <- here("content/blog/2024-04-30-gc-plots-v2/")

top_deg <- read.csv(paste0(wrk_path, "top_deg.csv"))
metadata <- read.csv(paste0(wrk_path, "metadata.csv"))
vst_cts <- read.csv(paste0(wrk_path, "vst_counts.csv"))
```

# Check

Check each file on how the data appears

## Metadata

``` r
metadata %>% 
  gt()
```

<div id="ykuqjajfbw" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#ykuqjajfbw table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#ykuqjajfbw thead, #ykuqjajfbw tbody, #ykuqjajfbw tfoot, #ykuqjajfbw tr, #ykuqjajfbw td, #ykuqjajfbw th {
  border-style: none;
}
&#10;#ykuqjajfbw p {
  margin: 0;
  padding: 0;
}
&#10;#ykuqjajfbw .gt_table {
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
&#10;#ykuqjajfbw .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#ykuqjajfbw .gt_title {
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
&#10;#ykuqjajfbw .gt_subtitle {
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
&#10;#ykuqjajfbw .gt_heading {
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
&#10;#ykuqjajfbw .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#ykuqjajfbw .gt_col_headings {
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
&#10;#ykuqjajfbw .gt_col_heading {
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
&#10;#ykuqjajfbw .gt_column_spanner_outer {
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
&#10;#ykuqjajfbw .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#ykuqjajfbw .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#ykuqjajfbw .gt_column_spanner {
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
&#10;#ykuqjajfbw .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#ykuqjajfbw .gt_group_heading {
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
&#10;#ykuqjajfbw .gt_empty_group_heading {
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
&#10;#ykuqjajfbw .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#ykuqjajfbw .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#ykuqjajfbw .gt_row {
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
&#10;#ykuqjajfbw .gt_stub {
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
&#10;#ykuqjajfbw .gt_stub_row_group {
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
&#10;#ykuqjajfbw .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#ykuqjajfbw .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#ykuqjajfbw .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#ykuqjajfbw .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#ykuqjajfbw .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#ykuqjajfbw .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#ykuqjajfbw .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#ykuqjajfbw .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#ykuqjajfbw .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#ykuqjajfbw .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#ykuqjajfbw .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#ykuqjajfbw .gt_footnotes {
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
&#10;#ykuqjajfbw .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#ykuqjajfbw .gt_sourcenotes {
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
&#10;#ykuqjajfbw .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#ykuqjajfbw .gt_left {
  text-align: left;
}
&#10;#ykuqjajfbw .gt_center {
  text-align: center;
}
&#10;#ykuqjajfbw .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#ykuqjajfbw .gt_font_normal {
  font-weight: normal;
}
&#10;#ykuqjajfbw .gt_font_bold {
  font-weight: bold;
}
&#10;#ykuqjajfbw .gt_font_italic {
  font-style: italic;
}
&#10;#ykuqjajfbw .gt_super {
  font-size: 65%;
}
&#10;#ykuqjajfbw .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#ykuqjajfbw .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#ykuqjajfbw .gt_indent_1 {
  text-indent: 5px;
}
&#10;#ykuqjajfbw .gt_indent_2 {
  text-indent: 10px;
}
&#10;#ykuqjajfbw .gt_indent_3 {
  text-indent: 15px;
}
&#10;#ykuqjajfbw .gt_indent_4 {
  text-indent: 20px;
}
&#10;#ykuqjajfbw .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    &#10;    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="X">X</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="sample">sample</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="condition">condition</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="X" class="gt_row gt_right">1</td>
<td headers="sample" class="gt_row gt_left">SRR17547999</td>
<td headers="condition" class="gt_row gt_left">normal</td></tr>
    <tr><td headers="X" class="gt_row gt_right">2</td>
<td headers="sample" class="gt_row gt_left">SRR17548000</td>
<td headers="condition" class="gt_row gt_left">normal</td></tr>
    <tr><td headers="X" class="gt_row gt_right">3</td>
<td headers="sample" class="gt_row gt_left">SRR17548001</td>
<td headers="condition" class="gt_row gt_left">normal</td></tr>
    <tr><td headers="X" class="gt_row gt_right">4</td>
<td headers="sample" class="gt_row gt_left">SRR17548002</td>
<td headers="condition" class="gt_row gt_left">normal</td></tr>
    <tr><td headers="X" class="gt_row gt_right">5</td>
<td headers="sample" class="gt_row gt_left">SRR17548003</td>
<td headers="condition" class="gt_row gt_left">normal</td></tr>
    <tr><td headers="X" class="gt_row gt_right">6</td>
<td headers="sample" class="gt_row gt_left">SRR17548004</td>
<td headers="condition" class="gt_row gt_left">normal</td></tr>
    <tr><td headers="X" class="gt_row gt_right">7</td>
<td headers="sample" class="gt_row gt_left">SRR17548005</td>
<td headers="condition" class="gt_row gt_left">prediabetes</td></tr>
    <tr><td headers="X" class="gt_row gt_right">8</td>
<td headers="sample" class="gt_row gt_left">SRR17548006</td>
<td headers="condition" class="gt_row gt_left">prediabetes</td></tr>
    <tr><td headers="X" class="gt_row gt_right">9</td>
<td headers="sample" class="gt_row gt_left">SRR17548007</td>
<td headers="condition" class="gt_row gt_left">prediabetes</td></tr>
    <tr><td headers="X" class="gt_row gt_right">10</td>
<td headers="sample" class="gt_row gt_left">SRR17548008</td>
<td headers="condition" class="gt_row gt_left">prediabetes</td></tr>
    <tr><td headers="X" class="gt_row gt_right">11</td>
<td headers="sample" class="gt_row gt_left">SRR17548012</td>
<td headers="condition" class="gt_row gt_left">prediabetes</td></tr>
    <tr><td headers="X" class="gt_row gt_right">12</td>
<td headers="sample" class="gt_row gt_left">SRR17548009</td>
<td headers="condition" class="gt_row gt_left">prediabetes</td></tr>
  </tbody>
  &#10;  
</table>
</div>

There are two conditions in the data Prediabetes and Normal.

## Top DEG Res

``` r
top_deg %>% 
  select(ENTREZID, SYMBOL, log2FoldChange, pvalue, padj, qvalue) %>% 
  gt()
```

<div id="sdtewmaqrc" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#sdtewmaqrc table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#sdtewmaqrc thead, #sdtewmaqrc tbody, #sdtewmaqrc tfoot, #sdtewmaqrc tr, #sdtewmaqrc td, #sdtewmaqrc th {
  border-style: none;
}
&#10;#sdtewmaqrc p {
  margin: 0;
  padding: 0;
}
&#10;#sdtewmaqrc .gt_table {
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
&#10;#sdtewmaqrc .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#sdtewmaqrc .gt_title {
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
&#10;#sdtewmaqrc .gt_subtitle {
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
&#10;#sdtewmaqrc .gt_heading {
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
&#10;#sdtewmaqrc .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#sdtewmaqrc .gt_col_headings {
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
&#10;#sdtewmaqrc .gt_col_heading {
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
&#10;#sdtewmaqrc .gt_column_spanner_outer {
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
&#10;#sdtewmaqrc .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#sdtewmaqrc .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#sdtewmaqrc .gt_column_spanner {
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
&#10;#sdtewmaqrc .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#sdtewmaqrc .gt_group_heading {
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
&#10;#sdtewmaqrc .gt_empty_group_heading {
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
&#10;#sdtewmaqrc .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#sdtewmaqrc .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#sdtewmaqrc .gt_row {
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
&#10;#sdtewmaqrc .gt_stub {
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
&#10;#sdtewmaqrc .gt_stub_row_group {
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
&#10;#sdtewmaqrc .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#sdtewmaqrc .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#sdtewmaqrc .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#sdtewmaqrc .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#sdtewmaqrc .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#sdtewmaqrc .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#sdtewmaqrc .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#sdtewmaqrc .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#sdtewmaqrc .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#sdtewmaqrc .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#sdtewmaqrc .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#sdtewmaqrc .gt_footnotes {
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
&#10;#sdtewmaqrc .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#sdtewmaqrc .gt_sourcenotes {
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
&#10;#sdtewmaqrc .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#sdtewmaqrc .gt_left {
  text-align: left;
}
&#10;#sdtewmaqrc .gt_center {
  text-align: center;
}
&#10;#sdtewmaqrc .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#sdtewmaqrc .gt_font_normal {
  font-weight: normal;
}
&#10;#sdtewmaqrc .gt_font_bold {
  font-weight: bold;
}
&#10;#sdtewmaqrc .gt_font_italic {
  font-style: italic;
}
&#10;#sdtewmaqrc .gt_super {
  font-size: 65%;
}
&#10;#sdtewmaqrc .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#sdtewmaqrc .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#sdtewmaqrc .gt_indent_1 {
  text-indent: 5px;
}
&#10;#sdtewmaqrc .gt_indent_2 {
  text-indent: 10px;
}
&#10;#sdtewmaqrc .gt_indent_3 {
  text-indent: 15px;
}
&#10;#sdtewmaqrc .gt_indent_4 {
  text-indent: 20px;
}
&#10;#sdtewmaqrc .gt_indent_5 {
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
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="qvalue">qvalue</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="ENTREZID" class="gt_row gt_right">64428</td>
<td headers="SYMBOL" class="gt_row gt_left">CIAO3</td>
<td headers="log2FoldChange" class="gt_row gt_right">8.163592</td>
<td headers="pvalue" class="gt_row gt_right">5.372054e-14</td>
<td headers="padj" class="gt_row gt_right">4.050118e-10</td>
<td headers="qvalue" class="gt_row gt_right">3.441228e-10</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">677767</td>
<td headers="SYMBOL" class="gt_row gt_left">SCARNA7</td>
<td headers="log2FoldChange" class="gt_row gt_right">-1.885258</td>
<td headers="pvalue" class="gt_row gt_right">5.963071e-14</td>
<td headers="padj" class="gt_row gt_right">4.050118e-10</td>
<td headers="qvalue" class="gt_row gt_right">3.441228e-10</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">400720</td>
<td headers="SYMBOL" class="gt_row gt_left">ZNF772</td>
<td headers="log2FoldChange" class="gt_row gt_right">-1.435568</td>
<td headers="pvalue" class="gt_row gt_right">9.515986e-14</td>
<td headers="padj" class="gt_row gt_right">4.308838e-10</td>
<td headers="qvalue" class="gt_row gt_right">3.661053e-10</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">125875</td>
<td headers="SYMBOL" class="gt_row gt_left">CLDND2</td>
<td headers="log2FoldChange" class="gt_row gt_right">2.140854</td>
<td headers="pvalue" class="gt_row gt_right">6.641797e-12</td>
<td headers="padj" class="gt_row gt_right">2.255554e-08</td>
<td headers="qvalue" class="gt_row gt_right">1.916457e-08</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">124093</td>
<td headers="SYMBOL" class="gt_row gt_left">CCDC78</td>
<td headers="log2FoldChange" class="gt_row gt_right">8.249736</td>
<td headers="pvalue" class="gt_row gt_right">1.165447e-10</td>
<td headers="padj" class="gt_row gt_right">3.166287e-07</td>
<td headers="qvalue" class="gt_row gt_right">2.690271e-07</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">84264</td>
<td headers="SYMBOL" class="gt_row gt_left">HAGHL</td>
<td headers="log2FoldChange" class="gt_row gt_right">8.302630</td>
<td headers="pvalue" class="gt_row gt_right">1.447807e-10</td>
<td headers="padj" class="gt_row gt_right">3.277835e-07</td>
<td headers="qvalue" class="gt_row gt_right">2.785049e-07</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">79006</td>
<td headers="SYMBOL" class="gt_row gt_left">METRN</td>
<td headers="log2FoldChange" class="gt_row gt_right">7.018007</td>
<td headers="pvalue" class="gt_row gt_right">6.857968e-09</td>
<td headers="padj" class="gt_row gt_right">1.306349e-05</td>
<td headers="qvalue" class="gt_row gt_right">1.109954e-05</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">65990</td>
<td headers="SYMBOL" class="gt_row gt_left">ANTKMT</td>
<td headers="log2FoldChange" class="gt_row gt_right">6.724028</td>
<td headers="pvalue" class="gt_row gt_right">7.693455e-09</td>
<td headers="padj" class="gt_row gt_right">1.306349e-05</td>
<td headers="qvalue" class="gt_row gt_right">1.109954e-05</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">677777</td>
<td headers="SYMBOL" class="gt_row gt_left">SCARNA12</td>
<td headers="log2FoldChange" class="gt_row gt_right">-1.569236</td>
<td headers="pvalue" class="gt_row gt_right">5.958978e-08</td>
<td headers="padj" class="gt_row gt_right">8.994084e-05</td>
<td headers="qvalue" class="gt_row gt_right">7.641924e-05</td></tr>
    <tr><td headers="ENTREZID" class="gt_row gt_right">677775</td>
<td headers="SYMBOL" class="gt_row gt_left">SCARNA5</td>
<td headers="log2FoldChange" class="gt_row gt_right">-1.158381</td>
<td headers="pvalue" class="gt_row gt_right">4.517669e-06</td>
<td headers="padj" class="gt_row gt_right">6.136802e-03</td>
<td headers="qvalue" class="gt_row gt_right">5.214202e-03</td></tr>
  </tbody>
  &#10;  
</table>
</div>

Top 10 DEG are diplayed.

## VST Counts

``` r
vst_cts[1:4, 1:5] %>% 
  gt()
```

<div id="niizyreoue" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#niizyreoue table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#niizyreoue thead, #niizyreoue tbody, #niizyreoue tfoot, #niizyreoue tr, #niizyreoue td, #niizyreoue th {
  border-style: none;
}
&#10;#niizyreoue p {
  margin: 0;
  padding: 0;
}
&#10;#niizyreoue .gt_table {
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
&#10;#niizyreoue .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#niizyreoue .gt_title {
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
&#10;#niizyreoue .gt_subtitle {
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
&#10;#niizyreoue .gt_heading {
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
&#10;#niizyreoue .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#niizyreoue .gt_col_headings {
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
&#10;#niizyreoue .gt_col_heading {
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
&#10;#niizyreoue .gt_column_spanner_outer {
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
&#10;#niizyreoue .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#niizyreoue .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#niizyreoue .gt_column_spanner {
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
&#10;#niizyreoue .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#niizyreoue .gt_group_heading {
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
&#10;#niizyreoue .gt_empty_group_heading {
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
&#10;#niizyreoue .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#niizyreoue .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#niizyreoue .gt_row {
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
&#10;#niizyreoue .gt_stub {
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
&#10;#niizyreoue .gt_stub_row_group {
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
&#10;#niizyreoue .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#niizyreoue .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#niizyreoue .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#niizyreoue .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#niizyreoue .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#niizyreoue .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#niizyreoue .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#niizyreoue .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#niizyreoue .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#niizyreoue .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#niizyreoue .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#niizyreoue .gt_footnotes {
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
&#10;#niizyreoue .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#niizyreoue .gt_sourcenotes {
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
&#10;#niizyreoue .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#niizyreoue .gt_left {
  text-align: left;
}
&#10;#niizyreoue .gt_center {
  text-align: center;
}
&#10;#niizyreoue .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#niizyreoue .gt_font_normal {
  font-weight: normal;
}
&#10;#niizyreoue .gt_font_bold {
  font-weight: bold;
}
&#10;#niizyreoue .gt_font_italic {
  font-style: italic;
}
&#10;#niizyreoue .gt_super {
  font-size: 65%;
}
&#10;#niizyreoue .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#niizyreoue .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#niizyreoue .gt_indent_1 {
  text-indent: 5px;
}
&#10;#niizyreoue .gt_indent_2 {
  text-indent: 10px;
}
&#10;#niizyreoue .gt_indent_3 {
  text-indent: 15px;
}
&#10;#niizyreoue .gt_indent_4 {
  text-indent: 20px;
}
&#10;#niizyreoue .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    &#10;    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="X">X</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="ENTREZID">ENTREZID</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="SRR17547999">SRR17547999</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="SRR17548000">SRR17548000</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="SRR17548001">SRR17548001</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="X" class="gt_row gt_right">1</td>
<td headers="ENTREZID" class="gt_row gt_right">57801</td>
<td headers="SRR17547999" class="gt_row gt_right">5.532028</td>
<td headers="SRR17548000" class="gt_row gt_right">6.685125</td>
<td headers="SRR17548001" class="gt_row gt_right">4.936967</td></tr>
    <tr><td headers="X" class="gt_row gt_right">2</td>
<td headers="ENTREZID" class="gt_row gt_right">26155</td>
<td headers="SRR17547999" class="gt_row gt_right">9.547882</td>
<td headers="SRR17548000" class="gt_row gt_right">9.666100</td>
<td headers="SRR17548001" class="gt_row gt_right">9.758876</td></tr>
    <tr><td headers="X" class="gt_row gt_right">3</td>
<td headers="ENTREZID" class="gt_row gt_right">339451</td>
<td headers="SRR17547999" class="gt_row gt_right">7.117754</td>
<td headers="SRR17548000" class="gt_row gt_right">6.967143</td>
<td headers="SRR17548001" class="gt_row gt_right">7.488966</td></tr>
    <tr><td headers="X" class="gt_row gt_right">4</td>
<td headers="ENTREZID" class="gt_row gt_right">9636</td>
<td headers="SRR17547999" class="gt_row gt_right">9.554171</td>
<td headers="SRR17548000" class="gt_row gt_right">10.175736</td>
<td headers="SRR17548001" class="gt_row gt_right">8.719877</td></tr>
  </tbody>
  &#10;  
</table>
</div>

Note: Both Top DEG and VST counts have EntrezID with which they would be joined.

# Plot

## Prep

Prepare the data for plotting.

- Join the VST and DEG based on the ENTREZ IDs
- Remove un-neccessary columns
- Convert the wide table to a long table to get all normalized counts to one column
- Join with metadata using sample column

``` r
expr_df <- 
  top_deg %>% 
  # join the DEG with the variance stabilized counts using Entrez ID column
  inner_join(vst_cts, by = "ENTREZID") %>% 
  # Remove un-neccessary columns
  dplyr::select(-c(ENTREZID, log2FoldChange, pvalue, 
                   padj, qvalue, baseMean, lfcSE, GENENAME)) %>% 
  # pivot longer to get all normalized counts to one column
  pivot_longer(!SYMBOL, names_to = "sample", values_to = "norm_counts") %>% 
  # Join with metadata using sample column
  inner_join(metadata, by = "sample")
```

## Rank genes

To show the genes based on the p-value instead of alphabetical order. Get the genes and relevel in the next step

``` r
# This will be used to relevel the gene list
gene_rank <- 
  top_deg %>% 
  head(10) %>% 
  pull(SYMBOL)
```

## Make Plot

``` r
gc_boxplot <- 
  expr_df %>% 
  mutate(SYMBOL = fct_relevel(SYMBOL, gene_rank)) %>% 
  ggplot(aes(x = SYMBOL, y = norm_counts, fill = condition)) +
  geom_point(size = 3.5, alpha = 0.4, shape = 21, 
             position = position_dodge(0.55)) +
  stat_summary(fun.data = "mean_cl_boot", alpha = 0.8,
               geom = "crossbar", width = 0.5, color = "black",
               position = position_dodge(0.55)) +
  scale_fill_manual(values = c("darkorchid4", "gold2")) +
  labs(title = "Expression Box Plot of Top DEG", 
       subtitle = "Variance stabilized transformed gene counts were plotted",
       x = "", y = "Counts") +
  theme_bw() +
  theme(legend.position = "bottom")
```

``` r
gc_boxplot
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="960" />

The plot shows the VST counts of each sample per each gene. This would give an idea on the expression values range in each group.

# Session Info

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
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ##  [1] here_1.0.1      gt_0.10.0       lubridate_1.9.3 forcats_1.0.0  
    ##  [5] stringr_1.5.1   dplyr_1.1.4     purrr_1.0.2     readr_2.1.4    
    ##  [9] tidyr_1.3.0     tibble_3.2.1    ggplot2_3.4.4   tidyverse_2.0.0
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] sass_0.4.7        utf8_1.2.4        generics_0.1.3    xml2_1.3.5       
    ##  [5] blogdown_1.19     stringi_1.8.1     hms_1.1.3         digest_0.6.33    
    ##  [9] magrittr_2.0.3    evaluate_0.23     grid_4.3.0        timechange_0.2.0 
    ## [13] bookdown_0.39     fastmap_1.1.1     rprojroot_2.0.4   jsonlite_1.8.7   
    ## [17] backports_1.4.1   nnet_7.3-19       Formula_1.2-5     gridExtra_2.3    
    ## [21] fansi_1.0.5       scales_1.2.1      jquerylib_0.1.4   cli_3.6.1        
    ## [25] rlang_1.1.2       Hmisc_5.1-1       munsell_0.5.0     base64enc_0.1-3  
    ## [29] withr_2.5.2       cachem_1.0.8      yaml_2.3.7        tools_4.3.0      
    ## [33] tzdb_0.4.0        checkmate_2.3.0   htmlTable_2.4.2   colorspace_2.1-0 
    ## [37] rpart_4.1.21      vctrs_0.6.4       R6_2.5.1          lifecycle_1.0.4  
    ## [41] htmlwidgets_1.6.2 foreign_0.8-85    cluster_2.1.4     pkgconfig_2.0.3  
    ## [45] pillar_1.9.0      bslib_0.5.1       gtable_0.3.4      data.table_1.14.8
    ## [49] glue_1.6.2        highr_0.10        xfun_0.43         tidyselect_1.2.0 
    ## [53] rstudioapi_0.15.0 knitr_1.45        farver_2.1.1      htmltools_0.5.7  
    ## [57] labeling_0.4.3    rmarkdown_2.25    compiler_4.3.0
