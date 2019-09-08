---
title: "Notes from Book: Frisby's Mostly Adequate Guide to Functional Programming - Brian Lonsdorf"
date: 2017-11-24T09:38:45+03:00
draft: false
description: ""
tags: ["book_notes", "functional_programming"]
categories: ["software", "functional_programming"]
type: post
url:

---

My reading notes from the book

<!--more-->

<!-- toc -->

## Chapter 2: First Class Functions 

    A quick review
      ex: point-free programming
        // this line
        return ajaxCall(function(json) {
          return callback(json);
        });
        // is the same as this line
        return ajaxCall(callback);
      ex
        var getServerStuff = function(callback) {
          return ajaxCall(callback);
        };
        // ...which is equivalent to this
        var getServerStuff = ajaxCall; // <-- look mum, no ()'s
    Why favor first class?
      ex: no need to change args in wrapped function
        httpGet('/post/2', function(json) {
          return renderPost(json);
        });
        // add err
        httpGet('/post/2', function(json, err) {
          return renderPost(json, err);
        });
        // no change in first class function style
        httpGet('/post/2', renderPost);
      no need to name args
      multiple names for the same concept is source of confusion
        ex: do same thing, but one is general
          // specific to our current blog
          var validArticles = function(articles) {
            return articles.filter(function(article) {
              return article !== null && article !== undefined;
            });
          };
          // vastly more relevant for future projects
          var compact = function(xs) {
            return xs.filter(function(x) {
              return x !== null && x !== undefined;
            });
          };
      beware from "this" keyword
        if any underlying function uses this
          it is subject to leaky abstraction
        ex
          var fs = require('fs');
          // scary
          fs.readFile('freaky_friday.txt', Db.save);
          // less so
          fs.readFile('freaky_friday.txt', Db.save.bind(Db));

## Chapter 3: Pure Happiness with Pure Functions

    Oh to be pure again
      pure function
        given same input
        always return same output
        and has no observable side effect
      ex:
        slice is pure
        splice has effect
          modifies input array
        var xs = [1, 2, 3, 4, 5];
        // pure
        xs.slice(0, 3);
        //=> [1, 2, 3]
        xs.slice(0, 3);
        //=> [1, 2, 3]
        xs.slice(0, 3);
        //=> [1, 2, 3]
        // impure
        xs.splice(0, 3);
        //=> [1, 2, 3]
        xs.splice(0, 3);
        //=> [4, 5]
        xs.splice(0, 3);
        //=> []
      ex:
        // impure
        var minimum = 21;
        var checkAge = function(age) {
          return age >= minimum;
        };
        // pure
        var checkAge = function(age) {
          var minimum = 21;
          return age >= minimum;
        };
        # checkAge depends on mutable variable
          ie on system state
          this increases the cognitive load
          it is a big contribution to system complexity
            http://www.curtclifton.net/storage/papers/MoseleyMarks06a.pdf
    Side effects may include...
      what is sie effect
        anything that changes during computation
        other than the result
        a change of the system state
      ex
        file system
        database
        http calls
        mutations
        screen/logging
        user input
        querying DOM
        accessing system state
      so what is of use?
        they are not forbidden
        we want to contain them
      math
        pure functions are mathematical functions
        fp is all about them
    The case for purity
      Cacheable
        technique: memoization
        ex
          var squareNumber = memoize(function(x) {
            return x * x;
          });
          squareNumber(4);
          //=> 16
          squareNumber(4); // returns cache for input 4
          //=> 16
          var memoize = function(f) {
            var cache = {};
            return function() {
              var arg_str = JSON.stringify(arguments);
              cache[arg_str] = cache[arg_str] || f.apply(f, arguments);
              return cache[arg_str];
            };
          };
        ex: transforming impure funs into pure ones by delaying evaluation
          var pureHttpCall = memoize(function(url, params) {
            return function() {
              return $.getJSON(url, params);
            };
          });
          # doesn't make http call
            returns a function that will do so when called
            it is pure why?
              because it always returns same output given same input
              the function that it returns is not pure
      Portable/Self-Documenting
        ex
          //impure
          var signUp = function(attrs) {
            var user = saveUser(attrs);
            welcomeUser(user);
          };
          var saveUser = function(attrs) {
              var user = Db.save(attrs);
              ...
          };
          var welcomeUser = function(user) {
              Email(user, ...);
              ...
          };
          //pure
          var signUp = function(Db, Email, attrs) {
            return function() {
              var user = saveUser(Db, attrs);
              welcomeUser(Email, user);
            };
          };
          var saveUser = function(Db, attrs) {
              ...
          };
          var welcomeUser = function(Email, user) {
              ...
          };
          # functions are honest about their dependencies
        Erlang creator, Joe Armstrong: "The problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana... and the entire jungle".
      Testable
      Reasonable
        referential transparency
      Parallel Code

