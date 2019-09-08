---
title: "Studying JavaScript Promises"
date: 2018-03-18T16:37:50+03:00 
draft: false
description: ""
summaryLength: 13
summary_length: 23
tags: ["js"]
categories: ["software", "js"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: mertnuhoglu.com
resource_files:
- ex/study_js_promises_ex01/package.json
- ex/study_js_promises_ex01/script01.js
- ex/study_js_promises_ex01/script02.js
- ex/study_js_promises_ex01/script03.js
- ex/study_js_promises_ex01/script04.js
- ex/study_js_promises_ex01/script05.js
- ex/study_js_promises_ex01/script06.js
- ex/study_js_promises_ex01/script07.js
- ex/study_js_promises_ex01/script08.js
path: ~/projects/study/js/study_js_promises.Rmd

---

How to use JS Promises?

<!--more-->

<!-- toc -->


## v01: Basic Example 01

Studying the tutorials given in https://egghead.io/lessons/javascript-promises-with-es6

Source code in https://github.com/mertnuhoglu/study/tree/master/js/ex/study_js_promises_ex01/

``` bash
mkdir -p ex/study_js_promises_ex01
cd ex/study_js_promises_ex01
``` 

Edit `script01.js`

``` js
var d = new Promise((resolve, reject) => {
  if (true) {
    resolve('hello world');
  } else {
    reject('no bueno');
  }
});
d.then((data) => console.log('success : ', data));
d.catch((error) => console.log('error : ', error));
``` 

``` bash
node ex/study_js_promises_ex01/script01.js
  ## success :  hello world
``` 

Let's make `reject` be run:

Edit `script02.js`

``` js
...
  if (true) {
    resolve('hello world');
  } else {
    reject('no bueno');
  }
``` 

``` bash
node ex/study_js_promises_ex01/script02.js
  ## error :  no bueno
  ## (node:19988) UnhandledPromiseRejectionWarning: no bueno
  ## (node:19988) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 2)
  ## (node:19988) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
``` 

## v03: Asynchronous behaviour using Promises

Edit `script03.js`

``` js
...
  setTimeout(() => {
    if (true) {
      resolve('hello world');
    } else {
      reject('no bueno');
    }
  }, 1000);
``` 

``` bash
node ex/study_js_promises_ex01/script03.js
  ##> success :  hello world
``` 

## v04: Style: instead of `catch` append a new lambda parameter to `then`

``` js
...
d.then(
  (data) => console.log('success : ', data)
  , (error) => console.log('error : ', error)
);
``` 

``` bash
node ex/study_js_promises_ex01/script04.js
  ##> success :  hello world
``` 

## v05: Style: `catch` `then` methods can be chained

``` js
...
d.then( (data) => console.log('success : ', data) )
  .catch( (error) => console.log('error : ', error) );
);
``` 

``` bash
node ex/study_js_promises_ex01/script05.js
  ##> success :  hello world
``` 

Multiple `then` can be chained too:

``` js
...
d.then( (data) => console.log('success : ', data) )
  .then( (data) => console.log('success 2 : ', data) )
  .catch( (error) => console.log('error : ', error) );
);
``` 

``` bash
node ex/study_js_promises_ex01/script06.js
  ##> success :  hello world
  ##> success 2 :  undefined
``` 

But note the output: `success 2 :  undefined`. This is because the second chain gets its input data from the previous chain's return value.

``` js
...
d.then( (data) => {console.log('success : ', data); return 'foo';} )
  .then( (data) => console.log('success 2 : ', data) )
);
``` 

``` bash
node ex/study_js_promises_ex01/script07.js
  ##> success :  hello world
  ##> success 2 :  foo
``` 

## v08: Any Exception inside Promise triggers `catch` method

It doesn't matter where the exception is thrown. It will trigger `catch` method.

``` js
var d = new Promise((resolve, reject) => {
  throw new Error('error here');
  ...
``` 

``` bash
node ex/study_js_promises_ex01/script08.js
  ## error :  Error: error here
  ##     at Promise (/Users/mertnuhoglu/projects/jekyll/mertnuhoglu.com/content/tech/ex/study_js_promises_ex01/script08.js:2:9)
  ##     at new Promise (<anonymous>)
  ##     at Object.<anonymous> (/Users/mertnuhoglu/projects/jekyll/mertnuhoglu.com/content/tech/ex/study_js_promises_ex01/script08.js:1:71)
  ##     at Module._compile (internal/modules/cjs/loader.js:702:30)
  ##     at Object.Module._extensions..js (internal/modules/cjs/loader.js:713:10)
  ##     at Module.load (internal/modules/cjs/loader.js:612:32)
  ##     at tryModuleLoad (internal/modules/cjs/loader.js:551:12)
  ##     at Function.Module._load (internal/modules/cjs/loader.js:543:3)
  ##     at Function.Module.runMain (internal/modules/cjs/loader.js:744:10)
  ##     at startup (internal/bootstrap/node.js:238:19)
``` 



