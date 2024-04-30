---
title: Internal Data
author: Kesava Asam
date: '2024-04-30'
slug: []
categories: []
tags: []
---

``` r
library(tidyverse)
library(gt)
```

# Check Data

``` r
mtcars[1:6, 1:6] %>% 
  gt()
```

<div id="toyyhxmvmz" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#toyyhxmvmz table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#toyyhxmvmz thead, #toyyhxmvmz tbody, #toyyhxmvmz tfoot, #toyyhxmvmz tr, #toyyhxmvmz td, #toyyhxmvmz th {
  border-style: none;
}
&#10;#toyyhxmvmz p {
  margin: 0;
  padding: 0;
}
&#10;#toyyhxmvmz .gt_table {
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
&#10;#toyyhxmvmz .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#toyyhxmvmz .gt_title {
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
&#10;#toyyhxmvmz .gt_subtitle {
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
&#10;#toyyhxmvmz .gt_heading {
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
&#10;#toyyhxmvmz .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#toyyhxmvmz .gt_col_headings {
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
&#10;#toyyhxmvmz .gt_col_heading {
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
&#10;#toyyhxmvmz .gt_column_spanner_outer {
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
&#10;#toyyhxmvmz .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#toyyhxmvmz .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#toyyhxmvmz .gt_column_spanner {
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
&#10;#toyyhxmvmz .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#toyyhxmvmz .gt_group_heading {
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
&#10;#toyyhxmvmz .gt_empty_group_heading {
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
&#10;#toyyhxmvmz .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#toyyhxmvmz .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#toyyhxmvmz .gt_row {
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
&#10;#toyyhxmvmz .gt_stub {
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
&#10;#toyyhxmvmz .gt_stub_row_group {
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
&#10;#toyyhxmvmz .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#toyyhxmvmz .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#toyyhxmvmz .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#toyyhxmvmz .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#toyyhxmvmz .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#toyyhxmvmz .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#toyyhxmvmz .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#toyyhxmvmz .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#toyyhxmvmz .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#toyyhxmvmz .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#toyyhxmvmz .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#toyyhxmvmz .gt_footnotes {
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
&#10;#toyyhxmvmz .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#toyyhxmvmz .gt_sourcenotes {
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
&#10;#toyyhxmvmz .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#toyyhxmvmz .gt_left {
  text-align: left;
}
&#10;#toyyhxmvmz .gt_center {
  text-align: center;
}
&#10;#toyyhxmvmz .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#toyyhxmvmz .gt_font_normal {
  font-weight: normal;
}
&#10;#toyyhxmvmz .gt_font_bold {
  font-weight: bold;
}
&#10;#toyyhxmvmz .gt_font_italic {
  font-style: italic;
}
&#10;#toyyhxmvmz .gt_super {
  font-size: 65%;
}
&#10;#toyyhxmvmz .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#toyyhxmvmz .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#toyyhxmvmz .gt_indent_1 {
  text-indent: 5px;
}
&#10;#toyyhxmvmz .gt_indent_2 {
  text-indent: 10px;
}
&#10;#toyyhxmvmz .gt_indent_3 {
  text-indent: 15px;
}
&#10;#toyyhxmvmz .gt_indent_4 {
  text-indent: 20px;
}
&#10;#toyyhxmvmz .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    &#10;    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="mpg">mpg</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="cyl">cyl</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="disp">disp</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="hp">hp</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="drat">drat</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="wt">wt</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="mpg" class="gt_row gt_right">21.0</td>
<td headers="cyl" class="gt_row gt_right">6</td>
<td headers="disp" class="gt_row gt_right">160</td>
<td headers="hp" class="gt_row gt_right">110</td>
<td headers="drat" class="gt_row gt_right">3.90</td>
<td headers="wt" class="gt_row gt_right">2.620</td></tr>
    <tr><td headers="mpg" class="gt_row gt_right">21.0</td>
