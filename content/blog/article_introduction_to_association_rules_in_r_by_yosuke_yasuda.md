---
title: "Reproducing: Introduction to Association Rules in R by Yosuke Yasuda"
date: 2018-01-12T18:30:12+03:00 
draft: false
description: ""
tags: ["r"]
categories: ["software", "r"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: mertnuhoglu.com
resource_files:
-
path: ~/projects/study/r/convert_yuml_to_rdb_style_data.md
---

Association rules is used as a common recommendation algorithm.
<!--more-->

``` r
.libPaths("/Users/mertnuhoglu/.exploratory/R/3.4")
library(dplyr, warn.conflicts = F)
```

This document reproduces the code explained in [Introduction to Association Rules in R by Yosuke Yasuda](https://blog.exploratory.io/introduction-to-association-rules-market-basket-analysis-in-r-7a0dd900a3e0)

csv files can be accessed from http://github.com/mertnuhoglu/study_data

```r
readr::read_csv("/Users/mertnuhoglu/projects/study_data/ds/article_introduction_to_association_rules_in_r_by_yosuke_yasuda_groceries.csv", , quote = "\"", skip = 0 , col_names = FALSE , na = c("","NA")) %>%
  dplyr::mutate(transaction_id = row_number()) %>%
  tidyr::gather(key, product, -transaction_id, na.rm = TRUE, convert = TRUE) %>%
  dplyr::arrange(transaction_id) %>%
  exploratory::do_apriori(product, transaction_id, min_support = 0.0001) %>%
  dplyr::filter(support > 0.0004) %>%
  dplyr::group_by(rhs) %>%
  dplyr::top_n(3, lift)
  ## # A tibble: 30 x 5
  ## # Groups:   rhs [15]
  ##                                lhs          rhs      support confidence      lift
  ##                              <chr>        <chr>        <dbl>      <dbl>     <dbl>
  ##  1 canned beer, liquor (appetizer)         soda 0.0004067107  0.5714286  5.526057
  ##  2  frozen potato products, yogurt   whole milk 0.0004067107  0.8000000  3.589416
  ##  3         frankfurter, liver loaf      sausage 0.0005083884  0.7142857  7.602814
  ##  4             liver loaf, sausage  frankfurter 0.0005083884  0.5000000  8.478448
  ##  5         flour, other vegetables   whole milk 0.0005083884  0.8333333  3.738975
  ##  6        canned beer, canned fish   rolls/buns 0.0004067107  0.8000000  6.319679
  ##  7          liquor, red/blush wine bottled beer 0.0016268429  0.9411765 19.528419
  ##  8    bottled beer, red/blush wine       liquor 0.0016268429  0.6666667 81.958333
  ##  9            red/blush wine, soda       liquor 0.0006100661  0.5454545 67.056818
  ## 10                    liquor, soda bottled beer 0.0010167768  0.7692308 15.960727
  ## # ... with 20 more rows
```


