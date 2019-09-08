---
title: "Notes from Book: JavaScript Allonge - Reg raganwald Braithwaite"
date: 2017-11-24T10:24:26+03:00
draft: false
description: ""
tags: ["book_notes", "js"]
categories: ["software", "js"]
type: post
url:

---

My reading notes from the book

<!--more-->

<!-- toc -->

## Prelude

    values are expressions
      42 
      # value and expression
      42 + 12
      # expression: objects with operators are not value
    values and identity
      === operator
      !== operator
      are two objects identical?
        4 opts:
          1: different types
          2: same type different contents
          3: same type+content. no difference
            primitive values
          4: different ids
            reference types
            Array: [1,2]
            functions
            they are not identical even with same contents
            they are expressions but not values
              lisp: expressions are values too

## Basic Numbers

    literal:
      a notation for representing a fixed value 
      ex: integers, strings, booleans
        anonymous function is a literal for function type
      04
        octat literal
      internally 042 and 34 have same representation
        as double precision floating point
    floating
      internal representation: binary
        some fractions base 10 don't have exact representations base 2
        ex: 
          1.0 # => 1
          0.1 # => 1
          0.1 + 0.1 + 0.1 # => 0.3...4
      for monetary amounts: never use float
    operations on numbers
      precedence of operators
      modulus: %
      unary negation: -
      -(5 % 3) # -2

## Basic functions

    functions are values
    second simplest function
      () => 0
      (() => 0) # [Function]
      (() => 0)() # 0
    comma operator
      takes two args, evals to the value of rh arg
        (1,2) # 2
    simplest block
      a block has zero or more statements
        () => {} 
        (() => {})() # undefined
    undefined
      its own type of vvalue
      acts like a value type
      single value
        every undefined is identical
        not like NULL in SQL
    void
      void is an operator
      returns undefined
        void 0 # undefined
        void 1 # undefined
    back on the block
      blocks contain statements
      what's a statement
        first kind: expression
          return undefined
        (() => {1})() # undefined
      block returns a value
        { return 0 }
        return statement
          not an expression
            (() => return 0) ()
            # error
    functions that evaluate to functions
      () => () => 0

## Arguments

    (room) => {}
    call by value
    variables and bindings
      (x) => (y) => x
      when a function is invoked
        invoked: applied to arguments
        a new environment is created
        environment: a dictionary that maps variable to values by name
    call by sharing
      value types vs. reference types
      what about reference types
        js places references to reference types in environments

## Closures and Scope

    how nested functions work?
      ((x) => (y) => x)(1)(2) # 1
    the function: (y) => x
      it contains a free variable x
        a variable that is not bound within the function
    pure functions:
      functions with no free variables
    closures
      functions with free variables
    pure functions: always mean the same thing
    a pure function can contain a closure
      (x) => (y) => x
             |\closure/|
      |\pure function/|
    environments
      environment for ((y) => x)(2)
        {y:2, '..': {x:1, ...}}
        '..' means parent, enclosure, super environment
      I Combinator (identity function)
        (x) => x
      K Combinator (Kestrel)
        (x) => (y) => x
      currying: 
        also called partial application
          calling with some of its args
    shadowy variables
      using same names
        (x) =>
          (x,y) => x + y
      say: it shadows the parent environment's binding
    chicken or egg
      global environment
      how to avoid this?
        (() => {
          ... // code here
        })()
      so no global state is shared

