---
title: "Notes from Training: RxJs CycleJs Reactive Trainings Egghead - Andre Staltz"
date: 2017-11-24T10:14:58+03:00
draft: false
description: ""
tags: ["book_notes", "js", "functional_programming"]
categories: ["software", "js", "functional_programming"]
type: post
url:
---

My study notes from the video training.

<!--more-->

## Introduction to Reactive Programming

    ref
      https://egghead.io/courses/introduction-to-reactive-programming
      https://gist.github.com/staltz/868e7e9bc2a7b8c1f754
      http://jsfiddle.net/staltz/8jFJH/48/
    01
      what is it?
        programming with event streams
          sequence of events happening over time (async)
          when an event happens, we react to it
        another sequence: array 
          sequence in space
      ex
        var source = Rx.Observable.interval(400).take(9)
          .map(i => ['1', '2' ..][i])
        var result = source
        result.subscribe(x => console.log(x))
      ex: sum all numbers
        var result = source
          .map(x => parseInt(x))
          .filter(x => !isNaN(x))
          .reduce((x,y) => x + y)
    02
      ex: double clicks of a button
        var button = document.querySelector('.button')
        var label = document.querySelector('h4')
      what if no rxjs?
        var counter = 0
        button.addEventListener()
        setTimeout(..)
      ex: rx approach
        var click_ = Rx.Observable.fromEvent(button, 'click')
        var doubleClick_ = click_
          .buffer(() => click_.throttle(250))
          .map(arr => arr.length)
          .filter(len => len === 2)
        doubleClick_.subscribe(e => {
          label.textContent = 'double click'
        })
        doubleClick_
          .throttle(1000)
          .subscribe(suggestion => {
            label.textContent = '-'
          })
    03 why use rxjs
      it allows you 
        to specify dynamic behavior of a value
        completely at the time of declaration
      ex
        var a = 3
        var b = 10 * a
        console.log(b) // 30
        a = 4
        console.log(b) // still 30
        b = 11 * a
        console.log(b) // 44
      b is not constant
        var b declaration doesn't specify dynamic behavior
        b is always 10 * a
        when a changes, b should change too
      ex
        var a_ = Rx.Observable.of(3) 
        var b_ = a_.map(a => 10 * a)
        b_.subscribe(b => console.log(b))
      now change a:
        how?
          a_.set(4)
        this is wrong
          a is dynamic as well
          we should specify it at declaration of a
      ex
        var a_ = Rx.Observable.of(3, 4) 
        var b_ = a_.map(a => 10 * a)
        b_.subscribe(b => console.log(b)) // 30 40
    04: github follow box
      ex
        var req_ = Rx.Observable.just('https://api.github.com/users')
      opt1: use jquery getJSON promise
        req_.subscribe(url => {
          jQuery.getJSON(url)
            .done(res => {
              console.log(res)
      opt2: use stream
        req_.subscribe(url => {
          var res_ = Rx.Observable.fromPromise(
            jQuery.getJSON(url))
          res_.subscribe(res => {
            console.log(res)
          })
        })
      observable does what promise does
        promise is simplified version of observable
      what if mapping request to response
      ex: map to observable
        var res_ = req_
          .map(url =>
            Rx.Observable.fromPromise(jQuery.getJSON(url))
          )
      we are mapping stream to something that happens later in time
        Observable of Observable
      ex: use flatMap instead
        var res_ = req_
          .flatMap(url =>
            Rx.Observable.fromPromise(jQuery.getJSON(url))
          )
      ex: output effect
        res_.subscribe( res => {
          console.log(res)
        })
    05
      render that data on the dom
      html
        div .container
          ul .suggestions
            li .suggestion1
              img
              a href .username 
                text
      we have res_
        array of users
        we need 3 of them
      ex
        var suggestion1_ = res_.map(listUser =>
          listUser[Math.floor(Math.random().listUser.length)]
        )
        var suggestion2_ = res_.map(listUser =>
          listUser[Math.floor(Math.random().listUser.length)]
        )
      ex: put this into a function
        function createSuggestion_(res_) {
          return res_.map(listUser =>
            listUser[Math.floor(Math.random().listUser.length)]
          )
        }
        var suggestion1_ = createSuggestion_(res_)
        var suggestion2_ = createSuggestion_(res_)
        var suggestion3_ = createSuggestion_(res_)
        function renderSuggestion(userData, selector) {
          var element = document.querySelector(selector)
          var usernameEl = element.querySelector('.username')
          usernameEl.href = userData.html_url
          usernameEl.textContent = userData.login
          var imgEl = element.querySelector('img')
          imgEl.src = userData.avatar_url
        }
        suggestion1_.subscribe(user => {
          renderSuggestion(user, '.suggestion1')
        })
        suggestion2_.subscribe(user => {
          renderSuggestion(user, '.suggestion1')
        })
        suggestion3_.subscribe(user => {
          renderSuggestion(user, '.suggestion1')
        })
    06: refresh button, get more users
      ex
        var refreshButton = document.querySelector('.refresh')
        refreshClick_ = Rx.Observable.fromEvent(refreshButton, 'click')
      each click is an api request
      ex
        var req_ = refreshClick_
          .map(ev => {
            var randomOffset = Math.floor(Math.random()*500);
            return 'https://api.github.com/users?since=';
          })
      but if i don't click refresh then, i will never get anything at all
      ex: start request
        var startupReq_ = Rx.Observable.just('https://api.github.com/users')
        var res_ = req_
          .merge(startupReq_)
          .flatMap(..)
      rxmarble
        ---a---b-->
        s--------->
        merge
        s--a--b--->
    07: bug: after refresh it doesn't clear immediately
      start with null
        prepend null
      ex
        function createSuggestion_(res_) {
          return res_.map(listUser =>
            listUser[Math.floor(Math.random().listUser.length)]
          ).startWith(null)
      but at each refresh, we should set null again
      ex
        function createSuggestion_(res_) {
          return res_.map(listUser =>
            listUser[Math.floor(Math.random().listUser.length)]
          )
          .startWith(null)
          .merge(refreshClick_.map(ev => null))
      rxmarble
        ----u--------u->
          startWith(N)
        N---u--------u->
        ---------N----->
          merge
        N---u----N---u->
    08: 3 different request
      how to make it one request?
        var suggestion1_ = createSuggestion_(res_)
        var suggestion2_ = createSuggestion_(res_)
        var suggestion3_ = createSuggestion_(res_)
      ex: shareReplay
        var res_ = req_
          .merge(startupReq_)
          .flatMap(..)
          .shareReplay(1)
      böylece yeni request yapmayacak, mevcudu tekrar kullanacak
    09: replace user
      ex
        var closeButton1 = document.querySelector('.close1')
        var close1Clicks = Rx.Observable.fromEvent(closeButton1, 'click')
      marble
        refreshClick_  -----f--------->
        req_           r----r--------->
        res_           --R-----R------>
        suggestion1_   N-u--N--u------>
      closing nasıl dahil olacak?
        refreshClick_  -----f--------->
        req_           r----r--------->
        res_           --R-----R------>
        closeClick_    -----------x--->
        suggestion1_   N-u--N--u--u--->
      ex
        function createSuggestion_(res_, closeClick_) {
          return res_.map(getRandomUser )
          .startWith(null)
          .merge(refreshClick_.map(ev => null))
          .merge(
            closeClick_.withLatestFrom(res_, 
              (x,listUsers) => getRandomUser(listUsers)
          )
    10: overview
    marble
      refreshClickStream
        refreshButton
      requestStream
        refreshClickStream
      responseStream
        requestStream
      closeClickStream
        closeButton1
      createSuggestionStream
        closeClickStream
        responseStream
        refreshClickStream
    code
      var refreshButton = document.querySelector('.refresh');
      var closeButton1 = document.querySelector('.close1');
      var closeButton2 = document.querySelector('.close2');
      var closeButton3 = document.querySelector('.close3');
      var refreshClickStream = Rx.Observable.fromEvent(refreshButton, 'click');
      var close1ClickStream = Rx.Observable.fromEvent(closeButton1, 'click');
      var close2ClickStream = Rx.Observable.fromEvent(closeButton2, 'click');
      var close3ClickStream = Rx.Observable.fromEvent(closeButton3, 'click');
      var requestStream = refreshClickStream.startWith('startup click')
          .map(function() {
              var randomOffset = Math.floor(Math.random()*500);
              return 'https://api.github.com/users?since=' + randomOffset;
          });
      var responseStream = requestStream
          .flatMap(function (requestUrl) {
              return Rx.Observable.fromPromise($.getJSON(requestUrl));
          });
      function createSuggestionStream(closeClickStream) {
          return closeClickStream.startWith('startup click')
              .combineLatest(responseStream,             
                  function(click, listUsers) {
                      return listUsers[Math.floor(Math.random()*listUsers.length)];
                  }
              )
              .merge(
                  refreshClickStream.map(function(){ 
                      return null;
                  })
              )
              .startWith(null);
      }
      var suggestion1Stream = createSuggestionStream(close1ClickStream);
      var suggestion2Stream = createSuggestionStream(close2ClickStream);
      var suggestion3Stream = createSuggestionStream(close3ClickStream);
      // Rendering ---------------------------------------------------
      function renderSuggestion(suggestedUser, selector) {
          var suggestionEl = document.querySelector(selector);
          if (suggestedUser === null) {
              suggestionEl.style.visibility = 'hidden';
          } else {
              suggestionEl.style.visibility = 'visible';
              var usernameEl = suggestionEl.querySelector('.username');
              usernameEl.href = suggestedUser.html_url;
              usernameEl.textContent = suggestedUser.login;
              var imgEl = suggestionEl.querySelector('img');
              imgEl.src = "";
              imgEl.src = suggestedUser.avatar_url;
          }
      }
      suggestion1Stream.subscribe(function (suggestedUser) {
          renderSuggestion(suggestedUser, '.suggestion1');
      });
      suggestion2Stream.subscribe(function (suggestedUser) {
          renderSuggestion(suggestedUser, '.suggestion2');
      });
      suggestion3Stream.subscribe(function (suggestedUser) {
          renderSuggestion(suggestedUser, '.suggestion3');
      });

## Cycle.js Fundamentals

    https://egghead.io/courses/cycle-js-fundamentals
    01
      ex
        Rx.Observable.timer(0, 1000) // 0--1--2--3
          .map(i => `seconds elapsed ${i}`)
          .subscribe(text => {
            const container = document. querySelector('#app')
            container.textContent = text
          })
      principle:
        separate logic from effects
      logic above: (functional)
        timer(..)
          .map()
      effect: (imperative)
        subscribe(..)
      goal: 
        put subscribe away 
        programmer writes logic
    02
      ex
        function main() {
          return Rx.Observable.timer(0, 1000) // 0--1--2--3
            .map(i => `seconds elapsed ${i}`)
        }
        function DOMEffect(text$) {
          text$.subscribe(text => {
            const container = document. querySelector('#app')
            container.textContent = text
          })
        }
        DOMEffect(main())
      ex: let's put log effect too
        function consoleLogEffect(msg$) {
          msg$.subscribe(msg => console.log(msg))
        }
      how to use it?
        const sink = main()
        DOMEffect(sink)
        consoleLogEffect(sink)
    03: different sinks for different effects
      ex
        function main() {
          return {
            DOM: Rx.Observable.timer(0, 1000) // 0--1--2--3
              .map(i => `seconds elapsed ${i}`)
            Log: Rx.Observable.timer(0, 2000).map(i=> 2 * i)
          }
        }
        const sinks = main()
        DOMEffect(sinks.DOM)
        consoleLogEffect(sinks.Log)
      this is how we start to control
        which sink goes into which effect
    04: run and driver
      we have logic: main()
      we have effects: DOMEffect, consoleLogEffect
      what about this part:
        const sinks = main()
        DOMEffect(sinks.DOM)
        consoleLogEffect(sinks.Log)
      this ties logic with effects
        put it into run()
      ex
        function run(mainFn) {
          const sinks = mainFn()
          DOMEffect(sinks.DOM)
          consoleLogEffect(sinks.Log)
        }
        run(main)
      now: i don't want to run some effects anymore
        we have hardcoded effects into run
        i want to pass effects to run
      ex
        function run(mainFn, effects) {
          const sinks = mainFn()
          Object.keys(effects).forEach(key => {
            effects[key](sinks[key])
          })
        }
        const effectsFn = {
          DOM: DOMEffect,
          Log: consoleLogEffect
        }
        run(main, effectsFn)
      now: rename effectsFn to drivers
        driver: interface between software and hardware
          think: hardware are effects, software is logic
      ex
        const drivers = { 
          DOM: DOMDriver,
          Log: consoleLogDriver
        }
    05: read effects
      we have main() and drivers
        drivers responsible of output effects
        what about input/read effects?
          main() needs to take them in
          drivers() don't return anything
      drivers need to return something
        in order to make read effects
      code
        function DOMDriver(text$) {
          text$.subscribe(text => {
            const container = document. querySelector('#app')
            container.textContent = text
          })
          .. make DOMSource
          return DOMSource;
        }
        function main(DOMSource) {
          return {
            DOM: Rx.Observable.timer(0, 1000) // 0--1--2--3
              .map(i => `seconds elapsed ${i}`)
            Log: Rx.Observable.timer(0, 2000).map(i=> 2 * i)
          }
        }
      where does sink/source come from?
        data flow network terminology
        source = input = read effects
        sink = output = write effects
      create read effects in DOM
        function DOMDriver(text$) {
          text$.subscribe(text => {
            const container = document. querySelector('#app')
            container.textContent = text
          })
          const DOMSource = Rx.Observable.fromEvent(document, 'click')
          return DOMSource;
        }
      how to give it to main()?
        function run(mainFn, drivers) {
          const sinks = mainFn(DOMSource)
          const DOMSource = drivers.DOM(sinks.DOM)
        problem:
          a = f(b)
          b = g(a)
        solution
          bProxy = ...
          a = f(bProxy)
          b = g(a)
          bProxy.imitate(b)
        code
          function run(mainFn, drivers) {
            const proxyDOMSource = new Rx.Subject()
            const sinks = mainFn(proxyDOMSource)
            const DOMSource = drivers.DOM(sinks.DOM)
            DOMSource.subscribe(click => proxyDOMSource.onNext(click))
    06: generalizing run
      this main function only takes DOM input
        we need to receive all types of sources
      code
        function main(sources) {
          const click$ = sources.DOM
          const sinks = {
            DOM: click$
              .startWith(null)
              .flatMapLatest(() =>
                Rx.Observable.timer(0, 1000) // 0--1--2--3
                  .map(i => `seconds elapsed ${i}`)
                ),
            Log: Rx.Observable.timer(0, 2000).map(i => 2 * i),
          }
          return sinks
        }
        function run(mainFn, drivers) {
          const proxySources = {}
          Object.keys(drivers).forEach(key => {
            proxySources[key] = new Rx.Subject()
          })
          const sinks = mainFn(proxySources)
          Object.keys(drivers).forEach(key => {
            const source = drivers[key](sinks[key])
            source.subscribe(x => proxySources[key]).onNext(x)
          })
        }
      cyclejs makes run() a library api
        Cycle.run(main, drivers)
    07
      main:
        .map(i => `seconds elapsed ${i}`)
        -->
        .map(i => {
          return {
            tagName: 'H1',
            children: [
              `seconds elapsed ${i}`
            ]
          }
        })
      DOMDriver:
        function DOMDriver(text$) {
          text$.subscribe(text => {
            const container = document. querySelector('#app')
            container.textContent = text
          })
        -->
        function DOMDriver(obj$) {
          function createElement(obj) {
            const element = document.createElement(obj.tagName)
            element.innerHTML = obj.children[0]
          }
          obj$.subscribe(obj => {
            const container = document. querySelector('#app')
            container.innerHTML = ''
            const element = createElement(obj)
            container.appendChild(element)
          })
      what if the DOM output is a composite object?
        main
          .map(i => {
            return {
              tagName: 'H1',
              children: [
                `seconds elapsed ${i}`
              ]
            }
          })
          -->
          .map(i => {
            return {
              tagName: 'H1',
              children: [
                {
                  tagName: 'SPAN',
                  children: [
                    `seconds elapsed ${i}`
                  ]
                }
              ]
            }
          })
        DOMDriver
          function createElement(obj) {
            const element = document.createElement(obj.tagName)
            element.innerHTML = obj.children[0]
          }
          -->
          function createElement(obj) {
            const element = document.createElement(obj.tagName)
            obj.children
              .filter(c => typeof c === 'object')
              .map(createElement)
              .forEach(c => element.appendChild(c))
            obj.children
              .filter(c => typeof c === 'string')
              .forEach(c => element.innerHTML += c)
            return element
          }
    08
      what if you want to restart everytime you hover instead of click
        current:
          const click$ = sources.DOM
        we look for
          const click$ = sources.DOM.selectEvents('span', 'mouseover')
        we should move this logic out of DOMDriver
          const DOMSource = Rx.Observable.fromEvent(document, 'click')
          -->
          const DOMSource = {
            selectEvents: function(tagName, eventType) {
              return Rx.Observable.fromEvent(document, eventType).
                filter(ev => ev.target.tagName === tagName.toUppercase())
            }
          }
        main
          const click$ = sources.DOM
          -->
          const mouseover$ = sources.DOM.selectEvents('span', 'mouseover')
          const sinks = {
            DOM: mouseover$
              ...
    09: hyperscript
      function h(tagName, children) {
        tagName: tagName,
        children: children,
      }
      main:
        return {
          tagName: 'H1',
          children: [
            {
              tagName: 'SPAN',
              children: [
                `seconds elapsed ${i}`
        -->
        .map(i =>
          h('H1', [
            h('SPAN', [
              `seconds elapsed ${i}`
            ])
          ])
      make specific h functions
        function h1(children) {
          return { 
            tagname: 'H1', 
            children: children,
          }
        h1([
          span([
            `seconds elapsed ${i}`
    10: toy dom driver to real
      problem1: #app is hardcoded currently
        make it parametric
        code
          const container = document. querySelector('#app')
          -->
          function makeDOMDriver(mountSelector) {
            return function DOMDriver(..) {
              ..
              const container = document. querySelector(mountSelector)
        using
          DOM: makeDOMDriver('#app')
      problem2: 
        we clear container.innerHTML everytime we get new obj event
        bad for performance
        code
          obj$.subscribe(obj => {
            const container = document. querySelector('#app')
            container.innerHTML = ''
      problem3:
        selectEvents is very constrained
          selectEvents: function(tagName, eventType) {
            return Rx.Observable.fromEvent(document, eventType).
        make it more generic
      solution
        cycle-dom.js
        const {h, h1, span, makeDOMDriver} = CycleDOM; 
        # delete makeDOMDriver
        use select
          const mouseover$ = sources.DOM.selectEvents('span', 'mouseover')
          -->
          const mouseover$ = sources.DOM.select('span').events('mouseover')
        # uses virtual dom: h1, span
      features
        h1({style: {background:'red'}}, [..])
    11
      write effects: sinks in main
      read effects: sources in main
      ex: hello world text input element
      code - basic
        const {label, input, h1, hr, div, makeDOMDriver } = CycleDOM
        function main(sources) {
          const inputEv$ = sources.DOM.select('.field').events('input')
          const name$ = inputEv$.map( ev => ev.target.value).startWith('')
          return { 
            DOM: name$.map( name =>
              div([
                label('Name:'),
                input('.field', {type: 'text'}),
                hr(),
                h1(`Hello ${name}`)
              ])
            )
          }
        }
        const drivers = {
          DOM: makeDOMDriver('#app'), 
        }
        Cycle.run(main, drivers)
    12: interactive counter
      code
        const {button, p, label, div, makeDOMDriver} = CycleDOM
        function main(sources) { 
          const decrementClick$ = sources.DOM.select('.decrement').events('click')
          const incrementClick$ = sources.DOM.select('.increment').events('click')
          const decrementAction$ = decrementClick$.map(ev => -1)
          const incrementAction$ = incrementClick$.map(ev => +1)
          const number$ = Rx.Observable.of(10)
            .merge(decrementAction$)
            .merge(incrementAction$)
            .scan( (prev, curr) => prev + curr)
          return {
            DOM:  number$( number =>
              div([
                button('#decrement', 'Decrement'),
                button('.increment', 'Increment'),
                p([
                  label(String(number))
                ])
              ])
            )
          }
        }
      opt1: merging naively
        const number$ = Rx.Observable.of(10)
          .merge(decrementAction$)
          .merge(incrementAction$)
        marble
          10--------------
          ----(-1)--(-1)--
          10--(-1)--(-1)--
        goal
          10--(-1)--(-1)--
          -->
          10--9--8--
      this is how you keep state
    13: using http driver to make http request
      code
        const {makeHTTPDriver} = CycleHTTPDriver
        const drivers = {
          DOM: ..
          HTTP: makeHTTPDriver(),
      ex: use jsonplaceholder api
      we know
        read effects = source
        write effects = sinks
      code
        // DOM read effect: button clicked
        // http write effect: request sent
        // http read effect: response received
        // DOM write effect: data displayed
        function main(sources) { 
          const clickEvent$ = sources.DOM.select('.get-first').events('click')
          const request$ = clickEvent$.map(() => {
            return {
              url: 'http://jsonplaceholder.typicode.com/users/1',
              method: 'GET',
            }
          })
          const response$$ = sources.HTTP
            .filter(response$ => response$.request.url === 'http://jsonplaceholder.typicode.com/users/1')
          const response$ = response$$.switch() // flattens
          const firstUser$ = response$.map(response => response.body)
            .startWith(null)
          return {
            DOM: firstUser$.map(
              div([
                button('.get-first', 'Get first user'),
                firstUser === null ? null : div('.user-details', [
                  h1('.user-name', 'firstUser.name'),
                  h4('.user-email', 'firstUser.emaail'),
                  a('.user-website', {href: 'firstUser.website'}, 'firstUser.website')
                ])
              ])
            HTTP: request$,
          }
      why const response$$ = sources.HTTP
        stream that emits response streams
        marble
          r: request
          -------r--------------r------
                 \____a_         \___b
          each branch is a response stream
    14: bmi calculator
      code
        // DOM read effect: detect slider change
        // recalculate BMI
        // DOM write effect: display BMI
        function main(sources) {
          const changeWeight$ = sources.DOM.select('.weight').events('input')
            .map(ev => ev.target.value)
          const changeHeight$ = sources.DOM.select('.height').events('input')
            .map(ev => ev.target.value)
          const state$ = Rx.Observable.combineLatest(
            changeWeight$.startWith(70),
            changeHeight$.startWith(170),
            (weight, height) => {
              const heightMeters = height * 0.01;
              const bmi = Math.round(weight / heightMeters * heightMeters));
              return {bmi, weight, height};
            }
          return {
            DOM: state$.map( state =>
              div([
                div([
                  label('Weight: ' + state.weight + ' kg'),
                  input('.weight', {type: 'range', min: 40, max: 150, value: 70})
                ]),
                div([
                  label('Height: ' + state.height + 'cm'),
                  input('.height', {type: 'range', min: 140, max: 220, value: 170})
                ]),
                h2('BMI is ' + state.bmi)
    15: mvi pattern
      main function grows
        3 parts:
          read effects
          logic
          write effects
        code
          function part1(DOMSource) {
            const changeWeight$ = DOMSource.select('.weight').events('input')
              .map(ev => ev.target.value)
            const changeHeight$ = DOMSource.select('.height').events('input')
              .map(ev => ev.target.value)
          }
          function part2(changeWeight$, changeHeight$) {
            const state$ = Rx.Observable.combineLatest(
              changeWeight$.startWith(70),
              changeHeight$.startWith(170),
              (weight, height) => {
                const heightMeters = height * 0.01;
                const bmi = Math.round(weight / heightMeters * heightMeters));
                return {bmi, weight, height};
              }
          }
          function part3(state$) {
            return {
              DOM: state$.map( state =>
                div([
                  div([
                    label('Weight: ' + state.weight + ' kg'),
                    input('.weight', {type: 'range', min: 40, max: 150, value: 70})
                  ]),
                  div([
                    label('Height: ' + state.height + 'cm'),
                    input('.height', {type: 'range', min: 140, max: 220, value: 170})
                  ]),
                  h2('BMI is ' + state.bmi)
            }
          }
          function main(sources) {
            const {changeWeight$, changeHeight$} = part1(sources.DOM) const state$ = part2(changeWeight$, changeHeight$)
            const vtree$ = part3(state$)
            return {
              DOM: vtree$
            }
        rename
          part3: view
          part2: model
          part1: intent
        code
          function main(sources) {
            const {changeWeight$, changeHeight$} = intent(sources.DOM) 
            const state$ = model(changeWeight$, changeHeight$)
            const vtree$ = view(state$)
            return {
              DOM: vtree$
            }
    16: component
      repeated code:
        const changeWeight$ = DOMSource.select('.weight').events('input')
          .map(ev => ev.target.value)
        const changeHeight$ = DOMSource.select('.height').events('input')
          .map(ev => ev.target.value)
      code 
        function main(sources) {
          const change$ = sources.DOM.select('.slider').events('input')
            .map(ev => ev.target.value)
          const value$ = change$.startWith(70)
          return {
            DOM: value$.map(value =>
              div('.labeled-slider', [
                label('.label', 'Weight: ' + value +kg'),
                input('.slider', {type: 'range', min: 40, max: 150, value})
              ])
            )
          }
          -->
          function intent(DOMSource) {
            return DOMSource.select('.slider').events('input')
              .map(ev => ev.target.value)
          }
          function model(change$, props$) {
            const initialValue$ = props$.map(props => props.init).first()
            const value$ = initialValue$.concat(change$)
            return Rx.Observable.combineLatest(value$, props$, (value, props) => {
              return {
                label: props.label,
                unit: props.unit,
                min: props.min,
                max: props.max,
                value: value,
              }
            }
          }
          function view(state$) {
            return state$.map(state =>
              div('.labeled-slider', [
                label('.label', `${state.label}: ${state.value${state.unit}`'Weight: ' + value +kg'),
                input('.slider', {type: 'range', min: state.min, max: state.max, value})
              ])
            )
          }
          function LabeledSlider(sources) {
            const change$ = intent(sources.DOM)
            const value$ = model(change$, sources.props)
            const vtree$ = view(value$)
            return {
              DOM: vtree$
            }
          }
          const drivers = {
            DOM: makeDOMDriver('#app'),
            props: () => Rx.Observable.of({
              label: 'Height',
              unit: 'cm',
              min: 140,
              max: 220,
              init: 170
            })
          }
    17: using component
      how can we reuse it?
        rename main
          LabeledSlider
        use LabeledSlider in a new main
          function main(sources) {
            return LabeledSlider(sources)
        move props into new main()
          function main(sources) {
            const props$ = Rx.Observable.of({
              label: 'Height',
              unit: 'cm',
              min: 140,
              max: 220,
              init: 170
            })
            return LabeledSlider({DOM: sources.DOM, props: props$})
    18: multiple instances of a component
      code
        function main(sources) {
          const weightProps$ = Rx.Observable.of({
            label: 'Weight',
            unit: 'kg',
            min: 40,
            max: 150,
            init: 70
          const weightSinks = LabeledSlider({DOM: sources.DOM, props: weightProps$})
          const heightProps$ = Rx.Observable.of({
            label: 'Height',
            unit: 'cm',
            min: 140,
            max: 220,
            init: 170
          })
          })
          const heightSinks = LabeledSlider({DOM: sources.DOM, props: heightProps$})
          const vtree$ = Rx.Observable.combineLatest(
            weightSinks.DOM, heightSinks.DOM, (weightVTree, heightVTree) =>
              div([
                weightVTree,
                heightVTree,
              ])
            )
      we need to separate DOM elements
        .slider
      code
        const weightSinks = LabeledSlider({DOM: sources.DOM, props: weightProps$})
        const weightVTree$ = weightSinks.DOM.map(vtree => {
          vtree.properties.className += ' weight'
          return vtree
        })
        ..
        const heightSinks = LabeledSlider({DOM: sources.DOM, props: heightProps$})
        const heightVTree$ = heightSinks.DOM.map(vtree => {
          vtree.properties.className += ' height'
          return vtree
        })
        const vtree$ = Rx.Observable.combineLatest(
          weightVTree$, heightVTree$, (weightVTree, heightVTree) =>
      now select correct component
        const weightSinks = LabeledSlider({DOM: sources.DOM, props: weightProps$})
        -->
        const weightSinks = LabeledSlider({DOM: sources.DOM.select('.weight'), props: weightProps$})
        const heightSinks = LabeledSlider({DOM: sources.DOM.select('.height'), props: heightProps$})
    19: isolating component instances
      code
        const isolate = CycleIsolate;
      replace
        const weightSinks = LabeledSlider({DOM: sources.DOM.select('.weight'), props: weightProps$})
        -->
        const WeightSlider = isolate(LabeledSlider, 'weight')
        const weightSinks = WeightSlider({DOM: sources.DOM, props: weightProps$})
      replace
        const weightVTree$ = weightSinks.DOM.map(vtree => {
          vtree.properties.className += ' weight'
          return vtree
        })
        -->
        const weightVTree$ = weightSinks.DOM
      what isolate does?
        it does preprocessing and postprocessing
      code
        const IsolateLabeledSlider = function (sources) {
          return isolate(LabeledSlider(sources))
        }
    20: exporting values from components through sinks
      code
        const bmi$ = Rx.Observable.combineLatest(weightValue$, heightValue$, 
          (weight, height) => {
          }
        const vtree$ = Rx.Observable.combineLatest(
          bmi$, weightVTree$, heightVTree$, (weightVTree, heightVTree) =>
            div([
              weightVTree,
              heightVTree,
              h2('BMI is ' + bmi)
      where do we get weightValue$
        from LabeledSlider
        but it doesn't export anything except DOM
        we need to get value stream out of state$
      code
        function LabeledSlider(sources) {
          const change$ = intent(sources.DOM)
          const value$ = model(change$, sources.props)
          const vtree$ = view(value$)
          return {
            DOM: vtree$
          }
        }
        -->
        function LabeledSlider(sources) {
          const change$ = intent(sources.DOM)
          const value$ = model(change$, sources.props)
          const vtree$ = view(value$)
          return {
            DOM: vtree$
            value: state$.map(state => state.value),
          }
        }
      use those values
        const weightValue$ = weightSinks.value
        const heightValue$ = heightSinks.value
        const bmi$ = Rx.Observable.combineLatest(weightValue$, heightValue$, 
          (weight, height) => {
            const heightMeters = height * 0.01
            const bmi = Math.round( weight / (heightMeters*heightMeters))
            return bmi
          }
      so, sinks mean
        write effect as vtree$
        or value outputs
    21: overview of cycle
      in real apps
        we don't use <script>
      next

## RxJS Beyond the Basics: Creating Observables from scratch

    https://egghead.io/courses/rxjs-beyond-the-basics-creating-observables-from-scratch
    01
      Observable type
      different ways to create
      two rxjs
        Reactive-Extensions
          v4
        ReactiveX
          younger
          v5
    02
      Rx.Observable
        not like EventEmitter 
        more like a generalization of a function
      ex
        function foo() {
          console.log('Hello')
          return 42
        }
        var x = foo()
        console.log(x)
        -->
        console.log(foo())
        -->
        console.log(foo.call())
      all computation inside foo is not run until it is called
      ex
        var bar = Rx.Observable.create(function (observer) {
          console.log('hello')
          observer.next(42)
        })
        bar.subscribe(function (x) {
          console.log(x)
        })
      if we don't call subscribe, observable won't run
        like functions
      functions are sync/async?
        ex
          console.log('before')
          console.log(foo.call())
          console.log('after')
      what about observable
        ex
          console.log('before')
          bar.subscribe(function (x) {
            console.log(x)
          })
          console.log('after')
        same result
          so, observer call is the same as function call
      what is the difference?
        in functions you cannot return after returning once
        observables can return multiple values in time
        observable.subscribe() give values in time
      ex
        var bar = Rx.Observable.create(function (observer) {
          console.log('hello')
          observer.next(42)
          observer.next(100)
          setTimeout(function() {
            observer.next(300)
          }, 1000)
        })
        console.log('before')
        bar.subscribe(function (x) {
          console.log(x)
        })
        console.log('after')
      so observables are generalizations of functions
        with regard to
          time of returning value
          number of returned values
    03
      generator functions do similar stuff
        return multiple values
      ex
        function* baz() {
          consele.log('hello')
          yield 42
          yield 100
        }
        var iterator = baz()
        console.log(iterator.next().value)
        console.log(iterator.next().value)
      difference with observable?
        rx
          observable = producer (push)
          observer subscriber = consumer
        generator
          generator = producer (pull)
          consumer determines when values are sent
        you cannot put setTimeout in producer side with generators
          time to consume is determined by consumer
        generators = passive lazy factories of values
          useful with fibonacci
    04
      how to handle errors in observables
        var bar = Rx.Observable.create(function (observer) {
          try {
            console.log('hello')
            observer.next(42)
          } catch (err) {
            observer.error(err)
          }
        })
        bar.subscribe(function nextValueHandler(x) {
          console.log(x)
        }, function errorHandler(err) {
          console.log('wrong: ' + err)
        })
    05: observablces can complete
      complete: no values more
      ex
        var bar = Rx.Observable.create(function (observer) {
          try {
            console.log('hello')
            observer.next(42)
            setTimeout(function() {
              observer.next(300) 
              observer.complete()
            })
          } catch (err) {
            observer.error(err)
          }
        })
        bar.subscribe(function nextValueHandler(x) {
          console.log(x)
        }, function errorHandler(err) {
          console.log('wrong: ' + err)
        })
    06: creation operator of
      from, create, of
      of: sequence of values
      ex: of
        var bar = Rx.observable.create(function (observer) {
          observer.next(42)
          observer.next(100)
          observer.next(200)
          observer.complete()
        }
        -->
        var foo = Rx.observable.of(42, 100, 200)
    07: from fromarray frompromise
      ex: fromArray
        var foo = Rx.observable.fromArray([42, 100, 200])
      ex: fromPromise
        var foo = Rx.observable.fromPromise(promise
          fetch('https://null.jsbin.com')
        )
        bar.subscribe(function nextValueHandler(x) {
          console.log('next ' + x.status)
        }, function errorHandler(err) {
          console.log('wrong: ' + err)
        }, function () {
          console.log('done')
        })
      ex: from
        var foo = Rx.observable.from([42, 100, 200])
        var foo = Rx.observable.from(promise
          fetch('https://null.jsbin.com')
        )
      ex: from with generator
        function* generator() {
          yield 10
          yield 20
          yield 30
        }
        var iterator = generator()
        var foo = Rx.Observable.from(iterator)
    08: fromEventPattern fromEvent
      we have api with:
        addEventHandler
        removeEventHandler
      then use: fromEventPattern
      ex
        Rx.Observable.fromEventPattern(
          addEventHandler, removeEventHandler
        )
      ex
        function addEventHandler(handler) {
          document.addEventListener('click', handler)
        }
        function removeEventHandler(handler) {
          document.removeEventListener('click', handler)
        }
        var foo = Rx.Observable.fromEventPattern(
          addEventListener, remoteEventHandler
        )
      how does it work under the hood?
        code
          function fromEventPattern(add, remove) {
            return Rx.Observable.create(function (observer) {
              add(function (ev) {
                observer.next(ev)
              })
            })
          }
          var foo = fromEventPattern(addEventHandler, removeEventHandler)
      fromEvent: specialized version of fromEventPattern
        mainly for DOM
      ex
        var foo = Rx.Observable.fromEvent(document, 'click')
    09: empty never throw
      ex
        var foo = Rx.Observable.empty()
        // done
        ===
        Rx.Observable.create(function (observer) {
          observer.complete()
        })
      think of it: zero
      ex: never
        var foo = Rx.Observable.empty()
        // nothing happens
        ===
        Rx.Observable.create(function (observer) {
        })
      think of it: infinite observer
        observer waits infinitely
      ex: throw
        var foo = Rx.Observable.throw(new Error('bla'))
        // nothing happens
        ===
        Rx.Observable.create(function (observer) {
          observer.error()
        })
    10: interval, timer
      alternatives to setInterval
      ex:
        Rx.Observable.create(function (observer) {
          var i = 0
          setInterval( function () {
            observer.next(i)
            i = i + 1
          }, 10000)
        })
        -->
        Rx.Observable.interval(1000)
      it sets a new interval
        ex
      ex: timer
        Rx.Observable.timer(3000, 1000)
      wait for 3 sec, then work like interval(1000)
      can take date too
    11: create
      same as calling constructor 
        ex
          var bar = Rx.observable(function (observer) {
            observer.next(42)
            observer.next(100)
            observer.next(200)
            observer.complete()
          }
        give a name for parameter function
          function subscribe(observer) {
            observer.next(42)
            observer.next(100)
            observer.next(200)
            observer.complete()
          }
          var bar = Rx.observable(subscribe)
        alternative way of writing subscriber
          bar.subscribe(function nextValueHandler(x) {
            console.log('next ' + x.status)
          }, function errorHandler(err) {
            console.log('wrong: ' + err)
          }, function () {
            console.log('done')
          })
          -->
          var observer = {
            next: function (x) { console.log('next + x) }, 
            error: function (err) { console.log('error ' + err) },
            complete: function () { console.log('done') },
          }
          bar.subscribe(observer)
        together
          function subscribe(observer) {
            observer.next(42)
            observer.next(100)
            observer.next(200)
            observer.complete()
          }
          var bar = Rx.observable(subscribe)
          var observer = {
            next: function (x) { console.log('next + x) }, 
            error: function (err) { console.log('error ' + err) },
            complete: function () { console.log('done') },
          }
          bar.subscribe(observer)
        write without Rx
          function subscribe(observer) {
            observer.next(42)
            observer.next(100)
            observer.next(200)
            observer.complete()
          }
          var observer = {
            next: function (x) { console.log('next + x) }, 
            error: function (err) { console.log('error ' + err) },
            complete: function () { console.log('done') },
          }
          subscribe(observer)
        Rx adds operators to this
      12: return subscriptions
        return unsubscribe function to stop receiving values
        ex: without rx
          function subscribe(observer) {
            var id = setInterval(function () {
              observer.next('hi')
            }, 1000)
            return function unsubscribe() {
              clearInterval(id)
            }
          }
          var unsubscribe = subscribe({
            next: function (x) { console.log('next + x) }, 
            error: function (err) { console.log('error ' + err) },
            complete: function () { console.log('done') },
          })
          setTimeout( function() {
            unsubscribe()
          }, 4500)
        ex: with rx
          -->
          var foo = new Rx.Observable(subscribe)
          var subscription = foo.subscribe(..)
          setTimeout( function() {
            subscription.unsubscribe()
          }, 4500)
      13:
      next

## RxJS Beyond the Basics: Operators in Depth

    https://egghead.io/courses/rxjs-beyond-the-basics-operators-in-depth
    01
      ex: map, filter, merge, combineLatest
      input: observable
      output: observable
      ex
        var foo = Rx.Observable.of(1,2,3,4,5)
        function multiplyByTen(source) {
          var result = Rx.Observable.create(function subscribe(observer) {
            source.subscribe(
              function (x) { observer.next(x*10)},
              function (err) { observer.error(err) },
              function () { observer.complete() }
            )
          })
          return result
        }
        var bar = multiplyByTen(foo)
        var observer = {
          next: function (x) { console.log('next + x) }, 
          error: function (err) { console.log('error ' + err) },
          complete: function () { console.log('done') },
        }
        bar.subscribe(observer)
      -->
      use prototype
        function multiplyByTen() {
          var source = this
          ..
        }
        Rx.Observable.prototype.multiplyByTen = multiplyByTen
        var bar = foo.multiplyByTen
      --> generalize it 
        function multiplyByTen(multiplier) {
          ..
          function (x) { observer.next(x*multiplier)},
          ..
        }
        Rx.Observable.prototype.multiplyBy = multiplyBy
        var bar = foo.multiplyBy(10)
    02: marble diagrams
      lower case letters
        --a-b--c-------------------------------
        values delivered to observer.next()
      numbers
        --1-3--5-------------------------------
        values delivered to observer.next()
      pipe character
        --1-3--5------------------------------|
        completed
      X character
        --1-3--5------------------------------X
        error
      ex: Rx.Observable.interval(0,1,2,3,4,5)
        4 dashes = 1 sec
        ---0---1---2---3---4---5--...
      ex: Rx.Observable.of(1,2,3,4)
        (1234)---...
        (paranthesis) means delivered syncronously
      ex: Rx.Observable.interval(0,1,2,3,4,5)
        source_stream
        operator
        result_stream
        foo: ---0---1---2---...
               multiplyBy(2)
        bar: ---0---2---4---...
    03: map, mapTo
      ex
        function calculate(transformationFn) {
          var source = this
          var result = Rx.Observable.create(function subscribe(observer) {
            source.subscribe(
              function (x) { observer.next(transformationFn(x))},
              function (err) { observer.error(err) },
              function () { observer.complete() }
            )
          })
          return result
        }
        Rx.Observable.prototype.calculate = calculate
        var bar = foo.calculate(x => x * 2)
      map is calculate
      ex
        var bar = foo.map( x => 10 )
        ===
        var bar = foo.map( () => 10)
        ===
        var bar = foo.mapTo(10)
    04: utility op: do
      return what you get exactly
      ex
        var bar = foo.do(function (x) {
          console.log('!' + x)
        })
      special case of map, equivalent to:
        var bar = foo.map(function (x) {
          console.log('!' + x)
          return x
        })
      useful for debugging
      ex
        foo: ---0---1---...
              do(x => console.log('before ' + x))
             ---0---1---...
              map(x => x * 2)
             ---0---2---...
              do(x => console.log('after ' + x))
             ---0---2---...
      ex
        var bar = foo
          .do(x => console.log('before ' + x))
          .map(x => x * 2)
          .do(x => console.log('after ' + x))
    05: filter
    06: take first skip
      ex
        --0--1--2--3--4--5--6--7-
          take(3)
        --0--1--2|
      ex
        --0--1--2--3--4--5--6--7-
          first()
        --0|
      ex
        --0--1--2--3--4--5--6--7-
          skip(3)
        -----------3--4--5--6--7-
    07: takeLast, skipLast, last
      take, first, skip
        refer to beginning of stream
      how to refer to end?
        only if stream completes
      ex
        --0--1--2--3--4--5--6--7-
          take(3)
        --0--1--2|
          takeLast(2)
        ---------(12|)
      ex
        --0--1--2--3--4--5--6--7-
          take(3)
        --0--1--2|
          skipLast(2)
        ---------(0|)
    08: concat, startWith
      how to replace completion of a stream with a new stream?
        only if a stream completeds (finite)
      ex
        --0--1--2--3--4--5--6--7-
          take(3)
        --0--1--2|                (foo)
                 (345|)           (more)
          concat
        --0--1--2(345|)
      code
        var foo = Rx.Observable.interval(100).take(3)
        var more = Rx.Observable.of(3,4,5)
        var bar = foo.concat(more)
      how to prepend values?
        var bar = foo.startWith('a')
      ex
        --0--1--2--3--4--5--6--7-
          take(3)
        --0--1--2|                (foo)
          startWith('a')
        a-0--1--2|                
      doesn't assume finite stream
      ex
        --0--1--2--3--4--5--6--7-
          startWith('a')
        a-0--1--2--3--4--5--6--7-
    09: combination operator merge
      concat combines sequentially
      ex
        ----0----1---2--  (foo)
        --0----1-----3--  (bar)
           merge
        --0-0--1-1---(23)--  
    10: combineLatest
      merge: or style
      combineLatest: and style
      ex
        ----0----1---2--  (foo)
        --0----1-----3--  (bar)
          foo.combineLatest(bar, (x,y) => x + y)
        ----0--1-2---7--  
    11: withLatestFrom
      similar to combineLatest
        and style
      ex
        ----H--e---l---l---o|  (foo)
        --0--1---0---1---0|    (bar)
          foo.withLatestFrom(bar, (x,y) => y === 1 ? x.toUpperCase() : c.toLowerCase())
        ----h--E---l---L---o|  
      uppercase if latest value from bar is 1
      this is a map
    12: zip
      zip foo and bar
        and-style
        combine 1. of foo with 1. of bar
        combine 2. of foo with 2. of bar
        ...
      ex
        ----0----1---2--  (foo)
        --0----1-----3--  (bar)
          zip((x,y) => x + y)
        ----0----2---5--
      ex: spread a sync value over time
        (hello|)          (foo)
        --0--1--2--3--4|  (bar)
          zip((x,y) => x)
        --h--e--l--l--o|
    13: scan
      combining values over time of one observable
        horizontal combinator
      ex
        ---h--e--l--l--o
          scan((acc,x) => acc + x, '')
        ---h--(he)(hel)(hell)(hello)
    14: buffer
      group values by n
      varieties
        buffer
        bufferCount
        bufferTime
      ex
        ---h--e--l--l--o|
          bufferCount(2)
        ------([h,e])---ll--o|
      ex
        ---h--e--l--l--o|
          bufferTime(300ms)
        ------([h,e])---ll--o|
      ex
        ---h--e--l--l--o|
        -------0-----1--2|
          buffer(closing observable)
        ------([h,e])---ll--o|
    15: delay
      ex
        --0--1--2--3--4|
          delay(1000)
        ----0--1--2--3--4|
      ex: delayWhen(fn) :: Observable -> () -> Observable
        --0--1--2--3--4|
          delay(x => ----0| )
        ----0--1--2--3--4|
      code
        var result = foo.delayWhen(x =>
          Rx.Observable.interval(1000).take(1)
        )
        // 1 saniyelik gecikme
      variable amount of delay possible
      code
        var result = foo.delayWhen(x =>
          Rx.Observable.interval(x * x * 100).take(1)
        )
    16: debounce, debounceTime
      similar to delay and delayWhen
      delay(1000) // waits 1000 ms
        debounceTime(1000) // waits for 1000 silence
          silence: time between events
        ancak 1000 ms boşluk olursa eventi alır
          drop if less than 1000 ms passes between two events
      ex
        --0--1--2|
          debounceTime(1000)
        ------------2|
        assume: each - is 250 ms
      ex: text field
        var inputText = Rx.Observable.fromEvent(fieldElem, 'input')
          .map(ev => ev.target.value)
          .debounceTime(500)
        // ancak 500 ms kullanıcı bir şey yapmazsa göster
      ex: çok küçük debounceTime
        --0--1--2|
          debounceTime(50)
        --0--1--2|
        // bu durumda sadece 50 ms delay olur
      delayWhen similar to debounce
      ex: fn ile debounce belirleme
        foo.debounce(() => Rx.Observable.interval(1000).take(1))
    17: throttle, throttleTime
      debounce: rate limiting operation
      more similar ops:
        throttleTime ~ debounceTime
        throttle ~ debounce
        work like debounce but backwards
      debounce waits for silence, then emits
        throttle first emits, then causes silence
      ex: ----- 1 sec silence
        --0--1--2-----4|
          throttleTime(1000)
        --0-----2-----4|
      other rate limiting ops
        auditTime, audit
      throttleTime is a filtering op
        it drops some values
        debounce has delay, therefore it is transforming too
    18: distinct, distinctUntilChanged
      keep registry (list of events)
      ex
        --a--b--a--c--b|
          distinct()
        --a--b-----c---|
      custom compare function
      ex
        var result = foo.disticnt( (x,y) =>
          x.toLowerCase() === y.toLowerCase()
        )
      ex
        --a--b--A--c--b|
          distinct(compare)
        --a--b-----c---|
      flusher arg
        this is observable
        when should registry be cleared
      ex
        var flusher = Rx.Observable.interval(1100).take(1).concat(Rx.Observable.never())
        // temizler registry'yi
      ex
        --a--b--A--c--b|
        ------0--------...
          distinct
        --a--b--A--c--b|
      distinct yerine distinctUntilChanged daha çok kullanılır
        keeps recent registry only
      ex
        --a--b--a--c--b|
          distinctUntilChanged()
        --a--b--a--c--b|
      ex
        --a--b--a--a--b|
          distinctUntilChanged()
        --a--b--a-----b|
      can be used as rate limiting operator
        --a--b--aaaab
    19: catch
      ex
        --a--b--c--d--2|  (foo)
          map(toUpperCase)
        --a--b--c--d--#  (bar)
      catch replaces error with another observable
      ex
        var result = bar.catch(error => Rx.Observable.of('Z'))
      ex
        --a--b--c--d--2|  (foo)
          map(toUpperCase)
        --a--b--c--d--#  (bar)
          catch(# => -Z|)
        --a--b--c--d--Z  
      replace with complete()
        var result = bar.catch(error => Rx.Observable.empty())
      ex
        --a--b--c--d--2|  (foo)
          map(toUpperCase)
        --a--b--c--d--#  (bar)
          catch(# => -Z|)
        --a--b--c--d--|  
      catch takes 2nd arg: output observable
        output obs is result observable
        allows retry behavior
      ex
        var result = bar.catch((error, outputObs) => outputObs)
      ex
        --a--b--c--d--2|  (foo)
          map(toUpperCase)
        --a--b--c--d--#  (bar)
          catch(# => )
        --a--b--c--d----a--b--c--d----a--b--c--d--   
    20: retry, retryWhen
    next

## RxJS Subjects and Multicasting Operators

![img/ss-174.png](/images/ss-174.png)
![img/ss-172.png](/images/ss-172.png)

    https://egghead.io/courses/rxjs-subjects-and-multicasting-operators
    01
      each Observable has only one observer
        observable.subscribe(observerA)
        observable.subscribe(observerB)
        these two observers both listen to independent executions of observable
        /Users/mertnuhoglu/Dropbox/public/img/ss-174.png
      how to share an observable with multiple observers?
        subjects 
    02
      how to share an observable with multiple observers?
        we need to have only one subscribe() call
        observable.subscribe(bridgeObserver)
        var bridgeObserver = {
          next: function (x) {
            this.observers.forEach(o => o.next(x)
          }
          ..
          observers: [],
          addObserver: function (observer) {
            this.observers.push(observer)
          }
        }
        bridgeObserver.addObserver(observerA)
        setTimeout(function () {
          bridgeObserver.addObserver(observerB)
        }, 2000)
      but this is in effect: bridgeObserver is as if it is an Observable
        instead of addObserver, change it with subscribe() then it is an observable
        this is what Rx.Subject() is
      ex
        var subject = new Rx.Subject()
        observable.subscribe(subject)
        subject.subscribe(observerA)
        /Users/mertnuhoglu/Dropbox/public/img/ss-172.png
    03: use subject as an event bus

## Use Higher Order Observables in RxJS Effectively

    https://egghead.io/courses/use-higher-order-observables-in-rxjs-effectively

## Step-by-Step Async JavaScript with RxJS

    https://egghead.io/courses/step-by-step-async-javascript-with-rxjs
    ch01
      Rx.Observable.timer(0, 100)
        .map(i => `Seconds ${i}`)
        .subscribe(text => {
          const container = document.querySelector('#app');
          container.textContent = text;
        });
      logic: timer().map(..)
      effects: subscribe(..)
        ≅ model.subscribe( view )
        view reactive
        how does view work? check inside
      logic: functional
      effects: imperative
      notes
        what is effect: changing something outside (state)
        why subscribe?
          foo.subscribe({bar.f()})
            foo: active
            bar: reactive
            what is bar?
            it is view
      notes: on subscribe
        these are equivalent:
          model.subscribe(view)
          model.addListener(view)
          model.subscribe( fun(event) {..} )
          model.subscribe( text => ... )
          observable.subscribe(observer)
    ch02: main function
      code
        function main() {
          return Rx.Observable.timer(..)
            .map(..)
        function DOMEffect(text$) {
          text$.subscribe( .. )
        function consoleLogEffect(msg$) {
          msg.subscribe(..)
        const sink = main()
        DOMEffect(sink)
        consoleLogEffect(sink)
      notes
        observables ≅ functions wrapping variables
      question
        why do you call effects imperative? they work with observables as well.
      notes - sink
        x$: read: x stream
        driver's input is main's output
          | fun    | input  | output |
          |--------|--------|--------|
          | driver | sink   | source |
          | main   | source | sink   |
        we look from the perspective of main: source: input, sink: output
    ch03: different effects encapsulated
      code
        function main() {
          return {
            DOM: ..
            Log: ..
        const sinks = main()
        DOMEffect(sinks.DOM)
        consoleLogEffect(sinks.Log)
    ch04: run and driver functions
      encapsulate run
        code
          function run(mainFn) {
            const sinks = mainFn()
            DOMEffect(sinks.DOM)
          run(main)
        run
          ties together: main and effects
      generify effects inside run
        code
          DOMEffect(sinks.DOM) 
          >>>
          function run(mainFn, effects) {
            ..
            Object.keys(effects).forEach( key => {
              effects[key]
            });
          const effectsFunctions = {
            DOM = DOMEffect,
            Log = ..
          run(main, effectsFunctions)
      rename effects as driver
        code
          const effectsFunctions = ..
          >>>
          const drivers = ..
        why?
          driver: interface between external world and our app
    ch05: read effects
      rationale
        we have write effects
        what about read effects
          anything from external world to app?
      code
        function main(DOMSource) {..
        function DOMDriver(..) {
          ..
          const DOMSource = Rx.Observable.fromEvent(document, 'click')
          return DOMSource
      terms
        source: input (read) effects
        sink: output (write) effects
      cycle logic
        a = f(b)
        b = g(a)
      corresponds to
        sinks = main(sources)
        sources = drivers(sinks)
      how to solve cyclic definition?
        bProxy = ...
        a = f(bProxy)
        b = g(a)
        bProxy.imitate(b)
      code
        function run(..) {
          const proxyDomSource = new Rx.Subject()
          const sinks = mainFn(proxyDomSource)
          const DOMSource = drivers.DOM(sinks.DOM)
          DOMSource.subscribe(click => proxyDomSource.onNext(click))
      code - to use ui input
        function main(DOMSource) {
          const click$ = DOMSource
          return {
            DOM: click$
              .startWith(null)
              .flatMapLatest(() =>
                ..
    ch13: http driver
      notes
        sources.DOM -> sinks.HTTP -> sources.HTTP -> sinks.DOM


