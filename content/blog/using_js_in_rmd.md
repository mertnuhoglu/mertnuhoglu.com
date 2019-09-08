---
title: "How to Use Javascript inside Rmd Documents"
date: 2018-03-07T11:39:05+03:00 
draft: false
description: ""
tags: ["css"]
categories: ["software", "css"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: mertnuhoglu.com
resource_files:
- 
path: ~/projects/study/js/using_js_in_rmd.md

---

It is possible to put custom js code inside Rmarkdown documents.

<!--more-->

<!-- toc -->

## Embedding `<script>` tag

We can just embed any html tag such as `script` or `style` directly inside the Rmarkdown text. They will be included in the final html output.

    <script>window.alert("hello")</script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.2.0/require.min.js"></script>

## Embedding `js` blocks

Or we can use the usual code blocks.

JQuery is already included with Rmarkdown. So, we can use JQuery functions directly:

```js
$('.title').text("hello")
```