## Coffee Craving

    anonymous functions: functions without a name
    ex:
      ((diameter) =>
        ((PI) =>
          diameter * PI(3.14)(2)
      # 6.28
    this separates concerns nicely
      outer function: describes its parameters
        everything else is encapsulated
        naming PI is its concern, not ours
      but invoking functions is more expensive than evaluating expressions
    const
      is a statement
      const PI = 3.14
      no cost of function invocation
    multitple bindings per line
      const x = 3, y = 5
    nested block
      if statement
        is not an expression
        its clauses are statements or blocks
    const and lexical scope
    rebinding
      not allowed for const bounded names
        only in a new environment (scope)

## Naming functions

    problem
      this does not name a function:  
        const repeat = (x) => x
      similar to this:
        const answer = 42
      this doesn't name number 42. 
      it binds an anonymous function to a name
        but the function remains anonymous
    function keyword
      function(x) { return x }
      we have to use return since we use block
      naming
        const repeat = function r (..) {..}
        # r is the name of function
      but r is not the name we bind to the value of the function
        r: function's name
        repeat: name in the environment
        this is: named function expression
      think of binding names
        as properties of the environment 
        not of the function
      the name is a property:
        repeat.name # r
    function declarations
      function declaration statement
        function someName () {
        ..
        }
      differences
        1. sıralama önemsiz. en tepede tanımlanmış sayılır
    function declaration caveat

## Combinators and Function Decorators

    specific types of higher-order functions
    combinators
      in mathematics
        a higher order function that uses function application and combinators 
      looser definition
        higher order pure functions
        that take only functions as arguments and return a function
      Compose combinator
        logic: B combinator
        impl:
          const compose = (a, b) =>
            (c) => a(b(c))
        ex:
          compose( double, addOne )
    a balanced statement about combinators
      combinators use verbs 
      names are second to them
    function decorators
      definition
        a higher order function 
        that takes one function, returns another function
        the returned function is a variation of argument function
      ex:
        const not = (fn) => (x) => !fn(x)
        usage:
          not(f)(42)
          const something = (x) => x != null
          const nothing = (x) => !something(x)
          const nothing = not(something)
      can be closures too

## Building Blocks

    usefulness of composition 
      %20 comes from using it
      %80 comes from organizing your code st you can use it
      ex: once, maybe
        you can use if statements
        but once and maybe compose
        ex:
          const invokeTransfer = once(maybe(actuallyTransfer(...)))
    partial application
      supply some of the arguments
        get a function that represents part of our application
      ex:
        const squareAll = (array) => map(array, (n) => n * n)
        # map's second arg is applied
        # squareAll is still the map function
      abstract one level higher:
        const mapWith = (fn) =>
          (array) => map(array, fn)
        const squareAll = mapWith( (n) => n * n)
      partial application is orthogonal to composition
        const safeSquareAll = mapWith(maybe((n) => n * n))

## Magic Names

    when a function is called
      js binds values to some magic names 
      in addition to arguments
    function keyword
      two rules
        1. for function keyword
        2. fat arrow functions
      magic names
        this
          bound to function's context
        arguments
          arguments[0]
          not an array
    fat arrow
      acquires bindings for this and arguments
        from its enclosing scope

## Recipes with Basic Functions

    Partial Application“
      ex: applying an argument, leftmost or rightmost
        const callFirst = (fn, larg) =>
          function (...rest) {
            return fn.call(this, larg, ...rest);
          }
        const callLast = (fn, rarg) =>
          function (...rest) {
            return fn.call(this, ...rest, rarg);
          }
        const greet = (me, you) =>
          `Hello, ${you}, my name is ${me}`;
        const heliosSaysHello = callFirst(greet, 'Helios');
        heliosSaysHello('Eartha')
          //=> 'Hello, Eartha, my name is Helios'
        const sayHelloToCeline = callLast(greet, 'Celine');
        sayHelloToCeline('Eartha')
          //=> 'Hello, Celine, my name is Eartha”
      ex: multiple args
        const callLeft = (fn, ...args) =>
          (...remainingArgs) =>
            fn(...args, ...remainingArgs);
        const callRight = (fn, ...args) =>
          (...remainingArgs) =>
            fn(...remainingArgs, ...args);”
    Unary
      takes any function
        turns it to take only one arg
      ex: map function
        map takes a function
          passes 3 args
            [1, 2, 3].map(function (element, index, arr)”
        we don't want to pass 3, just 1
        unary with parseInt“
          const unary = (fn) =>
            fn.length === 1
              ? fn
              : function (something) {
                  return fn.call(this, something)
                }
          ['1', '2', '3'].map(unary(parseInt))
            //=> [1, 2, 3]”
    Tap
      K combinator:
        const K = (x) => <y) => x
      an application:
        sideffects
        but keep the value around
      ex
        const tap = (value) =>
          (fn) => (
            typeof(fn) === 'function' && fn(value),
            value
          )
    Maybe
    Once
    Left-Variadic Functions
    Compose and Pipeline
