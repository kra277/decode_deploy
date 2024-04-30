---
title: GC plots v2
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

This document shows how to generate Boxplots for top DEG using the Variance stabilized counts from RNA seq results. 

Required files for Gene counts Boxplots: 

-     Metadata/phenotype file --> metadata
-     VST Normalized counts --> vst_counts
-     DEG results --> deg_res (Note these are arranged based on Pvalue)



```r
# Load required packages
library(tidyverse) # Data wrangling
library(gt) # for displaying data tables in a pdf
library(here) # Create customized upset plot
```

## Load

Load DEG results, VST stabilized gene counts, and metadata


```r
# get the working dir path
wrk_path <- here("content/blog/2024-04-30-gc-plots-v2/")

top_deg <- read.csv(paste0(wrk_path, "top_deg.csv"))
metadata <- read.csv(paste0(wrk_path, "metadata.csv"))
vst_cts <- read.csv(paste0(wrk_path, "vst_counts.csv"))
```


Make the Expression DF for Top DEG


```r
expr_df <- 
  top_deg %>% 
  # join the DEG with the variance stabilized counts using Entrez ID column
  inner_join(vst_cts, by = "ENTREZID") %>% 
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


```r
# This will be used to relevel the gene list
gene_rank <- 
  top_deg %>% 
  head(10) %>% 
  pull(SYMBOL)
```

\newpage

## Make Plot


```r
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


\newpage


```r
gc_boxplot
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="960" />


\newpage


```r
sessionInfo()
```

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
```
