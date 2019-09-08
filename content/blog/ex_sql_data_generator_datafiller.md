---
title: "Example: datafiller SQL Data Generator"
date: 2017-12-15T18:27:56+03:00 
draft: false
description: ""
tags: ["r", "debug"]
categories: ["software", "r"]
type: post
url:
author: "Mert Nuhoglu"
output: rmarkdown::html_document
blog: mertnuhoglu.com
resource_files:
- data/library.sql
- data/library_test_data.sql
- data/library_test_data02.sql
- data/library_test_data03.sql
- data/library_test_data04.sql
- data/library02.sql
- data/library03.sql
- data/library04.sql
path: ~/projects/study/sql/ex_sql_data_generator_datafiller.md

---

datafiller is a tool to generate SQL from DDL files.

<!--more-->

<!-- toc -->

## Getting Started 

``` bash
cat data/library.sql
  ##> CREATE TABLE Book(  --df: mult=3.0
  ##>   bid SERIAL PRIMARY KEY,
  ##>   title TEXT NOT NULL, 
  ##>   isbn ISBN13 NOT NULL 
  ##> );
  ##> CREATE TABLE Reader( 
  ##>   rid SERIAL PRIMARY KEY,
  ##>   firstname TEXT NOT NULL, 
  ##>   lastname TEXT NOT NULL, 
  ##>   born DATE NOT NULL, 
  ##>   gender BOOLEAN NOT NULL, 
  ##>   phone TEXT 
  ##> );
  ##> CREATE TABLE Borrow( --df: mult=1.5
  ##>   borrowed TIMESTAMP NOT NULL, 
  ##>   rid INTEGER NOT NULL REFERENCES Reader,
  ##>   bid INTEGER NOT NULL REFERENCES Book, 
  ##>   PRIMARY KEY(bid) -- a book is borrowed once at a time!
  ##> );
``` 

Note meta attributes:

``` sql
CREATE TABLE Book(  --df: mult=2.0
...
``` 

``` bash
datafiller --size=2 data/library.sql > data/library_test_data.sql
``` 

``` bash
head data/library_test_data.sql
  ##> 
  ##> -- data generated by /usr/local/bin/datafiller version 2.0.0 (r792 on 2014-03-23) for postgresql
  ##> 
  ##> -- fill table book (6)
  ##> \echo # filling table book (6)
  ##> COPY book (bid,title,isbn) FROM STDIN (ENCODING 'utf-8');
  ##> 1	title_1_1_1_	9784413254182
  ##> 2	title_4_4_4_4_4	9782571851779
  ##> 3	title_3_3_3	9787601387877
  ##> 4	title_2_2_2_2_2_	9788305707527
``` 

## Meta Attributes

``` bash
cat data/library02.sql
  ##> CREATE TABLE Book(  --df: mult=2.0
  ##>   bid SERIAL PRIMARY KEY,
  ##>   title TEXT NOT NULL, 
  ##>   -- df English: word=/Users/mertnuhoglu/projects/test_data/google-10000-english-no-swears.txt
  ##>   -- df: text=English length=4 lenvar=3
  ##>   isbn ISBN13 NOT NULL 
  ##> );
  ##> CREATE TABLE Reader( 
  ##>   rid SERIAL PRIMARY KEY,
  ##>   firstname TEXT NOT NULL, 
  ##>   -- df first: word=/Users/mertnuhoglu/projects/test_data/first_names.txt
  ##>   -- df: text=first length=1 lenvar=0
  ##>   lastname TEXT NOT NULL, 
  ##>   -- df last: word=/Users/mertnuhoglu/projects/test_data/last_names.txt
  ##>   -- df: text=last length=1 lenvar=0
  ##>   born DATE NOT NULL, -- df: start=1950-01-01 end=2005-01-01
  ##>   gender BOOLEAN NOT NULL, 
  ##>   phone TEXT 
  ##>   -- df: chars='0-9' length=10 lenvar=0
  ##>   -- df: null=0.01 size=1000000
  ##> );
  ##> CREATE TABLE Borrow( --df: mult=1.5
  ##>   borrowed TIMESTAMP NOT NULL, 
  ##>   rid INTEGER NOT NULL REFERENCES Reader,
  ##>   bid INTEGER NOT NULL REFERENCES Book, 
  ##>   PRIMARY KEY(bid) -- a book is borrowed once at a time!
  ##> );
  ##> 
  ##> 
``` 

