---
title: "Data Generation and Recommendation Engine Using Association Rules (Market Basket) in R"
date: 2017-12-25T18:27:56+03:00 
draft: false
description: ""
tags:
categories: ["datascience", "recommendation", "data"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: datascience.mertnuhoglu.com
resource_files:
- version.csv
- schema.sql
- product_category.csv
- init.sql
- data.sql
- brand.csv
path: ~/projects/study/ds/recommendation_engine_using_association_rules_in_r/data_generation_for_recommendation_engine_using_association_rules_in_r.md

---

<!--more-->

```r
library(dplyr, warn.conflicts = F)
library(dbplyr, warn.conflicts = F)
``` 

```r
df1 = readr::read_csv("brand.csv")
df2 = readr::read_csv("product_category.csv")
df3 = readr::read_csv("version.csv")
``` 

```r
df = merge(df1, df2, all = T) %>%
  merge(df3, all = T) %>%
  mutate( product_title = sprintf("%s %s %s", brand, product_category, version) %>% tolower ) 
writeLines(df$product_title, "product_title.txt")
``` 

``` bash
cat schema.sql
  ## DROP TABLE IF EXISTS product CASCADE;
  ## CREATE TABLE product (  
  ##   product_id SERIAL PRIMARY KEY
  ##   , title TEXT NOT NULL
  ##   -- df product_title: word=product_title.txt
  ##   -- df: text=product_title length=1
  ## );
  ## DROP TABLE IF EXISTS purchase_order CASCADE;
  ## CREATE TABLE purchase_order (  --df: mult=0.1
  ##   purchase_order_id SERIAL PRIMARY KEY
  ## );
  ## DROP TABLE IF EXISTS order_line CASCADE;
  ## CREATE TABLE order_line (  --df: mult=0.5
  ##   product_id INTEGER NOT NULL REFERENCES product (product_id)
  ##   , purchase_order_id INTEGER NOT NULL REFERENCES purchase_order (purchase_order_id)
  ##   , amount INTEGER NOT NULL --df: offset=1 size=5
  ##   , PRIMARY KEY (product_id, purchase_order_id)
  ## );
``` 

``` bash
datafiller --seed 1 --size=300 schema.sql > data.sql
``` 

``` bash
cat data.sql | sed -n "/COPY/,+2 p"
  ## COPY product (product_id,title) FROM STDIN (ENCODING 'utf-8');
  ## 1    kia jant fk 4
  ## 2    dacia kaporta 392
  ## COPY purchase_order (purchase_order_id) FROM STDIN (ENCODING 'utf-8');
  ## 1
  ## 2
  ## COPY order_line (product_id,purchase_order_id,amount) FROM STDIN (ENCODING 'utf-8');
  ## 183  6   5
  ## 186  14  2
``` 

Now, we are going to start postgresql database server and import the generated data.

``` 
docker start postgreststarterkit_db_1
``` 

``` bash
psql -d app -h localhost -p 5432 -U superuser -f init.sql
  ## psql:init.sql:1: NOTICE:  drop cascades to 3 other objects
  ## DETAIL:  drop cascades to table recommendation_engine.product
  ## drop cascades to table recommendation_engine.purchase_order
  ## drop cascades to table recommendation_engine.order_line
  ## DROP SCHEMA
  ## CREATE SCHEMA
  ## SET
  ## psql:schema.sql:1: NOTICE:  table "product" does not exist, skipping
  ## DROP TABLE
  ## CREATE TABLE
  ## psql:schema.sql:8: NOTICE:  table "purchase_order" does not exist, skipping
  ## DROP TABLE
  ## CREATE TABLE
  ## psql:schema.sql:12: NOTICE:  table "order_line" does not exist, skipping
  ## DROP TABLE
  ## CREATE TABLE
  ## # filling table product (300)
  ## COPY 300
  ## # filling table purchase_order (30)
  ## COPY 30
  ## # filling table order_line (150)
  ## COPY 150
  ## ALTER SEQUENCE
  ## ALTER SEQUENCE
  ## ANALYZE
  ## ANALYZE
  ## ANALYZE
``` 

Read data from PostgreSQL into R dataframe:

``` r
con2 <- DBI::dbConnect(RPostgreSQL::PostgreSQL()
  , user = Sys.getenv("SUPER_USER")
  , password = Sys.getenv("SUPER_USER_PASSWORD")
  , dbname = "app"
  , host = "localhost"
  , port = "5432"
  , options = "-c search_path=recommendation_engine"
)
prd = dplyr::tbl(con2, "product")
por = dplyr::tbl(con2, "purchase_order")
orl = dplyr::tbl(con2, "order_line")
```

Join the tables:

``` r
orl2 = orl %>%
  dplyr::inner_join(prd, by = "product_id") %>%
  dplyr::select(purchase_order_id, title, amount) %>%
  dplyr::arrange(purchase_order_id, title)
orl2
  ## # Source:     lazy query [?? x 3]
  ## # Database:   postgres 10.0.0 [superuser@localhost:5432/app]
  ## # Ordered by: purchase_order_id, title
  ##    purchase_order_id                 title amount
  ##                <int>                 <chr>  <int>
  ##  1                 1       citroen egsoz a      2
  ##  2                 1        kia filitre 20      4
  ##  3                 1     opel aynalar fk 4      5
  ##  4                 1        opel egsoz 160      1
  ##  5                 2    bmw silecekler 120      3
  ##  6                 2     bmw silecekler 39      1
  ##  7                 2 citroen yağlama 1.9 j      4
  ##  8                 2    honda şanzıman 120      2
  ##  9                 2          opel far 160      5
  ## 10                 2   twingo yakıt 3.2 jk      5
  ## # ... with more rows
```

Target shape requires to concatenate all products in one purchase order transaction together:

``` r
orl3 = orl2 %>%
  dplyr::collect() %>%
  dplyr::group_by(purchase_order_id) %>%
  dplyr::mutate(key = sprintf("x%s", row_number())) %>%
  dplyr::ungroup() %>%
  dplyr::mutate( row = row_number() ) 
orl3
  ## # A tibble: 150 x 5
  ##    purchase_order_id                 title amount   key   row
  ##                <int>                 <chr>  <int> <chr> <int>
  ##  1                 1       citroen egsoz a      2    x1     1
  ##  2                 1        kia filitre 20      4    x2     2
  ##  3                 1     opel aynalar fk 4      5    x3     3
  ##  4                 1        opel egsoz 160      1    x4     4
  ##  5                 2    bmw silecekler 120      3    x1     5
  ##  6                 2     bmw silecekler 39      1    x2     6
  ##  7                 2 citroen yağlama 1.9 j      4    x3     7
  ##  8                 2    honda şanzıman 120      2    x4     8
  ##  9                 2          opel far 160      5    x5     9
  ## 10                 2   twingo yakıt 3.2 jk      5    x6    10
  ## # ... with 140 more rows

orl4 = orl3 %>%
  dplyr::select(purchase_order_id, key, title) %>%
  tidyr::spread(key = key, value = title) 
orl4 
  ## # A tibble: 30 x 11
  ##    purchase_order_id                      x1   x10                     x2
  ##  *             <int>                   <chr> <chr>                  <chr>
  ##  1                 1         citroen egsoz a  <NA>         kia filitre 20
  ##  2                 2      bmw silecekler 120  <NA>      bmw silecekler 39
  ##  3                 3 honda göstergeler 1.5 j  <NA> kia yönlendirme 3.2 jk
  ##  4                 4 citroen göstergeler 392  <NA>      ford elektrik cax
  ##  5                 5         honda fren fk 4  <NA>  mercedes balans 1.5 j
  ##  6                 6         citroen far 392  <NA>       dacia aynalar 12
  ##  7                 7         dacia rot 1.9 j  <NA>                   <NA>
  ##  8                 8         citroen diğer a  <NA>       dacia aynalar 12
  ##  9                 9      citroen mekanik 20  <NA>   ford yönlendirme 160
  ## 10                10    ford yönlendirme 160  <NA>        opel aynalar 39
  ## # ... with 20 more rows, and 7 more variables: x3 <chr>, x4 <chr>,
  ## #   x5 <chr>, x6 <chr>, x7 <chr>, x8 <chr>, x9 <chr>
```

```r
.libPaths("/Users/mertnuhoglu/.exploratory/R/3.4")
library(exploratory)

# Data Analysis Steps
res = orl4 %>%
  dplyr::select(-purchase_order_id) %>%
  dplyr::mutate(transaction_id = row_number()) %>%
  tidyr::gather(key, product, -transaction_id, na.rm = TRUE, convert = TRUE) %>%
  dplyr::arrange(transaction_id) %>%
  exploratory::do_apriori(product, transaction_id, min_support = 0.0001) %>%
  dplyr::filter(support > 0.0004) %>%
  dplyr::group_by(rhs) %>%
  dplyr::top_n(3, lift)
res

  ## # A tibble: 9,966 x 5
  ## # Groups:   rhs [115]
  ##                      lhs                   rhs    support confidence  lift
  ##                    <chr>                 <chr>      <dbl>      <dbl> <dbl>
  ##  1       honda fren fk 4 mercedes balans 1.5 j 0.03333333        1.0    15
  ##  2 mercedes balans 1.5 j       honda fren fk 4 0.03333333        0.5    15
  ##  3     bmw mekanik 1.7 j      citroen fren 105 0.03333333        1.0    30
  ##  4      citroen fren 105     bmw mekanik 1.7 j 0.03333333        1.0    30
  ##  5     bmw mekanik 1.7 j       renault rot 1.3 0.03333333        1.0    30
  ##  6       renault rot 1.3     bmw mekanik 1.7 j 0.03333333        1.0    30
  ##  7          dacia aks 20        kia balans cax 0.03333333        1.0    30
  ##  8        kia balans cax          dacia aks 20 0.03333333        1.0    30
  ##  9          dacia aks 20         kia far 1.5 j 0.03333333        1.0    30
  ## 10         kia far 1.5 j          dacia aks 20 0.03333333        1.0    30
  ## # ... with 9,956 more rows
```

```r
res = orl4 %>%
  dplyr::select(-purchase_order_id) %>%
  dplyr::mutate(transaction_id = row_number()) %>%
  tidyr::gather(key, product, -transaction_id, na.rm = TRUE, convert = TRUE) %>%
  dplyr::arrange(transaction_id) %>%
  exploratory::do_apriori(product, transaction_id, min_support = 0.0001) %>%
  dplyr::filter(support > 0.0004) %>%
  dplyr::group_by(rhs) %>%
  dplyr::top_n(3, lift)
res
  ## # A tibble: 9,966 x 5
  ## # Groups:   rhs [115]
  ##                      lhs                   rhs    support confidence  lift
  ##                    <chr>                 <chr>      <dbl>      <dbl> <dbl>
  ##  1       honda fren fk 4 mercedes balans 1.5 j 0.03333333        1.0    15
  ##  2 mercedes balans 1.5 j       honda fren fk 4 0.03333333        0.5    15
  ##  3     bmw mekanik 1.7 j      citroen fren 105 0.03333333        1.0    30
  ##  4      citroen fren 105     bmw mekanik 1.7 j 0.03333333        1.0    30
  ##  5     bmw mekanik 1.7 j       renault rot 1.3 0.03333333        1.0    30
  ##  6       renault rot 1.3     bmw mekanik 1.7 j 0.03333333        1.0    30
  ##  7          dacia aks 20        kia balans cax 0.03333333        1.0    30
  ##  8        kia balans cax          dacia aks 20 0.03333333        1.0    30
  ##  9          dacia aks 20         kia far 1.5 j 0.03333333        1.0    30
  ## 10         kia far 1.5 j          dacia aks 20 0.03333333        1.0    30
  ## # ... with 9,956 more rows
```