## Chapter 4: Currying

    Can't live if livin' is without you
      ex
        var add = function(x) {
          return function(y) {
            return x + y;
          };
        };
        var increment = add(1);
        var addTen = add(10);
        increment(2);
        // 3
        addTen(2);
        // 12
      curry() function makes it easier
        var curry = require('lodash/curry');
        var match = curry(function(what, str) {
          return str.match(what);
        });
        var replace = curry(function(what, replacement, str) {
          return str.replace(what, replacement);
        });
        var filter = curry(function(f, ary) {
          return ary.filter(f);
        });
        var map = curry(function(f, ary) {
          return ary.map(f);
        });
      data as last argument
      usage
        match(/\s+/g, 'hello world');
        // [ ' ' ]
        match(/\s+/g)('hello world');
        // [ ' ' ]
        var hasSpaces = match(/\s+/g);
        // function(x) { return x.match(/\s+/g) }
        hasSpaces('hello world');
        // [ ' ' ]
        hasSpaces('spaceless');
        // null
        filter(hasSpaces, ['tori_spelling', 'tori amos']);
        // ['tori amos']
        var findSpaces = filter(hasSpaces);
        // function(xs) { return xs.filter(function(x) { return x.match(/\s+/g) }) }
        findSpaces(['tori_spelling', 'tori amos']);
        // ['tori amos']
        var noVowels = replace(/[aeiouy]/ig);
        // function(replacement, x) { return x.replace(/[aeiouy]/ig, replacement) }
        var censored = noVowels("*");
        // function(x) { return x.replace(/[aeiouy]/ig, '*') }
        censored('Chocolate Rain');
        // 'Ch*c*l*t* R**n'
    More than a pun/special sauce
      transform any function that works on single elements
        into one that works on arrays
        by wrapping it with map
      ex
        var getChildren = function(x) {
          return x.childNodes;
        };
        var allTheChildren = map(getChildren);
      partial application:
        giving a function fewer args that it expects
        it can remove boiler plate code
        ex: uncurried map:
          var allTheChildren = function(elements) {
            return _.map(elements, getChildren);
          };

## Chapter 5: Coding by Composing

    Functional husbandry
      compose
        var compose = (f,g) => x => f(g(x))
      ex
        var toUpperCase = function(x) {
          return x.toUpperCase();
        };
        var exclaim = function(x) {
          return x + '!';
        };
        var shout = compose(exclaim, toUpperCase);
        shout("send in the clowns");
      ex
        var head = x => x[0]
        var reverse = reduce( (acc,x) => [x].concat(acc), [])
        var last = compose(head, reverse)
      associativity law
        var associative = compose(f, compose(g,h)) == compose(compose(f,g), h)
      this allows variadic (any number of args) compose:
        var lastUpper = compose( toUpperCase, head, reverse )
    Pointfree
      never use data args
        ex
          //not pointfree because we mention the data: word
          var snakeCase = function(word) {
            return word.toLowerCase().replace(/\s+/ig, '_');
          };
          //pointfree
          var snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase);
        ex
          //not pointfree because we mention the data: name
          var initials = function(name) {
            return name.split(' ').map(compose(toUpperCase, head)).join('. ');
          };
          //pointfree
          var initials = compose(join('. '), map(compose(toUpperCase, head)), split(' '));
          initials("hunter stockton thompson");
          // 'H. S. T'
    Debugging
      use trace function
      ex: common mistake with map
        //wrong - we end up giving angry an array and we partially applied map with god knows what.
        var latin = compose(map, angry, reverse);
        latin(['frog', 'eyes']);
        // error
        // right - each function expects 1 argument.
        var latin = compose(map(angry), reverse);
        latin(['frog', 'eyes']);
        // ['EYES!', 'FROG!'])
      ex
        var trace = curry(function(tag, x) {
          console.log(tag, x);
          return x;
        });
        var dasherize = compose(join('-'), toLower, trace('after split'), split(' '), replace(/\s{2,}/ig, ' '));
        // after split [ 'The', 'world', 'is', 'a', 'vampire' ]
        var dasherize = compose(join('-'), map(toLower), split(' '), replace(/\s{2,}/ig, ' '));
        dasherize('The world is a vampire');
        // 'the-world-is-a-vampire'
    Category Theory
      identity
        var id = function(x) {
          return x;
        };
        compose(id, f) == compose(f, id) == f;
        // true

## Chapter 6: Example Application

    Declarative coding
      imperative: how to do a job
      declarative: specification of what we want as a result
        write expressions, instead of step by step instructions
      ex
        // imperative
        var makes = [];
        for (var i = 0; i < cars.length; i++) {
          makes.push(cars[i].make);
        }
        // declarative
        var makes = cars.map(function(car) { return car.make; });
      ex
        // imperative
        var authenticate = function(form) {
          var user = toUser(form);
          return logIn(user);
        };
        // declarative
        var authenticate = compose(logIn, toUser);
    A flickr of functional programming
      code
        <!DOCTYPE html>
        <html>
          <head>
            <script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.11/require.min.js"></script>
            <script src="flickr.js"></script>
          </head>
          <body></body>
        </html>
      flickr.js
        requirejs.config({
          paths: {
            ramda: 'https://cdnjs.cloudflare.com/ajax/libs/ramda/0.13.0/ramda.min',
            jquery: 'https://ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min'
          },
        });
        require([
            'ramda',
            'jquery',
          ],
          function(_, $) {
            var trace = _.curry(function(tag, x) {
              console.log(tag, x);
              return x;
            });
            // app goes here
          });
      our app will do:
        Construct a url for our particular search term
        Make the flickr api call
        Transform the resulting json into html images
        Place them on the screen
      2 impure actions: api call, screen
        we will enclose them
      code: impure
        var Impure = {
          getJSON: _.curry(function(callback, url) {
            $.getJSON(url, callback);
          }),
          setHtml: _.curry(function(sel, html) {
            $(sel).html(html);
          })
        };
      code: make api call
        var url = term => "http:..." + term
        var app = _.compose(Impure.getJSON(trace('response')), url);
        app('cats');
      code: json navigation
        # items.*media.m
        var prop = _.curry(function(property, object) {
          return object[property];
        });
        var mediaUrl = _.compose(_.prop('m'), _.prop('media'));
        var srcs = _.compose(_.map(mediaUrl), _.prop('items'));
      code: print on screen
        var renderImages = _.compose(Impure.setHtml('body'), srcs);
        var app = _.compose(Impure.getJSON(renderImages), url);
      code: make img tags
        var img = function(url) {
          return $('<img />', {
            src: url
          });
        };
        var images = _.compose(_.map(img), srcs);
        var renderImages = _.compose(Impure.setHtml('body'), images);
        var app = _.compose(Impure.getJSON(renderImages), url);
    A Principled Refactor
      optimization by map composition law
        we map over each item to turn it into a media url
        then map again to turn them into img tags
        map f . map g = map( f . g) 
        var law = compose(map(f), map(g)) === map(compose(f,g))
      refactoring:
        // original code
          var mediaUrl = _.compose(_.prop('m'), _.prop('media'));
          var srcs = _.compose(_.map(mediaUrl), _.prop('items'));
          var images = _.compose(_.map(img), srcs);
        line up
          var mediaUrl = _.compose(_.prop('m'), _.prop('media'));
          var images = _.compose(_.map(img), _.map(mediaUrl), _.prop('items'));
        apply composition law
          var mediaUrl = _.compose(_.prop('m'), _.prop('media'));
          var images = _.compose(_.map(_.compose(img, mediaUrl)), _.prop('items'));
        extracting function
          var mediaUrl = _.compose(_.prop('m'), _.prop('media'));
          var mediaToImg = _.compose(img, mediaUrl);
          var images = _.compose(_.map(mediaToImg), _.prop('items'));