Note the following attributes:

``` sql
  title TEXT NOT NULL, 
  -- df English: word=/Users/mertnuhoglu/projects/test_data/google-10000-english-no-swears.txt
  -- df: text=English length=4 lenvar=3
  ...
  born DATE NOT NULL, -- df: start=1950-01-01 end=2005-01-01
  ...
  phone TEXT 
  -- df: chars='0-9' length=10 lenvar=0
  -- df: null=0.01 size=1000000
  ...
``` 

``` bash
datafiller --size=3 data/library02.sql > data/library_test_data02.sql
``` 

``` bash
cat data/library_test_data02.sql
  ##> 
  ##> -- data generated by /usr/local/bin/datafiller version 2.0.0 (r792 on 2014-03-23) for postgresql
  ##> 
  ##> -- fill table book (6)
  ##> \echo # filling table book (6)
  ##> COPY book (bid,title,isbn) FROM STDIN (ENCODING 'utf-8');
  ##> 1	polished tu marble funded intro pennsylvania	9788014981218
  ##> 2	bedford vg	9782723776547
  ##> 3	trained commentary mozilla	9782723776547
  ##> 4	gourmet mathematics	9782913571662
  ##> 5	favors	9783956794780
  ##> 6	knitting purpose creatures skins	9782723776547
  ##> \.
  ##> 
  ##> -- fill table reader (3)
  ##> \echo # filling table reader (3)
  ##> COPY reader (rid,firstname,lastname,born,gender,phone) FROM STDIN (ENCODING 'utf-8');
  ##> 1	Eldon	Feela	1970-01-29	FALSE	6001655890
  ##> 2	Kerrie	Calk	1979-03-22	FALSE	6986568412
  ##> 3	Fiona	Echavarria	1960-08-05	FALSE	3753286868
  ##> \.
  ##> 
  ##> -- fill table borrow (4)
  ##> \echo # filling table borrow (4)
  ##> COPY borrow (borrowed,rid,bid) FROM STDIN (ENCODING 'utf-8');
  ##> 2017-12-21 16:29:57	2	1
  ##> 2017-12-21 16:30:57	3	2
  ##> 2017-12-21 16:30:57	1	3
  ##> 2017-12-21 16:28:57	2	4
  ##> \.
  ##> 
  ##> -- restart sequences
  ##> ALTER SEQUENCE book_bid_seq RESTART WITH 7;
  ##> ALTER SEQUENCE reader_rid_seq RESTART WITH 4;
  ##> 
  ##> -- analyze modified tables
  ##> ANALYZE book;
  ##> ANALYZE reader;
  ##> ANALYZE borrow;
``` 

``` bash
cat data/library03.sql
  ##> -- df English: word=/Users/mertnuhoglu/projects/test_data/google-10000-english-no-swears.txt
  ##> -- df first: word=/Users/mertnuhoglu/projects/test_data/first_names.txt
  ##> -- df last: word=/Users/mertnuhoglu/projects/test_data/last_names.txt
  ##> CREATE TABLE Book(  --df: mult=2.0
  ##>   bid SERIAL PRIMARY KEY,
  ##>   title TEXT NOT NULL, 
  ##>   -- df: text=English length=4 lenvar=3
  ##>   isbn INTEGER NOT NULL 
  ##> );
  ##> CREATE TABLE Reader( 
  ##>   rid SERIAL PRIMARY KEY,
  ##>   firstname TEXT NOT NULL, 
  ##>   -- df: text=first length=1 lenvar=0
  ##>   lastname TEXT NOT NULL, 
  ##>   -- df: text=last length=1 lenvar=0
  ##>   born DATE NOT NULL, -- df: start=1950-01-01 end=2005-01-01
  ##>   gender BOOLEAN NOT NULL, 
  ##>   -- df: rate=0.25
  ##>   phone TEXT 
  ##>   -- df: chars='0-9' length=10 lenvar=0
  ##>   -- df: null=0.01 size=1000000
  ##> );
  ##> CREATE TABLE Borrow( --df: mult=1.5
  ##>   borrowed TIMESTAMP NOT NULL, 
  ##>   -- df: size=72000 prec=60
  ##>   rid INTEGER NOT NULL REFERENCES Reader,
  ##>   bid INTEGER NOT NULL REFERENCES Book, 
  ##>   note TEXT,
  ##>   -- df: sub=power prefix=note size=1000 rate=0.03
  ##>   PRIMARY KEY(bid) -- a book is borrowed once at a time!
  ##> );
  ##> 
  ##> 
  ##> 
``` 

