---
title: "Mockup REST API Generator: JSON Server"
date: 2018-02-27T16:28:02+03:00 
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
- ex/db.json
path: ~/projects/study/js/mockup_rest_api_jsonserver.md
state: wip

---

json-server is a simple server to generate REST API.

<!--more-->

Install:

    npm install -g json-server

``` bash
cat ex/db.json
  ##> {
  ##>   "posts": [
  ##>     { "id": 1, "title": "json-server", "author": "typicode" }
  ##>   ],
  ##>   "comments": [
  ##>     { "id": 1, "body": "some comment", "postId": 1 }
  ##>   ],
  ##>   "profile": { "name": "typicode" }
  ##> }
``` 

``` bash
json-server --watch ex/db.json
``` 

``` bash
curl http://localhost:3000/posts/1
  ## {
  ##   "id": 1,
  ##   "title": "json-server",
  ##   "author": "typicode"
  ## }
``` 
