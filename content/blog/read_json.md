---
title: "R jsonlite: Read JSON Snippets"
date: 2018-02-03T10:25:38+03:00 
draft: false
description: ""
tags: ["r", "json"]
categories: ["software", "r", "json"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: mertnuhoglu.com
resource_files:
- data/plan_jobs.json
path: ~/projects/study/r/read_json.Rmd

---

I have some json files exported from a mongodb database. Now, I am going to read and process them using `R jsonlite` library.

<!--more-->

``` bash
jq . data/plan_jobs.json 
  ##> [
  ##>   {
  ##>     "_id": {
  ##>       "$oid": "59259c9b4eb11bdcb734897d"
  ##>     },
  ##>     "depot": "ADANA",
  ##>     "max_drops": 4,
  ##>     "max_vol_tol": 10,
  ##>     "shortStatus": "G",
  ##>     "status": "TAMAMLANDI",
  ##>     "submitDate": "2017-05-24 17:45:47",
  ##>     "submitId": {
  ##>       "$numberLong": "1495637147081"
  ##>     },
  ##>     "trailer_eff_volM3": 73,
  ##>     "truck_eff_volM3": 33,
  ##>     "user": "admin"
  ##>   },
  ##>   {
  ##>     "_id": {
  ##>       "$oid": "5926703e4eb11bdcb73489e4"
  ##>     },
  ##>     "depot": "CORLU",
  ##>     "max_drops": 4,
  ##>     "max_vol_tol": 10,
  ##>     "shortStatus": "G",
  ##>     "status": "TAMAMLANDI",
  ##>     "submitDate": "2017-05-25 08:48:45",
  ##>     "submitId": {
  ##>       "$numberLong": "1495691325885"
  ##>     },
  ##>     "trailer_eff_volM3": 55,
  ##>     "truck_eff_volM3": 33,
  ##>     "user": "plan_corlu"
  ##>   },...
``` 

```{r}
result = jsonlite::fromJSON("data/plan_jobs.json")
result
  ##>                          $oid     depot max_drops max_vol_tol shortStatus             status          submitDate   $numberLong trailer_eff_volM3
  ##> 1    59259c9b4eb11bdcb734897d     ADANA         4          10           G         TAMAMLANDI 2017-05-24 17:45:47 1495637147081                73
  ##> 2    5926703e4eb11bdcb73489e4     CORLU         4          10           G         TAMAMLANDI 2017-05-25 08:48:45 1495691325885                55
```