Note the following attributes:

``` sql
  gender BOOLEAN NOT NULL, 
  -- df: rate=0.25
  ...
  borrowed TIMESTAMP NOT NULL, 
  -- df: size=72000 prec=60
  ...
  note TEXT,
  -- df: sub=power prefix=note size=1000 rate=0.03
``` 

``` bash
datafiller --size=3 data/library03.sql > data/library_test_data03.sql
``` 

``` bash
cat data/library_test_data03.sql
  ##> 
  ##> -- data generated by /usr/local/bin/datafiller version 2.0.0 (r792 on 2014-03-23) for postgresql
  ##> 
  ##> -- fill table book (6)
  ##> \echo # filling table book (6)
  ##> COPY book (bid,title,isbn) FROM STDIN (ENCODING 'utf-8');
  ##> 1	palace funky	3
  ##> 2	moves indeed download attendance towers answer britney	4
  ##> 3	filled kent constant	6
  ##> 4	engineer nearby me salmon shell bike brought	5
  ##> 5	role	5
  ##> 6	bumper charm gd	3
  ##> \.
  ##> 
  ##> -- fill table reader (3)
  ##> \echo # filling table reader (3)
  ##> COPY reader (rid,firstname,lastname,born,gender,phone) FROM STDIN (ENCODING 'utf-8');
  ##> 1	Todd	Sesso	1988-01-18	FALSE	1931243076
  ##> 2	Dean	Ritmiller	1979-06-07	TRUE	1272396169
  ##> 3	Berry	Salos	1968-04-26	TRUE	9848238480
  ##> \.
  ##> 
  ##> -- fill table borrow (4)
  ##> \echo # filling table borrow (4)
  ##> COPY borrow (borrowed,rid,bid,note) FROM STDIN (ENCODING 'utf-8');
  ##> 2017-12-13 07:26:12	1	1	note_493_493
  ##> 2017-11-08 01:23:12	1	2	note_445_
  ##> 2017-11-28 22:35:12	1	3	note_223_
  ##> 2017-11-12 07:42:12	1	4	note_762_762
  ##> \.
  ##> 
  ##> -- restart sequences
  ##> ALTER SEQUENCE book_bid_seq RESTART WITH 7;
  ##> ALTER SEQUENCE reader_rid_seq RESTART WITH 4;
  ##> 
  ##> -- analyze modified tables
  ##> ANALYZE book;
  ##> ANALYZE reader;
  ##> ANALYZE borrow;
``` 