## Chapter 7: Hindley-Milner and Me

    What's your type?
      type signatures 
        meta language to
          communicate succinctly and effectively
        expressive power
        we can derive free theorems from them
        useful for
          compile time checks
          best possible documentation
    Tales from the cryptic
      ex
        //  capitalize :: String -> String
        var capitalize = function(s) {
          return toUpperCase(head(s)) + toLowerCase(tail(s));
        }
        capitalize("smurf");
        //=> "Smurf"
        //  strLength :: String -> Number
        var strLength = function(s) {
          return s.length;
        }
        //  join :: String -> [String] -> String
        var join = curry(function(what, xs) {
          return xs.join(what);
        });
        //  match :: Regex -> String -> [String]
        var match = curry(function(reg, s) {
          return s.match(reg);
        });
        //  replace :: Regex -> String -> String -> String
        var replace = curry(function(reg, sub, s) {
          return s.replace(reg, sub);
        });
        //  id :: a -> a
        var id = function(x) {
          return x;
        }
        //  map :: (a -> b) -> [a] -> [b]
        var map = curry(function(f, xs) {
          return xs.map(f);
        });
        //  head :: [a] -> a
        var head = function(xs) {
          return xs[0];
        };
        //  filter :: (a -> Bool) -> [a] -> [a]
        var filter = curry(function(f, xs) {
          return xs.filter(f);
        });
        //  reduce :: (b -> a -> b) -> b -> [a] -> b
        var reduce = curry(function(f, x, xs) {
          return xs.reduce(f, x);
        });
    Narrowing the possibility
      parametricity
        function will act on all types in a uniform manner
    Free as in theorem
      this reasoning gains us free theorems
        from Wadler's paper
      ex
        // head :: [a] -> a
        compose(f, head) == compose(head, map(f));
        // filter :: (a -> Bool) -> [a] -> [a]
        compose(map(f), filter(compose(p, f))) === compose(filter(p), map(f));
      compose(f, head) == compose(head, map(f));
        lhs
          head :: [a] -> a
          f :: a -> a
        take rhs
          map(f) :: [a] -> [a]
          head :: [a] -> a
      compose(map(f), filter(compose(p, f))) === compose(filter(p), map(f));
        lhs
          filter(p.f) --> head
          filter(p.f) :: [a] -> [a]
            because p.f :: a -> Bool
              because f :: a -> a and p :: a -> Bool
      this reasoning applies to any polymorphic type signature
    Constraints
      we can constrain types to an interface
        sort :: Ord a => [a] -> [a]
          a is an Ord
            ie: a implements Ord interface
      these interface declarations are called type constraints
        assertEqual :: (Eq a, Show a) => a -> a -> Assertion

