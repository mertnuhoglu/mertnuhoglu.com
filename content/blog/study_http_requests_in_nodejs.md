---
title: "Studying NodeJs HTTP Requests"
date: 2018-03-19T11:19:06+03:00 
draft: false
description: ""
tags: ["js"]
categories: ["software", "js"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: mertnuhoglu.com
resource_files:
- ex/study_http_requests_in_nodejs/package.json
- ex/study_http_requests_in_nodejs/src/index.js
- ex/study_http_requests_in_nodejs/src/index02.js
path: ~/projects/study/js/study_http_requests_in_nodejs.Rmd

---

How to make HTTP requests in nodejs?

<!--more-->

<!-- toc -->


### v01: Basics 

Source code in https://github.com/mertnuhoglu/study/tree/master/js/ex/study_http_requests_in_nodejs/

HTML page:

``` bash
mkdir -p ex/study_http_requests_in_nodejs && $(cd $_ && npm init -y && pnpm i parcel-bundler request)
mkdir -p ex/study_http_requests_in_nodejs/src && touch ex/study_http_requests_in_nodejs/src/index.js
``` 

Edit `ex/study_http_requests_in_nodejs/src/index.js`:

``` js
const request = require('request');
request('http://jsonplaceholder.typicode.com/users/1', function (error, response, body) {
  console.log('error:', error);
  console.log('statusCode:', response && response.statusCode);
  console.log('body:', body);
});
``` 

``` bash
node ex/study_http_requests_in_nodejs/src/index.js | sed -n '1,6 p'
  ##> error: null
  ##> statusCode: 200
  ##> body: {
  ##>   "id": 1,
  ##>   "name": "Leanne Graham",
  ##>   "username": "Bret",
``` 

## v02: Use ES6 Promise

``` bash
cd ex/study_http_requests_in_nodejs && pnpm i --save request-promise-native && touch ex/study_http_requests_in_nodejs/src/index02.js
``` 

``` js
var request = require('request-promise-native');
request('http://jsonplaceholder.typicode.com/users/1').
  then( html => console.log('body:', html) ).
  catch( err => console.log('error:', err) );
``` 

``` bash
node ex/study_http_requests_in_nodejs/src/index02.js | sed -n '1,6 p'
  ##> body: {
  ##>   "id": 1,
  ##>   "name": "Leanne Graham",
  ##>   "username": "Bret",
  ##>   "email": "Sincere@april.biz",
  ##>   "address": {
``` 

## v03: Use JSON Response Data

Edit `ex/study_http_requests_in_nodejs/src/index03.js`:

``` js
...
request('http://jsonplaceholder.typicode.com/users/1').
  then( json => console.log(JSON.parse(json).company))
``` 

``` bash
node ex/study_http_requests_in_nodejs/src/index03.js
``` 

