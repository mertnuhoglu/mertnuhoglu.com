---
title: "EcmaScript Modules"
date: 2018-03-13T19:09:33+03:00 
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
- ex/ecmascript_modules_ex01.js
- ex/ecmascript_modules_ex01.html
- ex/ecmascript_modules_ex04/src/index.html
- ex/ecmascript_modules_ex04/src/index.js
- ex/ecmascript_modules_ex05/webpack.config.js
- ex/ecmascript_modules_ex06/webpack.config.js
- ex/ecmascript_modules_ex06/src/style.css
- ex/ecmascript_modules_ex06/src/index.js
path: ~/projects/study/js/ecmascript_modules.md

---

My study notes and examples on EcmaScript/JavaScript Modules

<!--more-->

<!-- toc -->


## v01: Basic Example 01

``` bash
cat ex/ecmascript_modules_ex01.js
  ## export function addTextToBody(text) {
  ##   const div = document.createElement('div');
  ##   div.textContent = text;
  ##   document.body.appendChild(div);
  ## }
``` 

    export function addTextToBody(text) { ... }

Using `import` inside `<script type="module">` tag:

``` bash
cat ex/ecmascript_modules_ex01.html
  ## <!doctype html>
  ## <html lang="en">
  ##   <head>
  ##     <meta charset="utf-8">
  ##     <title>Es Module Demo</title>
  ##   </head>
  ##   <body>
  ##     <main id="js-main" class="main"></main>
  ##     <script type="module">
  ##       import {addTextToBody} from './ecmascript_modules_ex01.js';
  ##       addTextToBody('Modules are pretty cool.');
  ##     </script>
  ##   </body>
  ## </html>
``` 

    import {addTextToBody} from './ecmascript_modules_ex01.js';
    addTextToBody('Modules are pretty cool.');

We have two options to open web page:

1. We can start a static web server in order to prevent CORS errors:

    ``` bash
    python3 -m http.server
    ``` 
    
    Open web page: 
    
    http://localhost:8000/ex/ecmascript_modules_ex01.html

2. Or we can open the web page from a static web server such as rawgit:

    https://rawgit.com/mertnuhoglu/study/master/js/ex/ecmascript_modules_ex01.html

## v04: Webpack Official Getting Started Tutorial