<td headers="cyl" class="gt_row gt_right">6</td>
<td headers="disp" class="gt_row gt_right">160</td>
<td headers="hp" class="gt_row gt_right">110</td>
<td headers="drat" class="gt_row gt_right">3.90</td>
<td headers="wt" class="gt_row gt_right">2.875</td></tr>
    <tr><td headers="mpg" class="gt_row gt_right">22.8</td>
<td headers="cyl" class="gt_row gt_right">4</td>
<td headers="disp" class="gt_row gt_right">108</td>
<td headers="hp" class="gt_row gt_right">93</td>
<td headers="drat" class="gt_row gt_right">3.85</td>
<td headers="wt" class="gt_row gt_right">2.320</td></tr>
    <tr><td headers="mpg" class="gt_row gt_right">21.4</td>
<td headers="cyl" class="gt_row gt_right">6</td>
<td headers="disp" class="gt_row gt_right">258</td>
<td headers="hp" class="gt_row gt_right">110</td>
<td headers="drat" class="gt_row gt_right">3.08</td>
<td headers="wt" class="gt_row gt_right">3.215</td></tr>
    <tr><td headers="mpg" class="gt_row gt_right">18.7</td>
<td headers="cyl" class="gt_row gt_right">8</td>
<td headers="disp" class="gt_row gt_right">360</td>
<td headers="hp" class="gt_row gt_right">175</td>
<td headers="drat" class="gt_row gt_right">3.15</td>
<td headers="wt" class="gt_row gt_right">3.440</td></tr>
    <tr><td headers="mpg" class="gt_row gt_right">18.1</td>
<td headers="cyl" class="gt_row gt_right">6</td>
<td headers="disp" class="gt_row gt_right">225</td>
<td headers="hp" class="gt_row gt_right">105</td>
<td headers="drat" class="gt_row gt_right">2.76</td>
<td headers="wt" class="gt_row gt_right">3.460</td></tr>
  </tbody>
  &#10;  
</table>
</div>

# Plot

``` r
mtcars %>% 
  ggplot(aes(hp, mpg)) +
  geom_point(alpha = 0.5, size = 3, color = "orchid2") +
  geom_smooth(method = 'lm', color = "orchid4", 
              se = F, linewidth = 2, alpha = 0.5) +
  theme_bw() +
  labs(title = "As horsepower increases, mileage decreases")
```

    ## `geom_smooth()` using formula = 'y ~ x'

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

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
    ##  [1] gt_0.10.0       lubridate_1.9.3 forcats_1.0.0   stringr_1.5.1  
    ##  [5] dplyr_1.1.4     purrr_1.0.2     readr_2.1.4     tidyr_1.3.0    
    ##  [9] tibble_3.2.1    ggplot2_3.4.4   tidyverse_2.0.0
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] sass_0.4.7        utf8_1.2.4        generics_0.1.3    xml2_1.3.5       
    ##  [5] lattice_0.22-5    blogdown_1.19     stringi_1.8.1     hms_1.1.3        
    ##  [9] digest_0.6.33     magrittr_2.0.3    evaluate_0.23     grid_4.3.0       
    ## [13] timechange_0.2.0  bookdown_0.39     fastmap_1.1.1     Matrix_1.6-3     
    ## [17] jsonlite_1.8.7    mgcv_1.9-0        fansi_1.0.5       scales_1.2.1     
    ## [21] jquerylib_0.1.4   cli_3.6.1         rlang_1.1.2       munsell_0.5.0    
    ## [25] splines_4.3.0     withr_2.5.2       cachem_1.0.8      yaml_2.3.7       
    ## [29] tools_4.3.0       tzdb_0.4.0        colorspace_2.1-0  vctrs_0.6.4      
    ## [33] R6_2.5.1          lifecycle_1.0.4   pkgconfig_2.0.3   pillar_1.9.0     
    ## [37] bslib_0.5.1       gtable_0.3.4      glue_1.6.2        highr_0.10       
    ## [41] xfun_0.43         tidyselect_1.2.0  rstudioapi_0.15.0 knitr_1.45       
    ## [45] farver_2.1.1      nlme_3.1-163      htmltools_0.5.7   labeling_0.4.3   
    ## [49] rmarkdown_2.25    compiler_4.3.0
