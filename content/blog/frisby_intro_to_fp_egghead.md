---
title: "Notes from Training: Frisby Intro to FP - egghead Brian Lonsdorf"
date: 2017-11-24T10:01:46+03:00
draft: false
description: ""
tags: ["book_notes", "functional_programming"]
categories: ["software", "functional_programming"]
type: post
url:
---

My reading notes from the video and book tutorials of Brian Lonsdorf. This is one of the best training videos I have ever seen.

<!--more-->

<!-- toc -->


## 01-egghead-javascript-linear-data-flow-with-container-style-types-box.mp4

    ex: imperative 01
      const nextChar = str => {
        const trimmed = str.trim()
        const number = parseInt(trimmed)
        const nextNumber = number + 1
        return String.fromCharCode(nextNumber)
      }
      const result = nextChar('  64 ')
      console.log(result)
    ex: v2
      const nextChar = str => 
        String.fromCharCode( parseInt( str.trim()) + 1)
      const result = nextChar('  64 ')
      console.log(result)
    ex: v3
      const nextChar = str => 
        [str] 
        .map(s => s.trim())
        .map(s => parseInt(s))
        .map(i => String.fromCharCode(i))
      const result = nextChar('  64 ')
      console.log(result)
      # each step has its operation contained
    ex: v4: Box instead of []
      const Box = x =>
      ({
        map: f => Box(f(x)),
        inspect: () => `Box(${x})`
      })
      const nextChar = str => 
        Box(str)
        .map(s => s.trim())
        .map(s => parseInt(s))
        .map(i => String.fromCharCode(i))
      const result = nextChar('  64 ')
      console.log(result)
      // Box(A)
    ex: v5: move it out of Box
      const Box = x =>
      ({
        map: f => Box(f(x)),
        fold: f => f(x),
        inspect: () => `Box(${x})`
      })
      const nextChar = str => 
        Box(str)
        .map(s => s.trim())
        .map(s => parseInt(s))
        .map(i => String.fromCharCode(i))
        .fold(c => c.toLowerCase())
    ex: applyDiscount('$5.00', '20%')
      const moneyToFloat = str =>
        Box(str)
        .map(s => s.replace(/\$/g, ''))
        .fold(r => parseFloat(r))
      const percentToFloat = str =>
        Box(str.replace(/\%/g, ''))
        .map(replaced => parseFloat(replaced))
        .fold(number => number * 0.01)
      const applyDiscount = (price, discount) => {
        const cost = moneyToFloat(price)
        const savings = percentToFloat(discount)
        return cost - cost * savings
      }
      const result = applyDiscount('$5.00', '20%')
      console.log(result)
      // 4
    use map inside applyDiscount
      make applyDiscount sequential
    ex: multiple variables in a map
      const moneyToFloat = str =>
        Box(str)
        .map(s => s.replace(/\$/g, ''))
        .map(r => parseFloat(r))
      const percentToFloat = str =>
        Box(str.replace(/\%/g, ''))
        .map(replaced => parseFloat(replaced))
        .map(number => number * 0.01)
      const applyDiscount = (price, discount) => 
        moneyToFloat(price)
        .map(cost =>
          percentToFloat(discount)
          .map(savings =>
            cost - cost * savings))
      const result = applyDiscount('$5.00', '20%')
      console.log(result)
      // Box([object Object])
      # to deal with two variables
        we used nesting with closures
        we have two assignments, so we have two level Box as a result
      # problem: we have two Boxes nested as result
    v3: unnest Boxes
      const applyDiscount = (price, discount) => 
        moneyToFloat(price)
        .fold(cost =>
          percentToFloat(discount)
          .fold(savings =>
            cost - cost * savings))
      const result = applyDiscount('$5.00', '20%')
      console.log(result)
      // 4
      # linear flow
        variables are introduced using closures

## 03-egghead-javascript-composable-code-branching-with-either.mp4

    code: Either
      // const Either = Right || Left
      const Right = x =>
      ({ 
        map: f => Right(f(x)),
        inspect: () => `Right(${x})`
      })
    ex: use Right and map
      const result = Right(3).map(x => x + 1).map(x => x / 2)
      console.log(result)
      // Right(2)
    code: define Left 
      const Left = x =>
      ({
        map: f => Left(x),
        inspect: () => `Left(${x})`
      })
    ex: use Left and map
      const result = Left(3).map(x => x + 1).map(x => x / 2)
      console.log(result)
      // Left(3)
    why is this useful?
      input may be either right or left
        const result = rightOrLeft(3).map(x => x + 1).map(x => x / 2)
      fold has to accept two functions: f, g for each case Right and Left
    code: define fold
      // const Either = Right || Left
      const Right = x =>
      ({ 
        map: f => Right(f(x)),
        fold: (f, g) => g(x),
        inspect: () => `Right(${x})`
      })
      const Left = x =>
      ({
        map: f => Left(x),
        fold: (f, g) => f(x),
        inspect: () => `Left(${x})`
      })
      const result = Right(3).map(x => x + 1).map(x => x / 2).fold(x => 'error', x => x)
      console.log(result)
      // 2
      const result = Left(3).map(x => x + 1).map(x => x / 2)
      console.log(result)
      // error
      # fold: branch the code
        run either left, or right side
        null checks
    ex: findColor 
      const findColor = name => 
        ({red: '#ff4444', blue: '#3b5998', yellow: '#fff68f'})[name]
      const result = findColor('red').slice(1).toUpperCase()
      console.log(result)
      // #FF4444
    what if the color doesn't exist
    ex: findColor error
      const result = findColor('green').slice(1).toUpperCase()
      console.log(result)
      // throws error: undefined
    ex: use Either for null check
      const findColor = name => {
        const found = ({red: '#ff4444', blue: '#3b5998', yellow: '#fff68f'})[name]
        return found ? Right(found) : Left(null)
      }
      const result = findColor('green')
        .map(c => slice(1))
        .fold(e => 'no color', 
              c => c.toUpperCase())
      console.log(result)
      // no color
    ex: color is found
      const result = findColor('blue')
        .map(c => slice(1))
        .fold(e => 'no color', 
              c => c.toUpperCase())
      console.log(result)
      // 3B5998
    findColor has now a branching
      abstract this branching to fromNullable
    ex: fromNullable
      const fromNullable = x =>
        x != null ? Right(x) : Left(null)
      const findColor = name => 
        fromNullable({red: '#ff4444', blue: '#3b5998', yellow: '#fff68f'})[name]