Following [Webpack Getting Started Tutorial](https://webpack.js.org/guides/getting-started/), I typed the following commands:

``` bash
mkdir -p ex/ecmascript_modules_ex04
cd ex/ecmascript_modules_ex04
npm init -y
npm install webpack webpack-cli -D
mkdir src
mkdir dist
npm install --save lodash
``` 

``` bash
cat ex/ecmascript_modules_ex04/src/index.html
  ## <!doctype html>
  ## <html>
  ##   <head>
  ##     <title>Getting Started</title>
  ##   </head>
  ##   <body>
  ##     <script src="bundle.js"></script>
  ##   </body>
  ## </html>
``` 

``` bash
cat ex/ecmascript_modules_ex04/src/index.js
  ## import _ from 'lodash';
  ## 
  ## function component() {
  ##   var element = document.createElement('div');
  ##   element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  ##   return element;
  ## }
  ## document.body.appendChild(component());
``` 

``` bash
cp src/index.html dist/index.html
./node_modules/.bin/webpack src/index.js --output dist/bundle.js
``` 

Note that, here we don't generate `dist/index.html`. Instead we simply copied it from `src/index.html`. Later, we will use `HtmlWebpackPlugin` to generate it.

``` bash
open dist/index.html
``` 

Open web page:

https://rawgit.com/mertnuhoglu/study/master/js/ex/ecmascript_modules_ex04/dist/index.html

## v05: Configuration File: `webpack.config.js`

Now, let's use configuration file to specify parameters of webpack:

I copied everything from `ecmascript_modules_ex04` into a new folder

``` bash
cp -R ex/ecmascript_modules_ex04 ex/ecmascript_modules_ex05
``` 

``` bash
cat ex/ecmascript_modules_ex05/webpack.config.js
  ## const path = require('path');
  ## module.exports = {
  ##   entry: './src/index.js',
  ##   output: {
  ##     filename: 'bundle.js',
  ##     path: path.resolve(__dirname, 'dist')
  ##   }
  ## }
``` 

Now, we will use configuration settings while running `webpack`:

``` bash
cd ex/ecmascript_modules_ex05
./node_modules/.bin/webpack --config webpack.config.js
``` 

Open web page:

https://rawgit.com/mertnuhoglu/study/master/js/ex/ecmascript_modules_ex05/dist/index.html

## v06: Asset Management

### Loading CSS

``` bash
cp -R ex/ecmascript_modules_ex05 ex/ecmascript_modules_ex06
cd ex/ecmascript_modules_ex06
npm install --save-dev style-loader css-loader
``` 

Add `style-loader` and `css-loader` to webpack configuration:

``` bash
cat ex/ecmascript_modules_ex06/webpack.config.js | sed -n '/module:/,/}/ p'
  ##   module: {
  ##     rules: [
  ##       {
  ##         test: /\.css$/,
  ##         use: [
  ##           'style-loader',
  ##           'css-loader'
  ##         ]
  ##       },
``` 

Now, we can `import` css files into the js scripts.

``` bash
cat ex/ecmascript_modules_ex06/src/style.css
  ## .hello {
  ##   color: blue;
  ## }
``` 

To use css in js:

    import './style.css'; // +
    ...
    element.classList.add('hello'); // +


``` bash
cat ex/ecmascript_modules_ex06/src/index.js
  ## import _ from 'lodash';
  ## import './style.css'; // +
  ## import Icon from './icon.png'; // +
  ## import Data from './data.csv'; // +
  ## 
  ## function component() {
  ##   var element = document.createElement('div');
  ##   element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  ##   element.classList.add('hello'); // +
  ##   var myIcon = new Image();
  ##   myIcon.src = Icon;
  ##   element.appendChild(myIcon);
  ##   console.log(Data);
  ##   return element;
  ## }
  ## document.body.appendChild(component());
``` 

### Loading Images

``` bash
npm install --save-dev file-loader
``` 

Add to `webpack.config.js`:

      {
        test: /\.(png|svg|jpg|gif)$/,
        use: [
          'file-loader'
        ]
      }

To load image files inside js:

    import Icon from './icon.png'; // +
    ...
    var myIcon = new Image();
    myIcon.src = Icon;
    element.appendChild(myIcon);

### Loading Data

``` bash
npm install --save-dev csv-loader
``` 

Add to `webpack.config.js`:

      {
        test: /\.(csv|tsv)$/,
        use: [
          'csv-loader'
        ]
      }

To load image files inside js:

    import Data from './data.csv'; // +
    ...
    console.log(Data);

Open web page:

https://rawgit.com/mertnuhoglu/study/master/js/ex/ecmascript_modules_ex06/dist/index.html

## v07: Output Management

``` bash
cp -R ex/ecmascript_modules_ex06 ex/ecmascript_modules_ex07
cd ex/ecmascript_modules_ex07
npm install --save-dev html-webpack-plugin
``` 

Add `HtmlWebpackPlugin` to webpack configuration:

    const HtmlWebpackPlugin = require('html-webpack-plugin');
    ...
    plugins: [
      new HtmlWebpackPlugin({
        title: 'Output Management'
      })
    ],

`HtmlWebpackPlugin` generates `dist/index.html`.

### Cleaning up the `/dist` folder

``` bash
npm install --save-dev clean-webpack-plugin
``` 

Add `CleanWebpackPlugin` to webpack configuration:

    const CleanWebpackPlugin = require('clean-webpack-plugin');
    ...
    plugins: [
      new CleanWebpackPlugin(['dist']),

Open web page:

https://rawgit.com/mertnuhoglu/study/master/js/ex/ecmascript_modules_ex07/dist/index.html

## v08: Development

### Using source maps

How to track down errors? Webpack bundles source files into one bundle.js. When an error occurs how are we going to know from which origin file it comes from?

So, we will split our webpack configuration into two: one for development environment, one for production environment.

``` bash
cp -R ex/ecmascript_modules_ex07 ex/ecmascript_modules_ex08
cd ex/ecmascript_modules_ex08
``` 

Add `inline-source-map` to webpack configuration:

    devtool: 'inline-source-map',

### Watching Updates in Files

Every time we make a change in some file, we need to run `npm run build`.

Let's run this command automatically after each change.

``` bash
npm install --save-dev webpack-dev-server
``` 

Update `webpack.config.js`

    devServer: {
      contentBase: './dist'
    }

This setups a static web server for `./dist` directory on `localhost:8080`.

Update `package.json`

    "start": "webpack-dev-server --open",

Run `npm start`

Open web page: http://localhost:8080

Since these settings are for development purpose, we won't open this page from static web server.

## v09: Hot Module Replacement HMR

HMR allows any file to be updated without refresh or recompiling.

``` bash
cp -R ex/ecmascript_modules_ex08 ex/ecmascript_modules_ex09
cd ex/ecmascript_modules_ex09
``` 

### Enabling HMR

``` bash
cp -R ex/ecmascript_modules_ex08 ex/ecmascript_modules_ex09
cd ex/ecmascript_modules_ex09
``` 

`webpack.config.js`

    devServer: {
      contentBase: './dist',
      hot: true
    },
    ...
    new webpack.NamedModulesPlugin(),
    new webpack.HotModuleReplacementPlugin()

Run `npm start`

Open web page: http://localhost:8080

## v10: Using JQuery

Using jquery and its addin libraries according to the ES module rules is not as simple as it should be.

I follow the instructions given in [Webpack 2, jQuery plugins and imports-loader](https://medium.com/@stefanledin/webpack-2-jquery-plugins-and-imports-loader-e0d984650058) by Stefan Ledin.

Use `imports-loader` webpack plugin

``` bash
cp -R ex/ecmascript_modules_ex09 ex/ecmascript_modules_ex10
cd ex/ecmascript_modules_ex10
npm install --save-dev imports-loader
npm install --save jquery
``` 

Put jquery addin library chosen.js into src/vendor

``` bash
tree ex/ecmascript_modules_ex10
  ## ex/ecmascript_modules_ex10 [error opening dir]
  ## 
  ## 0 directories, 0 files
``` 

Add `imports-loader` for jquery to `webpack.config.js`

      {
        test: /vendor\/.+\.(jsx|js)$/,
        use: [
          'imports-loader?jQuery=jquery,$=jquery,this=>window',
        ]
      },

Add `import` statements for jquery and its plugins to `index.js`

    import {$, jQuery} from 'jquery';
    window.$ = $;
    window.jQuery = jQuery;
    import "./vendor/chosen.jquery.min";

Run `npm start`

Open web page: http://localhost:8080

Source code in https://github.com/mertnuhoglu/study/tree/master/js/ex/ecmascript_modules_ex10/

## v11: Using JQuery 02

Try `ProvidePlugin`

I follow the instructions given in [Webpack Documentation](https://webpack.js.org/plugins/provide-plugin/).

Use `ProvidePlugin` webpack plugin

``` bash
cp -R ex/ecmascript_modules_ex10 ex/ecmascript_modules_ex11
cd ex/ecmascript_modules_ex11
rm -rf node_modules
npm install --save-dev html-webpack-plugin
npm install
``` 

    new webpack.ProvidePlugin({
      $: 'jquery',
      jQuery: 'jquery'
    })

Now, we can use `$` anywhere in the scripts. 

Source code in https://github.com/mertnuhoglu/study/tree/master/js/ex/ecmascript_modules_ex11/