``` bash
cat data/library04.sql
  ##> -- df English: word=/Users/mertnuhoglu/projects/test_data/google-10000-english-no-swears.txt
  ##> -- df first: word=/Users/mertnuhoglu/projects/test_data/first_names.txt
  ##> -- df last: word=/Users/mertnuhoglu/projects/test_data/last_names.txt
  ##> CREATE TABLE Book(  --df: mult=2.0
  ##>   bid SERIAL PRIMARY KEY,
  ##>   title TEXT NOT NULL, 
  ##>   -- df: text=English length=4 lenvar=3
  ##>   isbn INTEGER NOT NULL 
  ##> );
  ##> CREATE TABLE Reader( 
  ##>   rid SERIAL PRIMARY KEY,
  ##>   firstname TEXT NOT NULL, 
  ##>   -- df: text=first length=1 lenvar=0
  ##>   lastname TEXT NOT NULL, 
  ##>   -- df: text=last length=1 lenvar=0
  ##>   born DATE NOT NULL, -- df: start=1950-01-01 end=2005-01-01
  ##>   gender BOOLEAN NOT NULL, 
  ##>   -- df: rate=0.25
  ##>   phone TEXT,
  ##>   -- df: chars='0-9' length=10 lenvar=0
  ##>   -- df: null=0.01 size=1000000
  ##>   email TEXT NOT NULL CHECK(email LIKE '%@%'),
  ##>   -- df: pattern='[a-z]{3,8}\.[a-z]{3,8}@(gmail|yahoo)\.com'
  ##>   -- define two macros
  ##>   -- df librarian: inet='10.1.0.0/16'
  ##>   -- df reader: inet='10.2.0.0/16'
  ##>   ip TEXT NOT NULL
  ##>   -- df: alt=reader:8,librarian:2
  ##>   -- This would do as well: --df: alt=reader:4,librarian
  ##> );
  ##> CREATE TABLE Borrow( --df: mult=1.5
  ##>   borrowed TIMESTAMP NOT NULL, 
  ##>   -- df: size=72000 prec=60
  ##>   rid INTEGER NOT NULL REFERENCES Reader,
  ##>   bid INTEGER NOT NULL REFERENCES Book, 
  ##>   note TEXT,
  ##>   -- df: sub=power prefix=note size=1000 rate=0.03
  ##>   PRIMARY KEY(bid) -- a book is borrowed once at a time!
  ##> );
  ##> 
``` 

Note the following attributes:

``` sql
  email TEXT NOT NULL CHECK(email LIKE '%@%'),
  -- df: pattern='[a-z]{3,8}\.[a-z]{3,8}@(gmail|yahoo)\.com'
  ...
``` 

``` bash
datafiller --seed 1 --size=3 data/library04.sql > data/library_test_data04.sql
``` 

``` bash
cat data/library_test_data04.sql
  ##> 
  ##> -- data generated by /usr/local/bin/datafiller version 2.0.0 (r792 on 2014-03-23) for postgresql
  ##> 
  ##> -- fill table book (6)
  ##> \echo # filling table book (6)
  ##> COPY book (bid,title,isbn) FROM STDIN (ENCODING 'utf-8');
  ##> 1	marvel meanwhile	1
  ##> 2	pens stationery expansys hilton learning transition casting	5
  ##> 3	issued ceiling ages olive lincoln moderate	5
  ##> 4	societies construction markets unable portland	1
  ##> 5	particles utah mask assault apparel submission panel	6
  ##> 6	ohio	5
  ##> \.
  ##> 
  ##> -- fill table reader (3)
  ##> \echo # filling table reader (3)
  ##> COPY reader (rid,firstname,lastname,born,gender,phone,email,ip) FROM STDIN (ENCODING 'utf-8');
  ##> 1	Kandra	Lewandoski	1993-08-08	FALSE	4459330731	oui.yfi@yahoo.com	10.2.236.144
  ##> 2	Marylee	Verone	2000-09-24	TRUE	7610496804	qhx.jytx@yahoo.com	10.2.220.251
  ##> 3	Raylene	Latshaw	1993-06-01	TRUE	9780596236	wnoxxo.flhwmuzf@yahoo.com	10.2.86.119
  ##> \.
  ##> 
  ##> -- fill table borrow (4)
  ##> \echo # filling table borrow (4)
  ##> COPY borrow (borrowed,rid,bid,note) FROM STDIN (ENCODING 'utf-8');
  ##> 2017-11-09 06:52:08	3	1	note_21_
  ##> 2017-11-10 22:16:08	1	2	note_543_543
  ##> 2017-12-15 03:12:08	1	3	note_180_180_18
  ##> 2017-11-07 13:46:08	1	4	note_110_
  ##> \.
  ##> 
  ##> -- restart sequences
  ##> ALTER SEQUENCE book_bid_seq RESTART WITH 7;
  ##> ALTER SEQUENCE reader_rid_seq RESTART WITH 4;
  ##> 
  ##> -- analyze modified tables
  ##> ANALYZE book;
  ##> ANALYZE reader;
  ##> ANALYZE borrow;
``` 