## 04-egghead-javascript-composable-error-handling-with-either.mp4

    ex: fs with try catch
      const fs = require('fs')
      const getPort = () => {
        try {
          const str = fs.readFileSync('config.json')
          const config = JSON.parse(str)
          return config.port
        } catch(e) {
          return 3000
        }
      }
      const result = getPort()
      # cat config.json
      # {"port": 8888}
      console.log(result)
      // 8888
    use Either to get rid of try catch
    ex: fs with try catch
      const tryCatch = f => {
        try {
          return Right(f())
        } catch(e) {
          return Left(e)
        }
      }
      const getPort = () =>   
        tryCatch(() => fs.readFileSync('config.json'))
        .map(c => JSON.parse(c))
        .fold(e => 3000,
              c => c.port)
      const result = getPort()
      console.log(result)
      // 8888
      # 170307.02.jpg
        evaluation for Right:
          Right(f()).fold(.., c => c.port)
          = (c => c.port)(f())
          = f().port
          = fs.readFileSync(..).port
          = 8888
          note that:
            Right = x => ({ fold: (f,g) => g(x) ..})
            x = f() = fs.readFileSync(..)
            g = c => c.port 
        evaluation for Left:
          Left(ex).fold( e => 3000, ..)
          = (e => 3000)(ex)
          = 30000
          note that:
            Left = x => ({ fold: (f,g) => f(x) ..})
            x = ex // from catch(ex)
            f = e => 3000
    what if json file cannot be parsed
    ex: json.parse with try catch
      const getPort = () =>   
        tryCatch(() => fs.readFileSync('config.json')) // Right('')
        .map(c => tryCatch(() => JSON.parse(c))) // Right(Right(''))
        .fold(e => 3000,
              c => c.port)
    use chain to unnest nested Right
    ex: use chain
      const Right = x => ({
        chain: f = f(x), .. 
      const Left = x => ({
        chain: f = Left(x), ..
      const getPort = () =>   
        tryCatch(() => fs.readFileSync('config.json')) // Right('')
        .chain(c => tryCatch(() => JSON.parse(c))) // Right('')
        .fold(e => 3000,
              c => c.port)

## 05-egghead-javascript-a-collection-of-either-examples-compared-to-imperative-code.mp4

    ex: openSite
      const openSite = () => {
        if (current_user) {
          return renderPage(current_user)
        } else {
          return showLogin()
        }
      }
      -->
      const openSite = () =>
        fromNullable(current_user)
        .fold(showLogin, renderPage)
    ex: getPrefs
      const getPrefs = user => {
        if (user.premium) {
          return loadPrefs(user.preferences)
        } else {
          return defaultPrefs
        }
      }
      -->
      const getPrefs = user =>
        (user.premium ? Right(user) : Left('not premium'))
        .map(u => u.preferences)
        .fold(() => defaultPrefs, prefs => loadPrefs(prefs))
    ex: streetName
      const streetName = user => {
        const address = user.address
        if(address) {
          const street = address.street
          if(street) {
            return street.name
          }
        }
        return 'no street'
      }
      -->
      const streetName = user => 
        fromNullable(user.address)
        .chain(a => fromNullable(a.street))
        .map(s => s.name)
        .fold(e => 'no street', n => n)
    ex: concatUniq
      const concatUniq = (x, ys) => {
        const found = ys.filter(y => y === x)[0]
        return found ? ys : ys.concat(x)
      }
      -->
      const concatUniq = (x, ys) => 
        fromNullable(ys.filter(y => y === x)[0])
        .fold(() => ys.concat(x), y => ys)
    ex: wrapExamples
      wrapExamples = example => {
        if(example.previewPath) {
          try {
            example.preview = fs.readFileSync(example.previewPath)
          } catch(e) {}
        }
        return example
      }
      -->
      const readFile = x => tryCatch(() => fs.readFile(x))
      wrapExamples = example => 
        fromNullable(example.previewPath)
        .chain(readFile)
        .fold(() => example, 
              ex => Object.assign({preview: p}, ex))
    ex: parseDbUrl
      const parseDbUrl = cfg => {
        try {
          const c = JSON.parse(cfg)
          if(c.url) {
            return c.url.match(/postgres:../)
          }
        } catch(e) {
          return null
        }
      }
      -->
      const parseDbUrl = cfg => {
        tryCatch(() => JSON.parse(cfg))
        .chain(c => fromNullable(c.url))
        .fold(e => null,
              url => url.match(/../))

## 07-egghead-javascript-semigroup-examples.mp4

    semigroup
      type that has concat method
    ex
      const res = "a".concat("b").concat("c")
      const res = [1,2].concat([3,4]).concat([5,6])
    associativity
      const res = [1,2].concat(([3,4].concat([5,6]))
      (1+1)+1 = 1+(1+1)
    code: Sum
      const Sum = x => ({
        x,
        concat: o => Sum(x + o.x),
        inspect: () => `Sum(${x})`
      })
      const res = Sum(1).concat(Sum(2))
    destructure concat's arguments to make it more readable
    code: Sum.concat
      concat: ({x: y}) => Sum(x + y),
    semigroup of Booleans
      true && false // false
      true && true // false
      const res = All(true).concat(All(false)) // All(false)
    code: Booleans All
      const All = x => ({
        x,
        concat: ({x: y}) => All(x && y),
        inspect: () => `All(${x})`
      })
    semigroup: First element of a pair tuple
      const res = First("ali").concat(First("mert"))
      console.log(res)
      // First(ali)
    code: First
      const First = x => ({
        x,
        concat: _ => First(x),
        inspect: () => `First(${x})`
      })

## 08-egghead-javascript-failsafe-combination-using-monoids.mp4

    when you combine something
      think about semigroups
      result of concat is semigroup element as well
    ex: combining two accounts
      const acct1 = {name: 'Nico', isPaid: true, points: 10, friends: ['Franklin']}
      const acct2 = {name: 'Nico', isPaid: false, points: 2, friends: ['Gatsby']}
    combining each element, define rules for how to combine each element
    ex: rules for combining each element
      const acct1 = {name: First('Nico'), isPaid: All(true), points: Sum(10), friends: ['Franklin']}
      const acct2 = {name: First('Nico'), isPaid: All(false), points: Sum(2), friends: ['Gatsby']}
      const res = acct1.concat(acct2)
    objects should support concat as well, use Map from immutable-ext
    ex: immutable-ext Map
      const {Map} = require("immutable-ext")
      const acct1 = Map({name: First('Nico'), isPaid: All(true), points: Sum(10), friends: ['Franklin']})
      const acct2 = Map({name: First('Nico'), isPaid: All(false), points: Sum(2), friends: ['Gatsby']})
      const res = acct1.concat(acct2)
      console.log(res.toJS())

## 09-egghead-javascript-a-curated-collection-of-monoids-and-their-uses.mp4

![img/ss-154.png](/images/ss-154.png)

    zero element
      1 + 0 // 1
      2 + 0 // 2
      x + 0 // x
    if we have zero element in a semigroup, then it is a monoid
    code: Sum monoid
      const Sum = x => ({
        x,
        concat: ({x: y}) => Sum(x + y),
        inspect: () => `Sum(${x})`
      })
      Sum.empty = () => Sum(0)
      const res = Sum.empty().concat(Sum(1).concat(Sum(2)))
    code: All monoid
      All.empty = () => All(true)
      const res = All.empty().concat(All(true).concat(All(false)))
    code: First monoid
      impossible
      First doesn't have identity element
      First('hello').concat(x) // hello
    code: Min monoid
      const Min = x => ({
        x, 
        concat: ({x: y}) => Min(x < y ? x : y)
      })
      Min.empty = () => Min(Infinity)
    code: Either semiring
      const Right = x =>
      ({ 
        map: f => Right(f(x)),
        fold: (f, g) => g(x),
        concat: o => 
          o.fold(e => Left(e),
                 r => Right(x.concat(r))),
        inspect: () => `Right(${x})`
      })
      const Left = x =>
      ({
        map: f => Left(x),
        fold: (f, g) => f(x),
        concat: o => Left(x),
        inspect: () => `Left(${x})`
      })
    code: First of Either monoid
      const First = either => ({
        fold: f => f(either),
        concat: o =>
          either.isLeft ? o : First(either)
      })
      First.empty = () => First(Left())
    ex: find the first number that is greater than 4 from a list
      const find = (xs, f) =>
        List(xs)
        .foldMap(x =>
          First(f(x) ? Right(x) : Left()), First.empty())
        .fold(x => x)
      find([3,4,5,6,7], x => x > 4)
      // Right(5)
    ex: Fn: filter words with vowels and long
      /Users/mertnuhoglu/Dropbox/public/img/ss-154.png
      const All = x => ({
        x,
        concat: ({x: y}) => All(x && y),
        inspect: () => `All(${x})`
      })
      const Fn = f => ({
        fold: f,
        concat: o =>
          Fn(x => f(x).concat(o.fold(x)))
      })
      const hasVowels = x => !!x.match(/[aeiou]/ig)
      const longWord = x => x.length >= 5
      const both = Fn(compose(All, hasVowels))
                   .concat(Fn(compose(All, longWord)))
      ['gym', 'bird', 'lilac']
      .filter(x => both.fold(x).x)
      // [lilac]
    @mine: why we didn't use compose, instead of concat above?
      instead of this:
        const both = Fn(compose(All, hasVowels))
                     .concat(Fn(compose(All, longWord)))
      can we write as such:
        const both = compose(All, hasVowels, longWord)
      no, because longWord returns boolean but hasVowels expects String
      thus they don't compose 
        both.fold(x) = fn1.concat(fn2).fold(x)
      we use concat, when the functions signatures don't allow composition
    @mine: evaluate these functions
      ahv = compose(All, hasVowels)
      alw = compose(All, longWord)
      both = Fn(ahv).concat(alw)
      both.fold(x) = Fn(ahv).concat(alw).fold(x)
        = Fn(a => ahv(a).concat(alw.fold(a))).fold(x) // Fn.concat evald
        = Fn(a => All(ahv(a) && alw(a))).fold(x) // All.concat and All.fold evald
        = (a => All(ahv(a) && alw(a))(x)  // Fn.fold evald
        = All(ahv(x) && alw(x))
    @mine: how to understand concat, fold etc
      concat: join two objects like lists
      fold: open the contents of the box
    ex: Pair semiring
      const Pair = (x, y) => ({ x, y,
        concat: ({x: x1, y: y1}) =>
          Pair(x.concat(x1), y.concat(y1)) 
      })

## 10-egghead-javascript-unboxing-things-with-foldable.mp4

    ex: fold sum
      const {Map, List} = require('immutable-ext')
      const {Sum} = require('../monoid')
      const res = [Sum(1), Sum(2), Sum(3)]
                  .reduce((acc, x) => acc.concat(x), Sum.empty())
      console.log(res)
      // Sum(6)
    ex: use Sum.fold instead of List.reduce
      const res = List.of(Sum(1), Sum(2), Sum(3))
                  .fold(Sum.empty())
      # fold, concats each item
    Box.fold takes a function. But List.fold takes a value. why?
      fold: removes Box and transform into one value
    ex: use Map.fold
      const res = Map({brian: Sum(1), sara: Sum(2)})
                  .fold(Sum.empty())
    ex: Map with integers not Sum()
      const res = Map({brian: 1, sara: 2})
        .map(x => Sum(x))
        .fold(Sum.empty())
    ex: List with integers not Sum()
      const res = List.of(1,2,3)
        .map(Sum)
        .fold(Sum.empty())
    map then fold is so common, we use foldMap
    ex: List foldMap
      const res = List.of(1,2,3)
        .foldMap(Sum, Sum.empty())
    @mine: why we don't specify acc in foldMap?
      fold, concats elements 
      concat contains accumulation logic inside
      const Sum = x => ({ x,
        concat: ({x:y}) => Sum(x + y),

## 11-egghead-javascript-delaying-evaluation-with-lazybox.mp4

    ex: previous ex: v5: move it out of Box
      const Box = x =>
      ({
        fold: f => f(x),
        map: f => Box(f(x)),
        inspect: () => `Box(${x})`
      })
      const nextChar = str => 
        Box(str)
        .map(s => s.trim())
        .map(s => parseInt(s))
        .map(i => String.fromCharCode(i))
        .fold(c => c.toLowerCase())
    ex: use LazyBox instead of Box
      const LazyBox = g =>
      ({
        fold: f => f(g()),
        map: f => LazyBox(() => f(x))
      })
      const nextChar = LazyBox(() => '  64 ')
        .map(s => s.trim())
        .map(s => parseInt(s))
        .map(i => String.fromCharCode(i))
        //.fold(c => c.toLowerCase())
      console.log(result)
      // { fold: [Function], map: [Function] }
    it doesn't run until we don't call fold
      this gives us purity by virtue of laziness
      Promise, Observable, Stream work like this
      nothing runs until fold

## 12-egghead-javascript-capturing-side-effects-in-a-task.mp4

    take Task
    ex: Task
      const Task = require('data.task')
      Task.of(1) // Task(1)
      .fork(e => console.log('err', e),
            x => console.log('success', x))
      // success 1
    ex: Task rejected
      const Task = require('data.task')
      Task.rejected(1) // Task(1)
      .fork(e => console.log('err', e),
            x => console.log('success', x))
      // err 1
    ex: Task map when rejected
      const Task = require('data.task')
      Task.rejected(1) // Task(1)
      .map(x => x + 1)
      .fork(e => console.log('err', e),
            x => console.log('success', x))
      // err 1
      # ignores map in rejected case
    ex: Task map when success
      const Task = require('data.task')
      Task.of(1) // Task(1)
      .map(x => x + 1)
      .fork(e => console.log('err', e),
            x => console.log('success', x))
      // success 2
    ex: chain with another Task
      const Task = require('data.task')
      Task.of(1) // Task(1)
      .map(x => x + 1)
      .chain(x => Task.of(x + 1))
      .fork(e => console.log('err', e),
            x => console.log('success', x))
      // success 3
    ex: chain with rejected Task
      const Task = require('data.task')
      Task.rejected(1) // Task(1)
      .map(x => x + 1)
      .chain(x => Task.of(x + 1))
      .fork(e => console.log('err', e),
            x => console.log('success', x))
      // err 1
      # ignores map and chain
    there is a different constructor that allows side effects 
    ex: launchMissiles
      const Task = require('data.task')
      const launchMissiles = () =>
        new Task((rej, res) => {
          console.log("launch missiles!")
          res("missile")
        }
      launchMissiles()
      .map(x => x + "!")
      .fork(e => console.log('err', e),
            x => console.log('success', x))
      // launch missiles!
      // success missile!
    ex: don't call fork
      launchMissiles()
      .map(x => x + "!")
      // nothing happens
    since fork runs the program, we can separate app from client
    ex: separate app from client
      const app = launchMissiles().map(x => x + "!")
      app.map(x => x + "!")
        .fork(e => console.log('err', e),
              x => console.log('success', x))
      # so the client can extend app

## 13-egghead-javascript-using-task-for-asynchronous-actions.mp4

    ex: imperative file reading and writing
      const Task = require('data.task')
      const fs = require('fs')
      const app = () =>
        fs.readFile('config.json', 'utf-8', (err, contents) => {
          if(err) throw err
          const newContents = contents.replace(/8/g, '6')
          fs.writeFile('config1.json', newContents, (err, _) => {
            if(err) throw err
            console.log('success!')
          })
        })
    replace error handling with Tasks
    ex: imperative file reading and writing
      const Task = require('data.task')
      const fs = require('fs')
      const readFile = (filename, enc) =>
        new Task((rej, res) =>
          fs.readFile(filename, enc, (err, contents) =>
            err ? rej(err) : res(contents)))
      const writeFile = (filename, contents) =>
        new Task((rej, res) =>
          fs.writeFile(filename, contents, (err, success) =>
            err ? rej(err) : res(success)))
      const app = () =>
        readFile('config.json', 'utf-8')
        .map(contents => contents.replace(/8/g, '6'))
        .chain(contents => writeFile('config1.json', contents))
      app().fork(e => console.log(e),
        x => console.log('success'))
    @mine: why not map instead of chain writeFile
      writeFile returns a Task
      if we use map, fork won't work
        .map(contents => writeFile('config1.json', contents))
    @mine: why not use writeFile directly as we do with readFile
      const app = () =>
        readFile('config.json', 'utf-8')
        .map(contents => contents.replace(/8/g, '6'))
        .writeFile('config1.json', _)
      map returns a Task
        Task doesn't have writeFile
      moreover: then what will be second arg of writeFile?
      readFile was the starting line, not like writeFile

## 14-egghead-javascript-you-ve-been-using-functors.mp4

    we have been using Functors
    definition of Functor
      any Type with map() method
      and obeys some laws
    code: functor laws
      defs
        const Box = require('./box')
        const Task = require('data.task')
        const Either = require('./either')
        const {Right, Left, fromNullable} = Either
        const {List, Map} = require('immutable-ext')
      Functor map f map g = Functor map f.g
        fx.map(f).map(g) == fx.map(x => g(f(x)))
    ex: proof of law
      const res1 = Box('squirrels')
        .map(s => s.substr(5))
        .map(s => s.toUpperCase())
      const res2 = Box('squirrels')
        .map(s => s.substr(5).toUpperCase())
      console.log(res1, res2)
      // Box(RELS) Box(RELS)
    ex: this works with any types. test Right, Left
      const res1 = Right('squirrels')
        .map(s => s.substr(5))
        .map(s => s.toUpperCase())
      const res2 = Right('squirrels')
        .map(s => s.substr(5).toUpperCase())
      console.log(res1, res2)
      // Right(RELS) Right(RELS)
      const res1 = Left('squirrels')
        .map(s => s.substr(5))
        .map(s => s.toUpperCase())
      const res2 = Left('squirrels')
        .map(s => s.substr(5).toUpperCase())
      console.log(res1, res2)
      // Left(squirrels) Left(squirrels)
    code: second functor law
      Functor map id = id Functor
        fx.map(id) == id(fx)
      const id = x => x
      const res1 = Box('crayons').map(id)
      const res2 = id(Box('crayons'))
      console.log(res1, res2)
      // Box(crayons) Box(crayons) 
      const res1 = List.of('crayons').map(id)
      const res2 = id(List.of('crayons'))
      console.log(res1, res2)
      // List(crayons) List(crayons) 

## 15-egghead-javascript-lifting-into-a-pointed-functor.mp4

    lifting a value into a type
    ex:
      Task.of('hello')
      Either.of('hello')
      Box.of(100)
    this is regardles of constructor complexity
    ex: complex constructor 
      new Task((rej, res) =>
        res('hello'))
      # this is not a generic interface

## 16-egghead-javascript-you-ve-been-using-monads.mp4

    we have been using monads
    Box, Either, Task, List are monads
    because they support
      F.of
      chain (flatMap, bind, >>=)
    ex: httpGet
      httpGet('/user')
      .map(user => httpGet(`/comments/${user.id}`)) // Task(Task([Comment]))
      # this is difficult to work with
      # nested boxing
    chain flattens these types
    ex: use chain to flatten
      httpGet('/user')
      .chain(user => httpGet(`/comments/${user.id}`)) // Task([Comment])
    monads allow us to nest computation 
    ex: use chain to flatten
      httpGet('/user')
      .chain(user => httpGet(`/comments/${user.id}`) 
        .chain(comments => updateDOM(user, comments))) // Task(DOM)

## 17-egghead-javascript-currying-with-examples.mp4

    ex: inc 
      const add = (x, y) => x + y
      const inc = y => add(1, y)
      const res = inc(2)
      console.log(res)
      // 3
    ex: use currying to define inc
      const add = x => y => x + y
      const inc = add(1) // y => 1 + y
      const res = inc(2)
      console.log(res)
      // 3
    ex: isOdd
      const modulo = dvr => dvd => dvd % dvr
      const isOdd = modulo(2)
      const res = isOdd(21)
      console.log(res)
      // 1
    ex: filter
      const filter = pred => xs => xs.filter(pred)
      const getAllOdds = filter(isOdd) // xs => xs.filter(isOdd)
      const res = getAllOdds([1,2,3,4])
      console.log(res)
      // 1 3
    we put pred first xs (data) last
      because data comes at last
    ex: replace and censor
      const replace = regex => repl => str =>
        str.replace(regex, repl)
      const censor = replace(/[aeiou]/ig)('*')
      const censorAll = map(censor)
      const res = censorAll(['hello world'])
      console.log(res)
      // h*ll* w*rld

## 18-egghead-angular-1-x-applicative-functors-for-multiple-arguments.mp4

    ex: problem
      const res = Box(x => x + 1).ap(Box(2)) // Box(3)
      console.log(res)
      // Box(3)
    define ap
    code: Box ap
      const Box = x =>
      ({
        ap: b2 => b2.map(x),
        chain: f => f(x),
        map: f => Box(f(x)),
        fold: f => f(x),
        inspect: () => `Box(${x})`
      })
    @mine: ap is dual of map
      170308.02.jpg
      let 
        inc = x => x + 1
        a = Box(2).map(inc)
        b = Box(inc).ap(Box(2))
        a == b
      eval
        a = Box( inc(2) ) // evald map
          = Box( 2 + 1 )
        b = Box(2).map(inc) // evald ap
          = a
    @mine: why is ap dual of map?
      Box = x => ({
        map: b1 => Box(b1(x)),
        ap: b2 => b2.map(x)
      })
      b1.ap(b2) = b2.map(b1) // in fact b1.x
        = Box(b1(b2.x))
      note:
        b1.ap(b2) = Box( b1(b2.x) )
        b2.map(b1) = Box( b1(b2.x) )
    ex:
      const res = Box(x => y => x + y).ap(Box(2)) // Box(y => 2 + y)
      const res = Box(x => y => x + y).ap(Box(2)).ap(Box(3)) // Box(5)
    ex: use add
      const add = x => y => x + y
      const res = Box(add).ap(Box(2)).ap(Box(3)) // Box(5)
      # this allows multiple args for functors
    note: map only allows single arg for function application
      but ap with currying allows multiple args 
      Box(x).map(f)
    code: ap law
      F(x).map(f) == F(f).ap(F(x))
      fx.map(f) == ff.ap(fx) // fx = F(x), ff = F(f)
    code: liftA2: helper function 
      # lift applicative with 2 args
      # f: function
      # fx, fy: functors
      const liftA2 = (f, fx, fy) =>
        F(f).ap(fx).ap(fy)
      # but we don't know F here
      # use ap law
      const liftA2 = (f, fx, fy) =>
        fx.map(f).ap(fy)
      # we don't mention any Functor
    code: use liftA2 with add
      const res1 = liftA2(add, Box(2), Box(3)) // Box(5)
      const res2 = Box(add).ap(Box(2)).ap(Box(3)) // Box(5)
    @mine: her şey dual'ler üzerinden yürüyor

## 19-egghead-javascript-applying-applicatives-exhibit-a.mp4

    ex
      const $ = selector =>
        Either.of({selector, height: 10})
      const getScreenSize = (screen, head, foot) =>
        screen - (head.height + foot.height)
      $('header').chain(head =>
        $('footer').map(footer =>
          getScreenSize(800, foot, size)))
    ex: use ap
      const getScreenSize = screen => head => foot =>
        screen - (head.height + foot.height)
      const res = Either.of(getScreenSize(800))
        .ap($('header'))
        .ap($('footer'))
      console.log(res)
      // Right(780)
    ex: use liftA2
      const res = liftA2(getScreenSize(800), $('header'), $('footer'))

## 20-egghead-javascript-list-comprehensions-with-applicative-functors.mp4

    ex:
      for(x in xs) {
        for(y in ys) {
          for(z in zs) {
          }
        }
      }
    capture these loops with ap function
    ex:
      const res = List.of(x => x).ap(List([1,2,3]))
      console.log(res)
      // List [1,2,3]
    ex:
      const merch = () =>
        List.of(x => y => `${x}-${y}`)
        .ap(List(['tshirt', 'sweater']))
        .ap(List(['large', 'small']))
      const res = merch()
      console.log(res)
      // List ["tshirt-large", "tshirt-small", ..]
      # this runs two nested loops
    ex:
      const merch = () =>
        List.of(x => y => z => `${x}-${y}-${z}`)
        .ap(List(['tshirt', 'sweater']))
        .ap(List(['large', 'small']))
        .ap(List(['black', 'white']))
      const res = merch()
      console.log(res)
      // List ["tshirt-large-black", "tshirt-large-white", ..]
    this captures a pattern 
      list comprehension
      declarative nature of looping

## 21-egghead-javascript-applicatives-for-concurrent-actions.mp4

    ex
      const Db = ({
        find: id =>
          new Task((rej, res) =>
            setTimeout(() =>
              res({id: id, title: `Project ${id}`}), 100))
      })
      const reportHeader = (p1, p2) =>
        `Report: ${p1.title} compared to ${p2.title}`
      Db.find(20).chain(p1 =>
        Db.find(8).map(p2 =>
          reportHeader(p1, p2)))
      # this waits until first find finishes
      # then calls second find
    @mine: why first chain then map?
      Db.find(8).map(p2 => reportHeader(p1,p2))
      returns a Task(..) object
      it won't run until Db.find(20) Task finishes
    ex: use ap
      Task.of(p1 => p2 => reportHeader(p1, p2))
      .ap(Db.find(20))
      .ap(Db.find(8))
      .fork(console.error, console.log)
      // Report: Project 20 compared to Project 8
      # this time both calls happen concurrently

## 22-egghead-javascript-leapfrogging-types-with-traversable.mp4

    code
      const futurize = require('futurize').futurize(Task)
      # like Promise
      # returns a lazy Task
    ex:
      const readFile = futurize(fs.readFile)
      const files = ['box.js', 'config.json']
      const res = files.map(fn => readFile(fn, 'utf-8'))
      console.log(res)
      // Task { fork: .. } Task {..}
      # we have list of Tasks
    how do we fork each?
      we want to convert
        [Task] => Task([])
      we need traverse
    ex:
      const files = List(['box.js', 'config.json'])
      files.traverse(Task.of, fn => readFile(fn, 'utf-8'))
        .fork(console.error, console.log)

## 23-egghead-javascript-maintaining-structure-whilst-asyncing.mp4

    ex:
      const httpGet = (path, params) =>
        Task.of(`${path}: result`)
      Map({home: '/', about: '/about-us', blog: '/blog'})
      .map(route => httpGet(route, {}))
      // Map({home: Task(..)..
    we'd rather one Task on the outside
      Task(Map({home: '..'}
    ex: 
      Map({home: '/', about: '/about-us', blog: '/blog'})
      .traverse(Task.of, route => httpGet(route, {}))
      .fork(console.error, console.log)
      // Map { "home": "/ result", ...
    we can traverse inside any structure
    ex: 
      Map({home: ['/', '/home'], about: ['/about-us']})
      .traverse(Task.of, routes => 
        List(routes)
        .traverse(Task.of, route => httpGet(route, {}))
      .fork(console.error, console.log)

## 24-egghead-javascript-principled-type-conversions-with-natural-transformations.mp4

![img/ss-155.png](/images/ss-155.png)

    what is natural transformation
      type conversion
        from one functor to another
      F a -> G a
    ex
      const eitherToTask = e =>
        e.fold(Task.rejected, Task.of)
      eitherToTask(Right("nightingale"))
        .fork(e => console.error('err', e),
          r => console.log('res', r))
      // res nightingale
      eitherToTask(Left("nightingale"))
        .fork(e => console.error('err', e),
          r => console.log('res', r))
      // err nightingale
    ex:
      const boxToEither = b =>
        b.fold(Right)
      const res = boxToEither(Box(100))
      console.log(res)
      // Right(100)
    why did we choose Right?
      Left will violate laws of natural transformation
    code: laws of natural transformation
      nt(x).map(f) == nt(x.map(f))
    ex: laws of natural transformation
      const res1 = boxToEither(Box(100)).map(x => x * 2)
      const res2 = boxToEither(Box(100).map(x => x * 2))
      console.log(res1, res2)
      // Right(200) Right(200)
    ex: what would happen with Left
      const boxToEither = b =>
        b.fold(Left)
      const res1 = boxToEither(Box(100)).map(x => x * 2)
      const res2 = boxToEither(Box(100).map(x => x * 2))
      console.log(res1, res2)
      // Left(100) Left(200)
    any function that satisfies law is a natural transformation
    ex:
      const first = xs =>
        fromNullable(xs[0])
      const res1 = first([1,2,3]).map(x => x + 1)
      const res2 = first([1,2,3].map(x => x + 1))
      console.log(res1, res2)
      // Right(2) Right(2)
      # so first is a nt too
    /Users/mertnuhoglu/Dropbox/public/img/ss-155.png

## 25-egghead-javascript-applying-natural-transformations-in-everyday-work.mp4

    when do we use nt?
    ex:
      ['hello', 'world']
        .chain(x => x.split(''))
      # we can't do this because chain doesn't exist on array
    ex: nt to array
      const res = List(['hello', 'world'])
        .chain(x => List(x.split('')))
      console.log(res)
      // List [ h e l l ...
    ex:
      const first = xs => fromNullable(xs[0])
      const largeNumbers = xs => xs.filter(x => x > 100)
      const larger = x => x * 2
      const app = xs =>
        first(largeNumbers(xs)).map(larger)
      console.log(app([2,400,50]))
      // Right 800
    ex: deeply nested boxes
      const fake = id =>
        ({id: id, name: 'user1', best_friend_id: id + 1})
      const Db = ({
        find: id =>
          new Task((rej, res) =>
            res(id > 2 ? Right(fake(id)) : Left('not found')))
      })
      const eitherToTask = e => e.fold(Task.rejected, Task.of)
      Db.find(3) // Task(Right(user))
        .chain(either =>
          either.map(user => Db.find(user.best_friend_id)))  // Right(Task(Right(User)))
      -->
      Db.find(3) // Task(Right(user))
        .chain(eitherToTask)   // Task(user)
        .chain(user =>
          Db.find(user.best_friend_id))  // Task(Right(User))
        .chain(eitherToTask)   // Task(user)
        .fork(e => console.error(e),
          r => console.log(r))
      // { id:4, best_friend_id: 5 }
      Db.find(2) // Task(Right(user))
        .chain(eitherToTask)   // Task(user)
        .chain(user =>
          Db.find(user.best_friend_id))  // Task(Right(User))
        .chain(eitherToTask)   // Task(user)
        .fork(e => console.error(e),
          r => console.log(r))
      // not found

## 26-egghead-javascript-isomorphisms-and-round-trip-data-transformations.mp4

    what is isomorphism
      a pair functions: to, from
      from(to(x)) == x
      to(from(y)) == y
      these functions prove a data type holds the same information as another data type
      String ~ [Char]
    code:
      const Iso = (to, from) => ({
        to,
        from
      })
      const chars = Iso(s => s.split(''), c => c.join(''))
      const res = chars.from(chars.to('hello world'))
      console.log(res)
      // hello world
    why is this useful
    ex: truncate
      const truncate = str =>
        chars.from(chars.to(str).slice(0, 3)).concat('...')
      const res = truncate('hello world')
      console.log(res)
      // hel...
    ex: [a] ~ Either null a
      const singleton = Iso(
        e => e.fold(() => [], x => [x]),
        ([x]) => x ? Right(x) : Left())
      const filterEither = (e, pred) =>
        singleton.from(singleton.to(e).filter(pred))
      const res = filterEither( Right('hello'), x => x.match(/h/ig) )
        .map(x => x.toUpperCase())
      console.log(res)
      // Right(HELLO)
      const res = filterEither( Right('ello'), x => x.match(/h/ig) )
        .map(x => x.toUpperCase())
      console.log(res)
      // Left()

## 27-egghead-javascript-real-world-example-pt1.mp4

    ex:
      node index.js oasis blue
    ex:
      npm init
      npm install data.either data.task fantasy-identities request --save
    ex:
      console.log(process.argv)
        $ node index.js oasis blur
        [ 'node', 'index.js', oasis, blur ]
    ex: index.js
      const Task = require('data.task')
      const Spotify = require('./spotify')
      const argv = new Task((rej, res) => res(process.argv))
      const names = argv.map(args => args.slice(2))
      const related = name =>
        findArtist(name)
        .map(artist => artist.id)
        .chain(relatedArtists)
      const main = ([name1, name2]) =>
        Task.of(rels1 => rels2 => [rels1, rels2])
        .ap( related(name1) )
        .ap( related(name2) )
      names.chain(main).fork(console.error, console.log)
    ex:
      spotify.js
        "https://api.spotify.com/v1/search?q=${query}&type=artist" // artists: {items: []}}
        "https://api.spotify.com/v1/artists/${id}/related-artists" // artists: []
        const request = require('request')
        const Task = require('data.task')
        const Either = require('data.either')
        const httpGet = url =>
          new Task((rej, res) =>
          request(url, (error, response, body) =>
            error ? rej(error) : res(body)))
        const first = xs =>
          Either.fromNullable(xs[0])
        const eitherToTask = e =>
          e.fold(Task.rejected, Task.of)
        const findArtist = name =>
          httpGet(`https://api.spotify.com/v1/search?q=${query}&type=artist`)
          .map(result => {
            console.log(result)
            return result
          })
          .map(result => result.artists.items)
          .map(first)
          .chain(eitherToTask)
        const relatedArtists = id =>
          httpGet(`https://api.spotify.com/v1/artists/${id}/related-artists`)
          .map(result => result.artists)
        module.exports = {findArtist, relatedArtists}
    ex: parse json data
      const parse = Either.try(JSON.parse)
      const findArtist = name =>
        httpGet(`https://api.spotify.com/v1/search?q=${query}&type=artist`)
        .map(parse)
        .map(result => result.artists.items)
        .map(first)
        .chain(eitherToTask)
      const relatedArtists = id =>
        httpGet(`https://api.spotify.com/v1/artists/${id}/related-artists`)
        .map(parse)
        .map(result => result.artists)
    ex: httpGet(url).map(parse) encapsulated
      const getJSON = url =>
        httpGet(url)
        .map(parse)
        .chain(eitherToTask)
      const parse = Either.try(JSON.parse)
      const findArtist = name =>
        getJSON(`https://api.spotify.com/v1/search?q=${query}&type=artist`)
        .map(result => result.artists.items)
        .map(first)
        .chain(eitherToTask)
      const relatedArtists = id =>
        getJSON(`https://api.spotify.com/v1/artists/${id}/related-artists`)
        .map(result => result.artists)
    ex: print related artist names only
      const related = name =>
        findArtist(name)
        .map(artist => artist.id)
        .chain(relatedArtists)
        .map(artists => artists.map(artist => artist.name))

## 28-egghead-javascript-real-world-example-pt2.mp4

    find set intersection of two lists of related artists
    ex: index.js
      const Intersection = xs => ({
        xs, 
        concat: ({xs: ys}) =>
          Intersection( xs.filter(x => ys.some(y => x === y)) )
      })
      const artistIntersection = rels1 => rels2 =>
        Intersection(rels1).concat(Intersection(rels2)).xs
      const main = ([name1, name2]) =>
        Task.of(artistIntersection)
        .ap( related(name1) )
        .ap( related(name2) )
    take n args instead of 2
    ex: index.js
      const {List} = require('immutable-ext')
      const artistIntersection = rels =>
        rels.foldMap(Intersection).xs
      const main = names =>
        List(names)
        .traverse(Task.of, related)
        .map(artistIntersection)
      >> node index.js oasis blur radiohead
      [ 'Pulp' ]
    ex:
      const {Pair, Sum} = require('./monoid')
      const artistIntersection = rels =>
        rels.foldMap( x => Pair(Intersection(x), Sum(x.length)))
        .toList()
      >> node index.js oasis blur radiohead
      [ 'Pulp', concat: [Function], 60 ]
    ex:
      const artistIntersection = rels =>
        rels.foldMap( x => Pair(Intersection(x), Sum(x.length)))
        .bimap(x => x.xs, y => y.x)
        .toList()
      >> node index.js oasis blur radiohead
      [ 'Pulp', 60 ]

