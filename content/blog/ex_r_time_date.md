---
title: "Example: R Time and Date"
date: 2017-12-19T18:27:56+03:00 
draft: false
description: ""
tags: ["examples", "R", ]
categories: ["software", "r"]
type: post
url:
author: "Mert Nuhoglu"
output: rmarkdown::html_document
blog: mertnuhoglu.com
resource_files:
-
---

How to use and format time and date information in R.

<!--more-->

<!-- toc -->

## ISO 8601 Format

Reading time:

```{r}
as.POSIXlt(Sys.time(), "UTC", "%Y-%m-%dT%H:%M:%S")
  ## [1] "2018-12-11 16:31:25 UTC"
as.POSIXlt(Sys.time(), "Asia/Baghdad", "%Y-%m-%dT%H:%M:%S")
  ## [1] "2018-12-11 19:31:26 +03"
```

Writing time:

```{r}
tm = as.POSIXlt(Sys.time(), "Asia/Baghdad", "%Y-%m-%dT%H:%M:%S")
strftime(tm , "%Y-%m-%dT%H:%M:%S%z")
  ## [1] "2018-12-11T19:31:26+0300"
strftime(Sys.time(), "%Y-%m-%dT%H:%M:%S%z")
  ## [1] "2018-12-11T19:31:26+0300"
```


