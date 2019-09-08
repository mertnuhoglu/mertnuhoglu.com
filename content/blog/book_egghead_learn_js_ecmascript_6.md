---
title: "Notes from Training: Learn JS Ecmascript 6 - Egghead"
date: 2017-11-24T10:21:59+03:00
draft: false
description: ""
tags: ["book_notes", "js"]
categories: ["software", "js"]
type: post
url:
---

My reading notes from the book

<!--more-->

## Notes

    arrow function
      syntax
        var createGreeting = function(x,y) {..}
        -->
        var arrowGreeting = (x,y) => ...;
        --> if one parameter
        var arrowGreeting = x => ...;
      ex: accessing this in outer scope
        opt1:
          var d = {
            name: ".."
            receive: function() {
              var that = this;
              this.handle(.., function(..) {
                that.name;
        opt2: removing that = this
          var d = {
            name: ".."
            receive: function() {
              this.handle(.., x => this.name)
    var vs. let
      es5: var declaration
        let m = "hi"
        { let m = "bye" }
        # m is overridden
      es6: let declaration uses block scoping
        let m = "hi"
        { let m = "bye" }
        # m is not overridden
    default values
      function greet(x, name = "John") {..}
      greet("hello")
      # x <- "hello"
    const
      es6: to make variables constant
        const x = "..."
      this is constant reference (not value)
        const x = {}
        x.foo = "a"
    shorthand properties
      shorthand
        let first = "ali"
        let person = {first}
        # person.first is "ali"
        --> as if
        let person = {first: first}
    object enhancements
      declaring a function on an object
        -- es6:
          var car = {
            go() { console.log("help"); }
          };
          car.go()
        -- es5:
          var car = {
            go: function() { console.log("help"); }
      trick: computed property
        -- es6
          var car = {
            ["go"]: function() { .. }
          car.go()
        -- same:
          car["go"]()
        -- same:
          var drive = "go"
          var car = {
            [drive]: function() {..}
          car.go()
    spread operator
      ex
        console.log([1,2])
        # [1,2]
        console.log(...[1,2])
        # 1 2
      ex: push elements into other array
        let first = [1,2]
        let second = [3,4]
        first.push(...second)
        # [1,2,3,4]
    string templates
      es5
        var h = "Hello "
        h + "World"
      es6
        `${h} World`
      can do expression
        `${3+2}`
        `${new Date().getHours()}`
    destructuring assignment
      es5
        var obj = { color: "blue" }
        obj.color
      es6
        opt1
          var {color} = { color: "blue" }
          color 
          # blue
        opt2: multiple properties
          var {color, pos} = { color: "blue", pos: 3 }
          color 
          pos
        opt3: function that returns
          function generateObj() { 
            return {color: "blue", pos: 3 }
          var {color} = generateObj()
          color 
        opt4: rename names
          var {color: col} = generateObj()
          col
        opt5: array destructuring
          var [first,,] = ["red", "blue", "yellow"]
          first
          # red
        opt6: forEach arrow syntax
          people.forEach( ({firstName}) => console.log(firstName) )
    modules
      math/addition.js
        function sumTwo(..) {..}
        export { sumTwo }
      main.js
        import {sumTwo} from 'math/addition'
        sumTwo(..)
      opts
        export function sumTwo(..)
        import {sumTwo as add} from 'math/addition'
        import * as addition
          addition.sumTwo()
        export var users
        lodash
          import * as _ from 'lodash'
          _.where(users, {age: 36})
    converting an array like object into an array with ArrayFrom
      Array.from
        const products = document.querySelectorAll('.product')
        # NodeList type
        -->
        const products = Array.from(document.querySelectorAll('.product'))
      use like array
        products
          .filter(product => parseFloat(product.innerHTML < 10)
          .forEach(product => product.style.color = 'red')
        # highlighting products less than 10 $
    promises 
      basic skeleton
        var d = new Promise()
        d.then()
        d.catch()
      ex
        var d = new Promise((resolve, rejct) => {
          if (true) {
            resolve('hello')
          } else {
            reject('no bueno')
          }
        })
        d.then((data) => console.log('success: ', data)
        # data: hello
        d.catch((error) => console.error('error: ', data)
      ex: setTimeout
        var d = new Promise((resolve, rejct) => {
        setTimeout( () => {
            if (true) {
              resolve('hello')
            } else {
              reject('no bueno')
            }
          }, 2000)
        })
      ex: error in then
        d.then( (data) => .., (error) => )
      ex: chain then, error together
        d.then()
          .error()
      ex: chain multiple then
        d.then(..)
          .then(..)
          .error(..)
    generators
      basic
        function* greet() {
          ..
        let greeter = greet()
        # greeter is an object with next() function
        let next = greeter.next()
        console.log(next)
      we need to yield something
        function* greet() {
          yield "hello"
    maps and weakmaps
      basic
        var m = new Map()
        m.set("foo", "bar")
        m.set("hello", "world")
        m.get("foo")
      for
        for (var value of m.values())
        for (var key of m.keys())
        for (var [key,value] of m.entries())
      functions and objects as keys
        m.set({}, 'bar')
        m.set(function(){}, 'x')
    rest (remaining) parameters vs arguments keyword
      arguments
        function m() {
          console.log(arguments)
        }
        m(1,2,3)
        # object Arguments{0,1,2}
      rest ...
        add: function(category, ...items) 
    parameter object destructuring with required values
      basic
        function ajax({
          type = "get",
          ...
          } = {}) {...}
        ajax({})
        # default values
      required parameter
        function required(name) {
          throw new Error(..)
        function ajax({
          url = required("url")
        # if url is not passed, throws error
    let keyword