## Chapter 8: Tupperware

    what about
      control flow
      error handling
      asynchronous actions
      state
      effects
    first: create a container
      holds any type of value
      it will be an object
        but we won't give properties, methods 
      it is just a box that stores our data
    code
      var Container = function(x) { this.__value = x; }
      Container.of = x => new Container(x)
    ex
      Container.of(3);
      //=> Container(3)
      Container.of('hotdogs');
      //=> Container("hotdogs")
      Container.of(Container.of({
        name: 'yoda',
      }));
      //=> Container(Container({name: "yoda" }))
    My First Functor
      code: map
        // (a -> b) -> Container a -> Container b
        Container.prototype.map = function(f) {
          return Container.of(f(this.__value));
        }
      code: using map
        Container.of(2).map(function(two) {
          return two + 2;
        });
        //=> Container(4)
        Container.of("flamethrowers").map(function(s) {
          return s.toUpperCase();
        });
        //=> Container("FLAMETHROWERS")
        Container.of("bombs").map(_.concat(' away')).map(_.prop('length'));
        //=> Container(10)
      this is Functor
        Functor: a type that implements map and obeys its laws
        an interface with a contract
        name also: Mappable
      rationale
        why boxing a value and using a map?
        better ask: why asking a box to apply functions?
          abstraction of function application
          when we map a function, we ask the box type to run it for us
          this is very powerful
    Schrödinger's Maybe
      Container is Identity
      other functors have more useful map
      code: Maybe
        var Maybe = function(x) {
          this.__value = x;
        };
        Maybe.of = function(x) {
          return new Maybe(x);
        };
        Maybe.prototype.isNothing = function() {
          return (this.__value === null || this.__value === undefined);
        };
        Maybe.prototype.map = function(f) {
          return this.isNothing() ? Maybe.of(null) : Maybe.of(f(this.__value));
        };
      code: using Maybe
        Maybe.of('Malkovich Malkovich').map(match(/a/ig));
        //=> Maybe(['a', 'a'])
        Maybe.of(null).map(match(/a/ig));
        //=> Maybe(null)
        Maybe.of({
          name: 'Boris',
        }).map(_.prop('age')).map(add(10));
        //=> Maybe(null)
        Maybe.of({
          name: 'Dinah',
          age: 14,
        }).map(_.prop('age')).map(add(10));
        //=> Maybe(24)
      note: Maybe takes care null checks
        since we abstract function application to Maybe.map
      code: pointfree map
        //  map :: Functor f => (a -> b) -> f a -> f b
        var map = curry(function(f, any_functor_at_all) {
          return any_functor_at_all.map(f);
        });
    Use Cases
      ex
        //  safeHead :: [a] -> Maybe(a)
        var safeHead = function(xs) {
          return Maybe.of(xs[0]);
        };
        var streetName = compose(map(_.prop('street')), safeHead, _.prop('addresses'));
        streetName({
          addresses: [],
        });
        // Maybe(null)
        streetName({
          addresses: [{
            street: 'Shady Ln.',
            number: 4201,
          }],
        });
        // Maybe("Shady Ln.")
      ex: returning Maybe(null) to signal failure
        //  withdraw :: Number -> Account -> Maybe(Account)
        var withdraw = curry(function(amount, account) {
          return account.balance >= amount ?
            Maybe.of({
              balance: account.balance - amount,
            }) :
            Maybe.of(null);
        });
        //  finishTransaction :: Account -> String
        var finishTransaction = compose(remainingBalance, updateLedger); // <- these composed functions are hypothetical, not implemented here...
        //  getTwenty :: Account -> Maybe(String)
        var getTwenty = compose(map(finishTransaction), withdraw(20));
        getTwenty({
          balance: 200.00,
        });
        // Maybe("Your balance is $180.00")
        getTwenty({
          balance: 10.00,
        });
        // Maybe(null)
      note: why map(finishTransaction)
          finishTransaction :: Account -> String
          but withdraw(20) :: Account -> Maybe(Account)
          so, map(finishTransaction) :: Maybe(Account) -> Maybe(String)
    Releasing the value
      there will always be end of line
        everything must end with some effect
        zen buddhist koan: "if a program has no observable effect, does it even run?"
      @mine:
        Functor'lar (map) zaten side effectleri izole ediyor. Monad'lara niye ihtiyaç var?
          functor'un ilk ve son işlemi, side effect
          fakat aralarda tamamen pure fonksiyonların kompozisyonu geçerli
          monad'lar yan etkili fonksiyonların aralarda dahi birleştirilmesini sağlıyor
      ex: instead of returning Maybe, return String or Maybe
        //  maybe :: b -> (a -> b) -> Maybe a -> b
        var maybe = curry(function(x, f, m) {
          return m.isNothing() ? x : f(m.__value);
        });
        //  getTwenty :: Account -> String
        var getTwenty = compose( maybe("You're broke!", finishTransaction), withdraw(20));
        getTwenty({ balance: 200.00, });
        // "Your balance is $180.00"
        getTwenty({ balance: 10.00, });
        // "You're broke!"
    Pure Error Handling
      try/catch is not pure
      Left, Right are subclasses of abstract type Either
      code
        var Left = function(x) {
          this.__value = x;
        };
        Left.of = function(x) {
          return new Left(x);
        };
        Left.prototype.map = function(f) {
          return this;
        };
        var Right = function(x) {
          this.__value = x;
        };
        Right.of = function(x) {
          return new Right(x);
        };
        Right.prototype.map = function(f) {
          return Right.of(f(this.__value));
        }
      ex: usage of Either
        Right.of('rain').map(function(str) {
          return 'b' + str;
        });
        // Right('brain')
        Left.of('rain').map(function(str) {
          return 'b' + str;
        });
        // Left('rain')
        Right.of({
          host: 'localhost',
          port: 80,
        }).map(_.prop('host'));
        // Right('localhost')
        Left.of('rolls eyes...').map(_.prop('host'));
        // Left('rolls eyes...')
      Left.map ignores its f
        power comes from the ability to embed an error message
      ex: function fails with Either instead of Maybe(null)
        var moment = require('moment');
        //  getAge :: Date -> User -> Either(String, Number)
        var getAge = curry(function(now, user) {
          var birthdate = moment(user.birthdate, 'YYYY-MM-DD');
          if (!birthdate.isValid()) return Left.of('Birth date could not be parsed');
          return Right.of(now.diff(birthdate, 'years'));
        });
        getAge(moment(), { birthdate: '2005-12-12', });
        // Right(9)
        getAge(moment(), { birthdate: '20010704', });
        // Left('Birth date could not be parsed')
      ex: using Either into compose
        //  fortune :: Number -> String
        var fortune = compose(concat('If you survive, you will be '), add(1));
        //  zoltar :: User -> Either(String, _)
        var zoltar = compose(map(console.log), map(fortune), getAge(moment()));
        zoltar({ birthdate: '2005-12-12', });
        // 'If you survive, you will be 10'
        // Right(undefined)
        zoltar({ birthdate: 'balloons!', });
        // Left('Birth date could not be parsed')
      note: Either(String,_) why _?
        console.log returns nothing
        value should be ignored
      note: fortune is ignorant of the functors
        finishTransaction as well
        at the time of calling, function is wrapped by map
          map(fortune)
          this transforms it to a functory function
        this process is called: lifting
        better: function work with normal data types
          then lifted into the right container when needed
      Either: not only a container for error messages
        it encapsulates logical disjunction (||) in a type
        it encodes the idea of Coproduct
          canonical sum type (or disjoint union of sets)
          because it is the sum of two contained types
        Either can be many things
          as functor: it is used for error handling
      code: either to return unboxed type
        //  either :: (a -> c) -> (b -> c) -> Either a b -> c
        var either = curry(function(f, g, e) {
          switch (e.constructor) {
            case Left:
              return f(e.__value);
            case Right:
              return g(e.__value);
          }
        });
        //  zoltar :: User -> _
        var zoltar = compose(console.log, either(id, fortune), getAge(moment()));
        zoltar({ birthdate: '2005-12-12', });
        // "If you survive, you will be 10"
        // undefined
        zoltar({ birthdate: 'balloons!', });
        // "Birth date could not be parsed"
        // undefined
      note: the use of id above
    Old McDonald had Effects
      a function contains side effect
        we wrapped it to make it pure
      ex
        x
        //  getFromStorage :: String -> (_ -> String)
        var getFromStorage = function(key) {
          return function() {
            return localStorage[key];
          };
        };
      but now getFromStorage(key) by itself is useless
      code
        var IO = function(f) {
          this.__value = f;
        };
        IO.of = function(x) {
          return new IO(function() {
            return x;
          });
        };
        IO.prototype.map = function(f) {
          return new IO(_.compose(f, this.__value));
        };
      IO
        difference from prev functors:
          __value is always a function
        we think of IO
          as containing the return value of the boxed action
          not the box itself
      ex: using IO
        //  io_window :: IO Window
        var io_window = new IO(function() {
          return window;
        });
        io_window.map(function(win) {
          return win.innerWidth;
        });
        // IO(1430)
        io_window.map(_.prop('location')).map(_.prop('href')).map(_.split('/'));
        // IO(["http:", "", "localhost:8000", "blog", "posts"])
        //  $ :: String -> IO [DOM]
        var $ = function(selector) {
          return new IO(function() {
            return document.querySelectorAll(selector);
          });
        };
        $('#myDiv').map(head).map(function(div) {
          return div.innerHTML;
        });
        // IO('I am some inner html')
      note: conceptual return values are written out
        actual return will be { __value: [Function] }
        mapped functions do not run
          they are stacked like command/queue pattern
      @mine:
        it is impossible to understand what happens without taking pen and paper
          and evaluating manually
      when to call impure function inside IO?
        the caller has the responsibility
          of running the effects
      ex: running IO effects
        ////// Our pure library: lib/params.js ///////
        //  url :: IO String
        var url = new IO(function() {
          return window.location.href;
        });
        //  toPairs ::  String -> [[String]]
        var toPairs = compose(map(split('=')), split('&'));
        //  params :: String -> [[String]]
        var params = compose(toPairs, last, split('?'));
        //  findParam :: String -> IO Maybe [String]
        var findParam = function(key) {
          return map(compose(Maybe.of, filter(compose(eq(key), head)), params), url);
        };
        ////// Impure calling code: main.js ///////
        // run it by calling __value()!
        findParam("searchTerm").__value();
        // Maybe([['searchTerm', 'wafflehouse']])
      note: map(.., url)
        map(f,g)
          //  map :: Functor f => (a -> b) -> f a -> f b
          var map = curry(function(f, any_functor_at_all) {
            return any_functor_at_all.map(f);
      note: result's type: IO(Maybe[x]))
        3 functors deep
      note: IO's __value isn't contained value
        it is meant to be called by a caller
      code: rename __value
        var IO = function(f) {
          this.unsafePerformIO = f;
        };
        IO.prototype.map = function(f) {
          return new IO(_.compose(f, this.unsafePerformIO));
        };
    Asynchronous Tasks
      code
        // Node readfile example:
        //=======================
        var fs = require('fs');
        //  readFile :: String -> Task Error String
        var readFile = function(filename) {
          return new Task(function(reject, result) {
            fs.readFile(filename, 'utf-8', function(err, data) {
              err ? reject(err) : result(data);
            });
          });
        };
        readFile('metamorphosis').map(split('\n')).map(head);
        // Task("One morning, as Gregor Samsa was waking up from anxious dreams, he discovered that
        // in bed he had been changed into a monstrous verminous bug.")
        // jQuery getJSON example:
        //========================
        //  getJSON :: String -> {} -> Task Error JSON
        var getJSON = curry(function(url, params) {
          return new Task(function(reject, result) {
            $.getJSON(url, params, result).fail(reject);
          });
        });
        getJSON('/video', {
          id: 10,
        }).map(_.prop('title'));
        // Task("Family Matters ep 15")
        // We can put normal, non futuristic values inside as well
        Task.of(3).map(function(three) {
          return three + 1,
        });
        // Task(4)
      comparison to Promise
        map = then
        but Promise is not pure
      like IO
        wraps a function
        waits for our command
        IO is subsumed by Task
          readFile, getJSON
          placing instructions for future
        unsafePerformIO = fork
      to run Task: call fork
        nonblocking thread
      ex: running with fork
        // Pure application
        //=====================
        // blogTemplate :: String
        //  blogPage :: Posts -> HTML
        var blogPage = Handlebars.compile(blogTemplate);
        //  renderPage :: Posts -> HTML
        var renderPage = compose(blogPage, sortBy('date'));
        //  blog :: Params -> Task Error HTML
        var blog = compose(map(renderPage), getJSON('/posts'));
        // Impure calling code
        //=====================
        blog({}).fork(
          function(error) {
            $("#error").html(error.message);
          },
          function(page) {
            $("#main").html(page);
          }
        );
        $('#spinner').show();
      note: Task also has Either like behavior
      ex: use of IO, Either together with Task
        // Postgres.connect :: Url -> IO DbConnection
        // runQuery :: DbConnection -> ResultSet
        // readFile :: String -> Task Error String
        // Pure application
        //=====================
        //  dbUrl :: Config -> Either Error Url
        var dbUrl = function(c) {
          return (c.uname && c.pass && c.host && c.db)
            ? Right.of('db:pg://'+c.uname+':'+c.pass+'@'+c.host+'5432/'+c.db)
            : Left.of(Error('Invalid config!'));
        }
        //  connectDb :: Config -> Either Error (IO DbConnection)
        var connectDb = compose(map(Postgres.connect), dbUrl);
        //  getConfig :: Filename -> Task Error (Either Error (IO DbConnection))
        var getConfig = compose(map(compose(connectDb, JSON.parse)), readFile);
        // Impure calling code
        //=====================
        getConfig('db.json').fork(
          logErr('couldn\'t read file'), either(console.log, map(runQuery))
        );

A Spot of Theory

![ss-105.png](/assets/img/ss-105.png)

![ss-106.png](/assets/img/ss-106.png)

      functors satisfy some laws
      functor laws
        // identity
        map(id) === id;
        // composition
        compose(map(f), map(g)) === map(compose(f, g));
      ex: validate laws
        var idLaw1 = map(id);
        var idLaw2 = id;
        idLaw1(Container.of(2));
        //=> Container(2)
        idLaw2(Container.of(2));
        //=> Container(2)
        # composition
        var compLaw1 = compose(map(concat(' world')), map(concat(' cruel')));
        var compLaw2 = map(compose(concat(' world'), concat(' cruel')));
        compLaw1(Container.of('Goodbye'));
        //=> Container(' world cruelGoodbye')
        compLaw2(Container.of('Goodbye'));
        //=> Container(' world cruelGoodbye')
      /Users/mertnuhoglu/Dropbox/public/img/ss-105.png
      the diagram commutes
        different paths mean different behavior
        but they end at the same type
        this formalism let us reason about our code
          without parsing and examining each scenario
      ex: commuting paths
        /Users/mertnuhoglu/Dropbox/public/img/ss-106.png
        //  topRoute :: String -> Maybe String
        var topRoute = compose(Maybe.of, reverse);
        //  bottomRoute :: String -> Maybe String
        var bottomRoute = compose(map(reverse), Maybe.of);
        topRoute('hi');
        // Maybe('ih')
        bottomRoute('hi');
        // Maybe('ih')
      we can refactor based on these properties
      ex: functors can stack
        var nested = Task.of([Right.of('pillows'), Left.of('no sleep for you')]);
        map(map(map(toUpperCase)), nested);
        // Task([Right('PILLOWS'), Left('no sleep for you')])
      note: map peels back (unnests) each layer
        and runs a function
        then wraps again
        there is no callback, if/else, for loop
        just an explicit context
      ex:
        var Compose = function(f_g_x) {
          this.getCompose = f_g_x;
        };
        Compose.prototype.map = function(f) {
          return new Compose(map(map(f), this.getCompose));
        };
        var tmd = Task.of(Maybe.of('Rock over London'));
        var ctmd = new Compose(tmd);
        map(concat(', rock on, Chicago'), ctmd);
        // Compose(Task(Maybe('Rock over London, rock on, Chicago')))
        ctmd.getCompose;
        // Task(Maybe('Rock over London'))
    Summary
      important functors not covered yet
        iterable data structures
          trees, lists, maps, pairs
        event streams, observables
        some for encapsulation, type modelling

## Chapter 9: Monadic Onions

    Pointy Functor Factory
      of method:
        it is not here to avoid new keyword
        but rather to place values in 
          a default minimal context
        it doesn't replace constructor
          it is part of an interface called Pointed
      pointed functor
        is a functor with an of method
        goal: drop any value in the type and start mapping
      ex:
        x
        IO.of('tetris').map(concat(' master'));
        // IO('tetris master')
        Maybe.of(1336).map(add(1));
        // Maybe(1337)
        Task.of({
          id: 2,
        }).map(_.prop('id'));
        // Task(2)
        Either.of('The past, present and future walk into a bar...').map(
          concat('it was tense.')
        );
        // Right('The past, present and future walk into a bar...it was tense.')
      note: IO, Task expect a function as arg
        but Maybe and Either do not
        of() allows consistent way to place a value into our functor
          IO.of = function(x) {
            return new IO(function() {
              return x;
      default minimal context
        ie. lift any volue in our type
        and map away
      note: Left.of has no sense
        Either.of is reasonable
    Mixing Metaphors
      ex: monads are like onions
        // Support
        // ===========================
        var fs = require('fs');
        //  readFile :: String -> IO String
        var readFile = function(filename) {
          return new IO(function() {
            return fs.readFileSync(filename, 'utf-8');
          });
        };
        //  print :: String -> IO String
        var print = function(x) {
          return new IO(function() {
            console.log(x);
            return x;
          });
        };
        // Example
        // ===========================
        //  cat :: String -> IO (IO String)
        var cat = compose(map(print), readFile);
        cat('.git/config');
        // IO(IO('[core]\nrepositoryformatversion = 0\n'))
      @mine: why nested IO now, but not before?
        because we compose f g where both return IO
        in previous examples, only one of them return IO
          ex:
            //  $ :: String -> IO [DOM]
            // head :: [a] -> a
            $('#myDiv').map(head)
          ex: now
            //  readFile :: String -> IO String
            //  print :: String -> IO String
            //  cat :: String -> IO (IO String)
            var cat = compose(map(print), readFile);
      note: IO is nested
        because print introduced a second IO
        we need map(map(f))
          and to observe the effect
          unsafePerformIO().unsafePerformIO()
      ex
        //  cat :: String -> IO (IO String)
        var cat = compose(map(print), readFile);
        //  catFirstChar :: String -> IO (IO String)
        var catFirstChar = compose(map(map(head)), cat);
        catFirstChar(".git/config");
        // IO(IO("["))
      ex
        //  safeProp :: Key -> {Key: a} -> Maybe a
        var safeProp = curry(function(x, obj) {
          return new Maybe(obj[x]);
        });
        //  safeHead :: [a] -> Maybe a
        var safeHead = safeProp(0);
        //  firstAddressStreet :: User -> Maybe (Maybe (Maybe Street) )
        var firstAddressStreet = compose(
          map(map(safeProp('street'))), map(safeHead), safeProp('addresses')
        );
        firstAddressStreet({
          addresses: [{
            street: {
              name: 'Mulburry',
              number: 8402,
            },
            postcode: 'WC2N',
          }],
        });
        // Maybe(Maybe(Maybe({name: 'Mulburry', number: 8402})))
      join: to unnest one layer if two layers of the same type
        var mmo = Maybe.of(Maybe.of('nunchucks'));
        // Maybe(Maybe('nunchucks'))
        mmo.join();
        // Maybe('nunchucks')
        var ioio = IO.of(IO.of('pizza'));
        // IO(IO('pizza'))
        ioio.join();
        // IO('pizza')
        var ttt = Task.of(Task.of(Task.of('sewers')));
        // Task(Task(Task('sewers')));
        ttt.join();
        // Task(Task('sewers'))
      monads
        pointed functors that can flatten
      code: join
        Maybe.prototype.join = function() {
          return this.isNothing() ? Maybe.of(null) : this.__value;
        }
      ex
        //  join :: Monad m => m (m a) -> m a
        var join = function(mma) {
          return mma.join();
        };
        //  firstAddressStreet :: User -> Maybe Street
        var firstAddressStreet = compose(
          join, map(safeProp('street')), join, map(safeHead), safeProp('addresses')
        );
        firstAddressStreet({
          addresses: [{
            street: {
              name: 'Mulburry',
              number: 8402,
            },
            postcode: 'WC2N',
          }],
        });
        // Maybe({name: 'Mulburry', number: 8402})
      code: IO.join
        x
        IO.prototype.join = function() {
          var thiz = this;
          return new IO(function() {
            return thiz.unsafePerformIO().unsafePerformIO();
          });
        };
      ex: using
        //  log :: a -> IO a
        var log = function(x) {
          return new IO(function() {
            console.log(x);
            return x;
          });
        };
        //  setStyle :: Selector -> CSSProps -> IO DOM
        var setStyle = curry(function(sel, props) {
          return new IO(function() {
            return jQuery(sel).css(props);
          });
        });
        //  getItem :: String -> IO String
        var getItem = function(key) {
          return new IO(function() {
            return localStorage.getItem(key);
          });
        };
        //  applyPreferences :: String -> IO DOM
        var applyPreferences = compose(
          join, map(setStyle('#main')), join, map(log), map(JSON.parse), getItem
        );
        applyPreferences('preferences').unsafePerformIO();
        // Object {backgroundColor: "green"}
        // <div style="background-color: 'green'"/>
    My chain hits my chest
      note: pattern: we call join after a map
        abstract this into a function chain
      code: chain
        //  chain :: Monad m => (a -> m b) -> m a -> m b
        var chain = curry(function(f, m){
          return m.map(f).join(); // or compose(join, map(f))(m)
        });
      chain = bind = >>= = flatMap
      ex
        // map/join
        var firstAddressStreet = compose(
          join, map(safeProp('street')), join, map(safeHead), safeProp('addresses')
        );
        // chain
        var firstAddressStreet = compose(
          chain(safeProp('street')), chain(safeHead), safeProp('addresses')
        );
        // map/join
        var applyPreferences = compose(
          join, map(setStyle('#main')), join, map(log), map(JSON.parse), getItem
        );
        // chain
        var applyPreferences = compose(
          chain(setStyle('#main')), chain(log), map(JSON.parse), getItem
        );
      chain is more than syntactic sugar
        it is more of tornado than a vacuum
        because we can capture
          both sequence and variable assignment
          in functional way
      ex
        // getJSON :: Url -> Params -> Task JSON
        // querySelector :: Selector -> IO DOM
        getJSON('/authenticate', {
            username: 'stale',
            password: 'crackers',
          })
          .chain(function(user) {
            return getJSON('/friends', {
              user_id: user.id,
            });
          });
        // Task([{name: 'Seimith', id: 14}, {name: 'Ric', id: 39}]);
        querySelector('input.username').chain(function(uname) {
          return querySelector('input.email').chain(function(email) {
            return IO.of(
              'Welcome ' + uname.value + ' ' + 'prepare for spam at ' + email.value
            );
          });
        });
        // IO('Welcome Olivia prepare for spam at olivia@tremorcontrol.net');
        Maybe.of(3).chain(function(three) {
          return Maybe.of(2).map(add(three));
        });
        // Maybe(5);
        Maybe.of(null).chain(safeProp('address')).chain(safeProp('street'));
        // Maybe(null);
      note: we could use compose as well
        but this style needs explicit variable assignment
        infix version of chain is faster
      chain can be derived
        t.prototype.chain = function(f) { return this.map(f).join(); }
      we can derive 
        map 
          if we have chain simply by boxing with of
        join
          as chain(id)
        other derivations: fantasyland repo
          specifcation for algebraic data types in js
    Power trip
      confusing
        how many containers deep?
        do we need map or chain?
        improve debugging with inspect

Theory

![](/assets/img/ss-107.png)

![](/assets/img/ss-108.png)

      associativity
        compose(join, map(join)) == compose(join, join);
        /Users/mertnuhoglu/Dropbox/public/img/ss-107.png
      second law: triangle identity
        // identity for all (M a)
        compose(join, of) === compose(join, map(of)) === id
        /Users/mertnuhoglu/Dropbox/public/img/ss-108.png
      category laws for monad
        var mcompose = function(f, g) {
          return compose(chain(f), g);
        };
        // left identity
        mcompose(M, f) == f;
        // right identity
        mcompose(f, M) == f;
        // associativity
        mcompose(mcompose(f, g), h) === mcompose(f, mcompose(g, h));
      monads form a category
        Kleisli category
        all objects are monads
        morphisms are chained functions

## Chapter 10: Applicative Functors

    Applying Applicatives
      ability to apply functors to each other
      ex
        // we can't do this because the numbers are bottled up.
        add(Container.of(2), Container.of(3));
        //NaN
        // Let's use our trusty map
        var container_of_add_2 = map(add, Container.of(2));
        // Container(add(2))
        ref
          //  map :: Functor f => (a -> b) -> f a -> f b
          var map = curry(function(f, any_functor_at_all) {
            return any_functor_at_all.map(f);
          });
        # we have a Container with partially applied add(2) function inside
          we want to apply add(2) to Container(3)
          f (a -> b) -> f a -> f b
    Ships in bottles
      ap: a function that can apply function contents of one functor to the value contents of another
      ex
        Container.of(add(2)).ap(Container.of(3));
        // Container(5)
        // all together now
        Container.of(2).map(add).ap(Container.of(3));
        // Container(5)
      code: ap
        Container.prototype.ap = function(other_container) {
          return other_container.map(this.__value);
        }
      applicative functor
        is a pointed functor with ap method
      property
        F.of(x).map(f) == F.of(f).ap(F.of(x))
        # mapping f =
          aping a functor of f
        # box x and map(f) =
          lift both f and x and ap them
      allows us to write in left-to-right style
        Maybe.of(add).ap(Maybe.of(2)).ap(Maybe.of(3));
        // Maybe(5)
        Task.of(add).ap(Task.of(2)).ap(Task.of(3));
        // Task(5)
    Coordination Motivation
      ex
        // Http.get :: String -> Task Error HTML
        var renderPage = curry(function(destinations, events) { /* render page */  });
        Task.of(renderPage).ap(Http.get('/destinations')).ap(Http.get('/events'))
        // Task("<div>some page with dest and events</div>")
      ex
        // Helpers:
        // ==============
        //  $ :: String -> IO DOM
        var $ = function(selector) {
          return new IO(function() { return document.querySelector(selector)});
        }
        //  getVal :: String -> IO String
        var getVal = compose(map(_.prop('value')), $);
        // Example:
        // ===============
        //  signIn :: String -> String -> Bool -> User
        var signIn = curry(function(username, password, remember_me) { /* signing in */  })
        IO.of(signIn).ap(getVal('#email')).ap(getVal('#password')).ap(IO.of(false));
        // IO({id: 3, email: "gg@allin.com"})
    Bro, do you even lift?
      pointfree way to write ap calls
        since we know map is equal to of/ap
      code
        x
        var liftA2 = curry(function(f, functor1, functor2) {
          return functor1.map(f).ap(functor2);
        });
        var liftA3 = curry(function(f, functor1, functor2, functor3) {
          return functor1.map(f).ap(functor2).ap(functor3);
        });
        //liftA4, etc
      ex 
        liftA2(add, Maybe.of(2), Maybe.of(3));
        // Maybe(5)
        liftA2(renderPage, Http.get('/destinations'), Http.get('/events'))
        // Task("<div>some page with dest and events</div>")
        liftA3(signIn, getVal('#email'), getVal('#password'), IO.of(false));
        // IO({id: 3, email: "gg@allin.com"})
    Operators
      in haskell there are infix operators
      ex
        -- haskell
        add <$> Right 2 <*> Right 3
        // JavaScript
        map(add, Right.of(2)).ap(Right.of(3))
      <$> is map
      <*> is ap
    Free Can Openers
    Laws
      applicatives are closed under composition
        ap never changes container types
      ex
        var tOfM = compose(Task.of, Maybe.of);
        liftA2(liftA2(_.concat), tOfM('Rainy Days and Mondays'), tOfM(' always get me down'));
        // Task(Maybe('Rainy Days and Mondays always get me down'))
    Identity
      identity law
        A.of(id).ap(v) == v
      ex
        var v = Identity.of("Pillow Pets");
        Identity.of(id).ap(v) == v
