---
title: "UML Diagrams from Text: yuml to uml"
date: 2018-01-20T18:30:12+03:00 
draft: false
description: ""
tags: ["yuml", "uml"]
categories: ["software"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: mertnuhoglu.com
resource_files:
- data/datamodel_enum01.md
- data/datamodel_order01.md
path: ~/projects/study/problem/yuml_to_uml.Rmd
ref_files:
- ~/Dropbox (BTG)/TEUIS PROJECT 05-ANALYSIS/working_library/data_models/conventions_for_data_modelling.md
state: wip

---

I like UML diagrams but I don't like UML diagram editors. Because using them increases my work load. I don't want to maintain one more thing while developing software. 

<!--more-->

<!-- toc -->

`yuml` allows me to generate UML diagrams from text. I start data modelling in `yuml` text files. From these text files, I generate automatically a few useful assets such as:

1. UML diagrams
2. DDL (data definition language) statements for database 
3. RDB (requirements database) data

[RDB](/what_is_rdb/) is an abbreviation that I use for requirements database. I try to store all software specifications as structured data in a relational database format.

The notation used in yuml files is from `yuml.me` website. It is a way to describe UML diagrams using text. By default, all cardinalities between two objects is n-n if not specifically overwritten. 

`^-` symbol denotes a super-subtype relationship. 

`-` symbol denotes an association relationship. 

`=` symbol denotes double associations.

Refer to the following web page to learn the notation: http://yuml.me/diagram/scruffy/class/samples

yuml.me is a free web site that generates UML diagrams from yuml formatted text. There is an unofficial command line client for yuml web app: https://github.com/wandernauta/yuml

I use that command line client to generate UML diagrams automatically. Currently, the source code for my code is here: https://github.com/mertnuhoglu/yuml_to_rdb_schema/tree/master/inst/bash/main_yuml_to_uml.sh

I will move this script to its own github repo. Currently, it is inside a related R package called `yumltordbschema`.

By convention, I put all yuml formatted data models into files whose name start with `data_model` or `datamodel`. `main_yuml_to_uml.sh` script searches all those files, does some text cleaning and calls the actual `yuml` command line tool. 

For example, I described data models of two subsystems called "order" and "enum". I wrote their yuml code inside two files: `datamodel_order01.md` and `datamodel_enum01.md`

``` bash
cat data/datamodel_enum01.md
## 
##     [enum_value| enum_value_id PK; enum_var_id @NN; enum_value_name TEXT; ]
##     [enum_var| enum_var_id PK; enum_var_name TEXT; ]
##     [enum_var] 1-n [enum_value]
``` 

``` bash
cat data/datamodel_order01.md
## 
##     [product| product_id PK; product_name TEXT; ]
##     [purchase_order| purchase_order_id PK; ]
##     [order_line| order_line_id PK; product_id @NN; amount INT; ]
##     [purchase_order] 1-n [order_line]
##     [product] 1-n [order_line]
``` 

To generate UML diagrams run the script:

``` bash
sh /Users/mertnuhoglu/projects/yumltordbschema/inst/bash/main_yuml_to_uml.sh data/
## 
## running /Users/mertnuhoglu/projects/yumltordbschema/inst/bash/main_yuml_to_uml.sh
## DATA_MODEL_DIR = data/
## SCRIPT_DIR = /Users/mertnuhoglu/projects/yumltordbschema/inst/bash
## running /Users/mertnuhoglu/projects/yumltordbschema/inst/bash/yuml_to_uml.sh
## DATA_MODEL_DIR = data/
## SCRIPT_DIR = /Users/mertnuhoglu/projects/yumltordbschema/inst/bash
## ./img/datamodel_enum01.png
## /Users/mertnuhoglu/projects/yumltordbschema/inst/bash/yuml_to_uml.sh: /usr/local/bin/yuml: /usr/local/opt/python/bin/python2.7: bad interpreter: No such file or directory
## /Users/mertnuhoglu/projects/yumltordbschema/inst/bash/yuml_to_uml.sh: /usr/local/bin/yuml: /usr/local/opt/python/bin/python2.7: bad interpreter: No such file or directory
## yuml data model input: ./datamodel_enum01.md
## yuml data model input: ./datamodel_order01.md
``` 

The script produces UML diagrams under `img/` directory:

``` bash
ls data/img
##
## datamodel_enum01.png
## datamodel_enum01_yuml.txt
## datamodel_order01.png
## datamodel_order01_yuml.txt
## simple_datamodel_enum01.png
## simple_datamodel_enum01_yuml.txt
## simple_datamodel_order01.png
## simple_datamodel_order01_yuml.txt
``` 

These are the generated UML diagrams:

![datamodel_order01.png](/images/datamodel_order01.png)
![datamodel_enum01.png](/images/datamodel_enum01.png)

@tbd:

## Simplified UML Diagrams

## Cleaning non-yuml Text in the Documentation Files

## Preprocessing and Standardizing yuml Code


