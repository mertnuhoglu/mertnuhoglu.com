---
title: "Notes from Book: Introduction to Functional Programming - Philip Wadler"
date: 2017-11-24T10:10:14+03:00
draft: false
description: ""
tags: ["book_notes", "functional_programming"]
categories: ["software", "functional_programming"]
type: post
url:

---

This is an old book but it is one of the best programming books ever written.

<!--more-->

<!-- toc -->

## ch01: Fundamental Concepts

### 1.1 Functional Programming

    programming in fp =
      building definitions and evaluating expressions
    programmer's role
      construct a function to solve a problem
    computer's role
      act as an evaluator (calculator)
    expressions
      contain instances of names of functions
      evaluated by using definitions as simplification (reduction)
    feature of fp
      order of evaluation doesn't affect outcome =
        meaning of expression is its value
        computer's role: to obtain it
      thus: expressions can be constructed, reasoned about using algebraic laws
        like other mathematical expressions
    result: a conceptual framework
      very simple, concise, flexible and powerful

#### 1.1.1 Sessions and scripts

    session: sequence of interactions bw user and computer
    building definitions
      script: a list of definitions
        ex
          square x = x * x
          min x y = x, if x <= y
            = y, if x > y
        two functions: min, square
        square: x as argument
        ex
          square (min 3 4) =
          square 3 =
          9
        purpose of definition:
          to introduce a binding between a name and a value
            ex: name: square, value: function that squares its argument
        environment (context): set of bindings
          expressions 
            evaluated in some context
            contain instances of names found in that context
          evaluator
            uses definitions of these names
            to simplify expressions

### 1.2 Expressions and values

    most important feature of mathematical notation:
      referential transparency
      expression is used only to describe a value
        there are no other effects
      value of expression depends only on values of its subexpressions
      variables: names that stand for unknown values

#### 1.2.1 Reduction

    evaluation = simplification = reduction
    => means "reduces to"
    ex
      square (3+) => square 7
      => 7 * 7
      => 49
      49 cannot be further reduced
        printed by computer
    canonical: expression is canonical if it cannot be further reduced
    @mine: concept map 
      [definition] consists of -> n [line]
      [line] -<>1 [expression]
      [line] -<>1 [guard]
      [definition] ^- [conditional_definition]
      [definition] introduces -> 1 [binding]
      [definition] has inner -> 1 [context]
      [context] has -> n [definition]
      [binding] -<>1 [name]
      [binding] -<>1 [value]
      [expression] contain instances of -> n [name]
      [evaluator] uses [definition] of [name] to simplify [expression]
      [expression] means -> [value]
      [variable] is [name] with unknown [value]

### 1.3 Types

    types: set of values
    2 kinds of types
      basic types: values are given as primitive
      compound types: values are constructed from other types
    strong typing:
      every expression can be assigned a type
        that can be deduced from the constituents alone
      consequence:
        any expression that cannot be assigned a type
        is not well-formed
      ex: not well-formed definition
        ay x = 'A'
        bee x = x + ay x
    2 stages of analysis before evaluation
      syntax: does expression conform to correct syntax
      type: does expression have sensible type
    @mine: concept map
      [type] is a set of -> n [value]
      [expression] has -> [type]
      [type] of [expression] can be deduced from its sub [expression]
      [type] ^- [basic_type]
      [type] ^- [compound_type]
      [compound_type] constructed from other -> n [type]

### 1.4 Functions and definitions

    most important value kind: function value
    function f:
      a rule of mapping with each element of type A to a unique element of type B
      source type: A
      target type: B
      f::A->B
    functions are values exactly as other values

#### 1.4.1 Type information

    ex:
      square:: num -> num
        type information of function
      square x = x * x
        definition of function
    type information is optional
      can be inferred from its defining equation alone
    very general source/target types
      ex
        id x = x
      type variables: α -> α 
        α denotes a type variable
    polymorphic type: id has polymorphic type
      expression contains type variables

#### 1.4.2 Forms of definition

    case analysis
      ex
        min x y = x, if x<=y
          = y, if x>y
      two expressions:
        actual function definition: = x
        guard: x <= y
      local definition: 
        ex:
          f(x,y) = a +1 where a = x / 2
        where: introduces a local definition
          whose context (scope) is rhs of definition of f

#### 1.4.3 Currying

    ex:
      min x y = x, if x<=y
        = y, if x>y
      min' (x,y) = x, if x<=y
        =y, if x>y
      difference between min and min':
        min: takes two arguments one at a time
        min': takes a single argument which is a structured value consisting of a pair of numbers
      min'::(num,num) -> num
      min::num->(num->num)
        (min x) is a function which takes y
    ex:
      (add 1): successor function
      (add 0): identity function
    currying: replacing structured arguments by a sequence of simple ones
    benefits of currying
      reduces number of brackets to be written in expressions
    associates to left:
      operation of functional application
      ex:
        min x y 
          means: (min x) y
          not: min (x y)
    @mine: concept map
      [function] is a rule of [mapping] between elements of [type] A and [type] B
      [function] is a -> [value]
      [function] is a kind of [value]
      [expression] is a -> [value]
      [type_variable]
      [polymorphic_type] is a -> [type]
      [expression] has [polymorphic_type] if it contains [type_variable]
      [currying] replaces multiple [argument] one by one
      [functional_application] is a kind of -> [operation]
      [operation] has -> [associativity]
      [left_associative] is a kind of -> [associativity]
      [right_associative] is a kind of -> [associativity]
      [specification] is a description of a [program]
      [implementation] is a [program] that satisfies [specification]

### 1.5 Specifications and implementations

    specification: mathematical description of the task a program is to perform
    implementation: a program that satisfies the specification
    different in nature
      specs: expressions of programmer's intent or client's expectation
        purpose: to be brief and clear 
      impl: expressions for execution by computer
        purpose: to be efficient
    link between the two: implementation satisfies the specification
      programmer: provide a proof that this is the case
    ex
      spec:
        increase x > square x for all x >= 0
          result of increase should be greater than square of its argument
      impl:
        increase x = square (x + 1)

## ch02: Basic Data Types

### 2.1 Numbers

#### 2.1.1 Precedence

    higher precedence first:
      functional application
      ^
      x / div mod
      + -
    ex:
      square 3 * 4 =
      (square 3) * 4

#### 2.1.2 Order of association

    functional application:
      operator denoted by a space
        associates to the left
          ex:
            5-4-2 =
            (5-4)-2
    associates to the right
      exponentiation:
        3^4^5 =
        3^(4^5)
      function type operator:
        α -> β -> γ =
        α -> (β -> γ)
    associative: something different
      an operator ⊕ is associative if:
        (x ⊕ y) ⊕ z = x ⊕ (y ⊕ z)
      + and x are associative, ^ not
      then order of association has no effect

#### 2.1.3 div and mod

    (+):: num -> num -> num
    section: bracketed operator
      converts it to ordinary prefix function
      (+) x y = x + y
    bracketed operators can be used in expressions
      both f x = f x x
      both (+) 3 = (+) 3 3 = 3 + 3 = 6
      double = both (+)
    section: an argument can also be enclosed with operator
      ex
        (x⊕) y = x ⊕ y
        (⊕x) y = y ⊕ x

#### 2.2.2 The logical operators
    
    ∧ conjunction (and)
    ∨ disjunction (or)
    ¬ negation (not)
    ex: leap year
      must be divisible by 4
      if divisible by 100, then it must be divisible by 400
      opt1:
        leap y = (y mod 4 = 0) ∧ (y mod 100 ≠ 0 ∨ y mod 400 = 0)
      opt2: by cases
        leap y = (y mod 4 = 0) , if y mod 100 = 0
          = (y mod 400 = 0), otherwise
    ex: do the lines form a triangle
      analyse a b c = 0, if a + b ≤ c
        = 1, if a + b > c ∧ a = c
        = 2, if a + b > c ∧ a ≠ c ∧ (a = b ∨ b = c)
    ex: undefined value 
      if no guard evaluates to True
    all alternatives in a conditional definition must have same type
    @mine: concept map: operators
      operators:
        functional application (space)
        arithmetic
        function type ->
      [operator] is [associative] 
        if (x ⊕ y) ⊕ z = x ⊕ (y ⊕ z)
      [operator] is a [infix_function]
      [bracketed_operator] is a [prefix_function]
        (+) x y = x + y
      [section] is a [curried_function]
        (x⊕) y = x ⊕ y
      [undefined_value] is a [value]
      [expression] has [undefined_value] if no [guard] evaluates to True

### 2.3 Characters and strings

#### 2.3.1 Strings

    list of characters

#### 2.3.2 Layout

    to produce tables, pictures, formatted text
      show:: α -> string
      show 42 = "42"
    justification functions
      ljustify, cjustify, rjustify :: num -> string -> string

### 2.4 Tuples

    ex:
      quotrem :: (num,num) -> (num,num)
      quotrem (x,y) = (x div y, x mod y)

#### 2.4.1 Example: rational arithmetic

### 2.5 Patterns

    ex:
      defined by patterns
        cond True x y = x
        cond False x y = y
      equivalent to
        cond p x y = x, if p = True
          = y, if p = False
    ex:
      by pattern
        permute 0 = 1
        permute 1 = 2
        permute 2 = 0
      equivalent to
        permute n = 1, if n = 0
          = 2, if n = 1
          = 0, if n = 2
    patterns containing variables
      ex:
        pred 0 = 0
        pred (n + 1) = n

### 2.6 Functions

    higher-order function: function which takes a function as argument, or delivers one as result

#### 2.6.1 Functional composition

    h x = f(g x)
    (f · g) x = f(g x)
    (·)::(β -> γ) -> (α -> β) -> (α -> γ)
    associative operation
      (f·g)·h = f·(g·h)
      thus no need to put in brackets when writing sequences of compositions
    benefit: some definitions become shorter
      soln x = f1 (f2 (f3 x))
      same as:
      soln = f1 · f2 · f3

#### 2.6.2 Operators

    operators: functions used in infix form
    all non-primitive operators 
      associate to the right
      have the same binding power
    when to define a function as operator?
      if operator is associative
      ex
        x ⊕ y ⊕ z
        same as
        radd (radd x y) z
        radd x (radd y z)

#### 2.6.3 Inverse Functions

    suppose for f::A->B
      distinct values in A are mapped to distinct values in B
      f x = f y iff x = y
      injective: such a function is injective
    inverse of f: f⁻¹
      f⁻¹(f x) = x
    ex:
      f:: num -> (num, num)
      f x = (sign x, abs x)
      f⁻¹(s,a) = s * a
    common in mathematical specifications: specify a function f by requirement that it is inverse of a given function g known to be injective
      ex:
        sqrt (square x) = x
      this gives no hint as to how to compute
        not executable (constructive) definition
    surjective: for every y in B there exists some x in A st f x = y
      f(f⁻¹y) = y
    bijective: f is both injective and surjective

#### 2.6.4 Strict and non-strict functions

    strict if f NaN = NaN
    ex:
      three :: num -> num
      three x = 3
      evaluations
        three (1/0) = 3
    three NaN ≠ NaN
    non-strict semantics is preferable to strict
      reasons
        reasoning about equality easier
        we can define new control structures by defining new functions
          ex
            cond p x y = x, if p
              = y, otherwise
            normal definition:
              recip x = 0, if x = 0
                = 1/x, otherwise
            instead use non-strict definition:
              recip x = cond (x = 0) 0 (1/x)
            evaluation:
              recip 0 = cond (0 = 0) 0 (1/0) = cond True 0 NaN = 0
            with strict semantics:
              recip 0 = cond (0 = 0) 0 (1/0) = cond True 0 NaN = NaN
      tuples are non-strict:
        (x,y)
        fst(1+0, 1/0) = 1
      eager vs. lazy evaluation:
        fst(x,y)
        eager
          evaluate x, y first
        lazy
          evaluate fst first

### 2.7 Type synonyms

    sometimes it is inconvenient to declare types of functions
    type synonym: enables programmer to introduce a new name for a given type
    ex:
      position == (num,num)
      angle == num
      distance == num
      definition of move:
        move :: distance -> angle -> position -> position
        move d a (x,y) = (x + d * cos a,y + d * sin a)
    ex:
      string == [char]
    can be generic: parameterised by type variables
      ex:
        pairs α == (α,α)
        automorph α == α -> α

### 2.8 Type inference

    (·)f g x = f(g x)
    f :: t1
    g :: t2
    x :: t3
    f(g x) :: t4
    (·) :: t1 -> t2 -> t3 -> t4
    three rules
      application rule
        if f x :: t, then x::t' and f::t'->t for some new type t'
      equality rule
      function rule
    @mine: concept map
      [definition] - n [pattern]
      [function] ^- [higher_order_function]
      [higher_order_function] has a [function] as [argument] or returns one
      [functional_composition] is [associative]
      nonprimitive [operator] are [right_associative]
      [function] is [injective] 
        if f x = f y iff x = y
      [inverse] of a [function]
      [function] is [surjective] 
        if ∀ y ∈ B => ∃ x ∈ A: f x = y
      [bijective] if both [injective] and [surjective]
      [strict] 
        if f Na = Na
      [nonstrict] 
        if f Na ≠ Na
      [nonstrict] is preferable to [strict]
      [tuple] are [nonstrict]
      [eager] vs. [lazy] evaluation
      [type_synonym] is a new [name] for a given [type]
      [type_synonym] can be [generic]
      [generic] means that it can be parameterised by [type_variable]

## ch03: Lists

### 3.1 List notation

### 3.2 List comprehensions

    uses a syntax adapted from conventional mathematics for describing sets
      [〈expression〉| 〈qualifier〉;...;〈qualifier〉]
        〈qualifier〉is either a boolean valued expression or a generator
        〈variable〉<- 〈list〉
      ex:
        ? [x*x|x <- [1..10]; even x]
        [4,16,36,64,100]
        ? [(a,b)| a <- [1..3]; b <- [1..2]]
        [(1,1),(1,2),(2,1)..]
      multiple generators: 
        later generators vary more quickly than their predecessors
        later generators can depend on earlier ones
      ex:
        ?[(i,j)| i<-[1..2]; j <-[i+1..2]]
      we can freely add generators with boolean valued expressions
      ex:
        ?[(i,j)| i<-[1..2]; even i; j <-[i+1..2]]
      ex: using comprehensions in other comprehensions
        pairs = [(i,j)| i<-[1..2]; even i; j <-[i+1..2]]
        [i+j | (i,j) <- pairs]
      ex: using list comprehensions in definitions
        spaces n = [' ' | j <- [1..n]]
      ex: greatest common divisor
        divisors n = [d | d <- [1..n]; n mod d = 0]
        gcd a b = max[d | d <- divisors a; b mod d = 0]
      ex: whether a number is prime
        prime n = (divisors n = [1,n])

### 3.3 Operations on lists

    concatenation: ++
      (++) :: [α] -> [α] -> [α]
      associative operations
      has identity element: []
    naming conventions:
      x: element
      xs: list of x
      xss: list of xs
    concat function: concatenates a list of lists into one long list
      concat :: [[α]] -> [α]
      concat xss = [x | xs <- xss; x <- xs]
    length: #
      (#)::[α]->num
    head and tail: hd, tl
      hd: first element
      tl: select remaining portion
      hd([x]++xs) = x
      tl([x]++xs) = xs
      hd[] = NaN
      hd :: [α]->α
      tl :: [α] -> [α]
      relationship between hd and tl:
        xs = [hd xs] ++ tl xs
    init and last: break a list at the end
      init(xs++[x]) = xs
      last(xs++[x]) = x
    take and drop: 
      ?take 3 [1..10]
      [1,2,3]
      ?drop 3 [1..10]
      [4,5,6,7,8,9,10]
      take n xs ++ drop n xs = xs
    takewhile and dropwhile
    reverse
    zip
      zip ::([α],[β]) -> [(α,β)]
      ?zip([0..2],"hel")
      [(0,'h'),..]
      use cases:
        scalar product: vectors x and y
          sp(xs,ys) = sum[x*y|(x,y)<-zip(xs,ys)]
          alternative definition:
            sp = sum · zipwith(*)
            zipwith f (xs,ys) = [f x y|(x,y) <- zip(xs,ys)]
        non-decreasing sequences: nondec function determines whether a sequence is in non-decreasing order
          nondec xs = and[x≤y|(x,y) <- zip(xs, tl xs)]
        position: position function returns position of first occurrence of x in xs
          position :: [α] -> α -> num
          positions xs x = [i | (i,y) <- zip([0..#xs-1],xs); x = y]
          position xs x = hd(positions xs x ++ [-1])
    list indexing: !
      (!)::[α]->num->α
      define (!) with zip
        xs ! n = hd[y|(i,y) <- zip([0..#xs-1],xs); i = n]
      define nondec with (!)
        nondec xs = and[xs!k≤xs!(k+1)|k<-[0..#xs-2]]
    list-difference: --
      ?[1,2,3,1]--[1,3]
      [2,1]
      concatenation and list-difference are related:
        (xs ++ ys) -- xs = ys
        tl xs = xs -- [hd xs]
        drop n xs = xs -- take n xs
      we can define the condition that one list is a permutation of another list:
        permutation xs ys = (xs -- ys = []) ∧ (ys -- xs = [])
    @mine: concept map
      concatenation: ++
        (++) :: [α] -> [α] -> [α]
      concat: 
        concat :: [[α]] -> [α]
        concat xss = [x | xs <- xss; x <- xs]

### 3.4 Map and filter

    map: applies a function to each element of a list
      map :: (α->β) -> [α] -> [β]
      map f xs = [f x | x <- xs]
    benefit of map: very simple and direct description
      sum of squares of first 100 positive integers
      sum(map square [1..100])
      ex: mathematical notation of sigma
        sigma f m n = sum(map f [m..n])
        sumsquares = sigma square 1 100
    useful algebraic identities concerning map:
      map(f·g) = (map f)·(map g)
        if we apply g to every element of a list, and then apply f to each element of the result, then the same effect is obtained by applying (f·g) to original list
        ie. map distributes through functional composition
        consequence: if f has an inverse f⁻¹:
          (map f)⁻¹ = map f⁻¹
    filter: takes a predicate p and a list xs and returns sublist xs whose elements satisfy p
      define by list comprehension:
        filter :: (α->bool) -> [α] -> [α]
        filter p xs = [x|x<-xs; p x]
      ex:
        filter even [1,4,3]
        (sum · map square · filter even) [1..10]
      benefits:
        concise and simple: sums of squares of even numbers in range 1 to 10
          (sum · map square · filter even) [1..10]
      useful identities:
        filter p (xs ++ ys) = filter p xs ++ filter p ys
        filter p · concat = concat · map (filter p)
        filter p · filter q = filter q · filter p
        first law: filter distributes through concatenation
        second law: generalises this distributive property to lists of lists
        third law: filters can be applied in any order
      ex: second law
        xss = [[1,2],[4,3]]
        p = even
        filter p · concat xss = filter p [1,2,4,3]
          = [2,4]
        concat · map (filter p) xss = concat [[2],[4]]
          = [2,4]
    translating comprehensions:
      there is a close relationship between functions map and filter and notation of list comprehensions
      we can define list comprehensions in terms of map, filter and concat
      4 rules for conversion:
        (1) [x| x <- xs] = xs
        (2) [f x| x <- xs] = map f xs
        (3) [e| x <- xs; p x; ...] = [e| x <- filter p xs; ...]
        (4) [e| x <- xs; y <- ys; ...] = concat [[e| y <- ys; ...] | x <- xs]
    ex: translate: [1| x <- xs]
      introduce function const
        const k x = k
      [1| x <- xs] = [const 1 x| x <- xs]
        = map (const 1) xs
    ex: translate: [x| x <- xs; x = min xs]
      introduce:
        minof xs x = (x = min xs)
      [x| x <- xs; x = min xs] 
        = [x| x <- xs; minof xs x]
        = [x| x <- filter (minof xs) xs]
        = filter (minof xs) xs
    ex: translate: [x| xs <- xss; x <- xs]
      [x| xs <- xss; x <- xs]
        = concat [[x | x <- xs] | xs <- xss]  '(4)
        = concat [xs | xs <- xss]             '(1)
        = concat xss                          '(1)
    ex: translate: [(i,j)|i <- [1..n]; j <- [i+1..n]]
      [(i,j)|i <- [1..n]; j <- [i+1..n]]
        = concat [[(i,j) | j <- [i+1..n]] | i <- [1..n]]  '(4)
        = concat [map (pair i) [i+1..n]] | i <- [1..n]]   '(2)
        = concat (map mpair [1..n])                       '(2)
      where
        pair i j = (i,j)
        mpair i = map (pair i)[i+1..n]
    comparison:
      list comprehensions: clear and simple
        no need to invent special names for subsidiary functions that arise with map and filter
      higher order functions:
        easier to state algebraic properties
        use these properties in the manipulation of expressions
    @mine: concept map
      [map] distributes over [composition]
        like: [multiplication] distributes over [addition]
        map(f·g) = (map f)·(map g)
        z*(x+y) = z*x + z*y

### 3.5 The fold operators

    previous operations: return lists
    fold operators: they can convert lists into other kinds of values as well
    foldr:
      foldr f a [x1, x2, ..., xn] = f x1 (f x2 (...(f xn a) ...))
      ex:
        foldr (⊕) a [x1,..,xn] = x1 ⊕ ( x2 ⊕ (...(xn ⊕ a)...))
      second arg of ⊕ must have same type as the result of ⊕
      first arg may have a different type
      foldr :: (α -> β -> β) -> β -> [α] -> β
      if first arg is of different type then foldr operator can't be associative
    using foldr we can define:
      sum = foldr (+) 0
      product = foldr (x) 1
      concat = foldr (++) []
      and = foldr (∧) True
      or = foldr (∨) False
    common in all examples: 
      function ⊕ is associative
      has identity element a
      ie.
        x ⊕ (y ⊕ z) = (x ⊕ y) ⊕ z
        x ⊕ a = x = a ⊕ x
      monoid: ⊕ and a form a monoid
    if ⊕ and a form a monoid, then:
      foldr (⊕) a [] = a
      foldr (⊕) a [x1,...,xn] = x1 ⊕ ... ⊕ xn
    ex where args to foldr is not a monoid:
      ex: #
        (#) = foldr oneplus 0
        where oneplus x n = 1 + n
        this defines the length of a list by counting one for each element
        oneplus :: α -> num -> num
        so it cannot be associative
    ex:
      reverse = foldr postfix []
        where postfix x xs = xs ++ [x]
      takewhile p = foldr (⊕) []
        where x ⊕ xs = [x] ++ xs, if p x
          = [], otherwise
      ex:
        takewhile (<3) [1..4] = 1 ⊕ (2 ⊕ (3 ⊕ (4 ⊕ [])))
          = [1] ++ ([2] ++ ([]))
          = [1,2]
    fold left: foldl
      foldl (⊕) a [x1, ..., xn] = (...((a⊕x1)⊕x2)...)⊕ xn
      ex:
        foldl (⊕) a [x1,x2] = (a ⊕ x1) ⊕ x2
      foldl :: (β->α->β)->β->[α]->β
      uses: 
        pack xs = foldl (⊕) 0 xs
          where n ⊕ x = 10 * n + x
        this codes a sequence of digits as a single number, most significant digit comes first
    difference between foldr and foldl
      Both fold-left and fold-right reduces one or more list with a reducing procedure from first to last element, but the order it is applied is kept in fold-right while it is reversed in fold-left. It's 
    @mine: concept map
      [foldr] is for right-associative [operator]
      [foldr] applies [function] in reverse order of the list argument
      [foldl] is for left-associative [operator]
      [foldl] applies [function] in same order of the list argument
      foldr :: (α -> β -> β) -> β -> [α] -> β
        second [argument] has same [type] as result of [function]
        because foldr (⊕) a [x1,..,xn] = x1 ⊕ ( x2 ⊕ (...(xn ⊕ a)...))
          a is the last argument in the open form
    @mine: mnemonics
      symmetry of reverse functions
        opt1: foldr
          reverse = foldr postfix []
            where postfix x xs = xs ++ [x]
        opt2: foldl
          reverse = foldl prefix []
            where prefix xs x = [x] ++ xs
        opt1 case
          foldr reverses order of operations on xs
            (x1 ++ (x2 ++ [])) = (x1 ++ [x2]) = [x2 x1]
          because of right association
            xs is second argument
            xs contains last first 
          postfix puts elements to the end of the resulting list
        opt2 case
          foldl keeps order of operations on xs
            (([] ++ x1) ++ x2) = ([x1] ++ x2) == [x2 x1]
          because of left association
            xs is first argument 
            xs contains first first 

#### 3.5.1 Laws

    important laws:
      three duality theorems
    first duality theorem
      foldr (⊕) a xs = foldl (⊕) a xs
      whenever ⊕ and a form a monoid and xs is a finite list
    second duality theorem
      generalisation of the first
      suppose:
        x ⊕ (y ⨂ z) = (x ⊕ y) ⨂ z
        ⊕ and ⨂ associate with each other 
        x ⊕ a = a ⨂ x
      foldr (⊕) a xs = foldl (⨂ ) a xs
      special case: (⊕) = (⨂ )
        first duality theorem follows
      uses:
        (#) = foldl plusone 0
          where plusone n x = n + 1
        reverse:
          reverse = foldl prefix []
            where prefix xs x = [x] ++ xs
          foldr postfix [] xs = foldl prefix [] xs
    third duality theorem:
      foldr (⊕) a xs = foldl (⊕') a (reverse xs)
      ⊕' is defined by:
        x ⊕' y = y ⊕ x
      ex: reversing prefix: cons function
        prefix xs x = [x] ++ xs
        cons x xs = [x] ++ xs
          thus:
          xs = foldr cons [] xs
          foldr cons [] xs = foldl prefix [] (reverse xs)
          xs = reverse (reverse xs)
      other uses:
        if ⊕ and a form a monoid:
          foldr (⊕) a (xs ++ ys) = (foldr (⊕) a xs) ++ (foldr (⊕) a ys)
        foldl f a (xs ++ ys) = foldl f (foldl f a xs) ys

#### 3.5.2 Fold over non-empty lists

    say: find max element of a list
      max = foldl (max) a
      question: what should a be?
        better if a is identity element
          if xs contains negative numbers, there is no such a 
      introduce new functions: foldl1, foldr1
        foldl1 (⊕) [x1 .. xn] = (..((x1 ⊕ x2) ⊕ x3) ..) ⊕ xn
        foldr1 (⊕) [x1, x2, ..., xn] = (x1 ⊕ ( .. (xn-1 ⊕ xn) ..))
        remember foldl:
          foldl (⊕) a [x1, ..., xn] = (...((a⊕x1)⊕x2)...)⊕ xn
        in particular:
          foldl1 (⊕) [x1] = x1
          foldl1 (⊕) [x1, x2] = x1 ⊕ x2
          foldl1 (⊕) [x1, x2, x3] = (x1 ⊕ x2) ⊕ x3
          foldr1 (⊕) [x1, x2, x3] = x1 ⊕ (x2 ⊕ x3)
        foldl1, foldr1 :: (α -> α -> α) -> [α] -> α
      max = foldl1 (max)
      defining foldl1 in terms of foldl:
        foldl1 (⊕) xs = foldl (⊕) (hd xs) (tl xs)
        foldr1 (⊕) xs = foldr (⊕) (last xs) (init xs)

#### 3.5.3 Scan

    scan: apply fold left to every initial segment of a list
    scan (⊕) a [x1, x2, ...] = [a, a ⊕ x1, (a ⊕ x1) ⊕ x2, ...]
    last value is: foldl (⊕) a xs
      thus: foldl (⊕) a = last · scan (⊕) a
    scan (+) 0 [1,2,3,4,5] = [0,1,3,6,10,15]
        
### 3.6 List patterns

    pattern matching with list arguments
    introduce: (:) cons (construct)
      (:) :: α -> [α] -> [α]
      inserts a value as a new first element of a list
      ? 1 : []
      [1]
      ? 1:2:[3,4]
      [1,2,3,4]
      (:) associates to the right
      x:xs = [x] ++ xs
      difference from ++:
        (:) every list can be expressed in exactly one way
          that is why it can do pattern matching
      hd(x:xs) = x
      tl(x:xs) = xs
      define: null test
        null[] = True
        null(x:xs) = False
      define: single if list contains a single element
        single [] = False
        single [x] = True
        single (x:y:xs) = False
        note that: [x] is abbreviation for x:[]
        note that: three cases are exhaustive and disjoint

## ch04: Examples

### 4.1 Converting numbers to words

    ex: (convert): translate number to its English formulation

### 4.2 Variable-length arithmetic

### 4.3 Text processing

### 4.4 Turtle graphics

### 4.5 Printing a calendar

## ch05: Recursion and Induction

    every natural number is
      either 0
      or (n + 1)
    every list is
      either []
      or (x:xs)
    operator (#) defined by recursion
      #[] = 0
      #(x:xs) = 1 + (#xs)
    concatenation by recursion
      [] ++ ys = ys
      (x:xs) ++ ys = x : (xs ++ ys)
    prove by induction for lists:
      P(xs) holds if
        case []. P([]) holds
        case (x:xs). if P(xs) => P(x:xs) for every x
    proof for: concatenation is associative:
      claim: xs ++ (ys ++ zs) = (xs ++ ys) ++ zs
      proof: induction on xs
        case [].
          [] ++ (ys ++ zs) = ys ++ zs
            = ([] ++ ys) ++ zs
          establishes the case
        case (x:xs).
          (x:xs) ++ (ys ++ zs) = x : (xs ++ (ys ++ zs))  (++.2)
            = x: ((xs++ys) ++ zs) (hypothesis)
            = (x : (xs ++ ys)) ++ zs (++.2)
            = ((x:xs) ++ ys) ++ zs (++.2)
    
### 5.3 Operations on lists

#### 5.3.1 Zip

    recursive definition of zip:
      zip ([], ys] = []
      zip (x:xs, []) = []
      zip (x:xs, y:ys) = (x,y) : zip (xs, ys)
    show that:
      #zip (xs,ys) = (#xs) min (#ys)
      proof.
        case [], ys.
          #zip ([],ys) = #[] (zip.1)
            = 0 (#.1)
            = 0 min (#ys)
          establishes the case
        case (x:xs), [].
        case (x:xs), (y:ys).
          ...
    
#### 5.3.5 Map and filter

    recursive definition of map:
      map f [] = []
      map f (x:xs) = f x : map f xs
    recursive filter:
      filter p [] = []
      filter p (x:xs) = x:filter p xs, if p x
        = filter p xs, other wise
    ex: filter odd (map square [2,3])
      = filter odd (4:map square [3])    (map.2)
      = filter odd (4:9)       (map.2)
      = filter odd (9)       (filter.3)
      = 9: filter odd []     (filter.2)
      = 9:[]     (filter.1)
      = [9]
    one law:
      filter p (map f xs) = map f (filter (p·f) xs)

### 5.4 Auxiliaries and generalisation

#### 5.4.1 List difference

    recursive definition of -- (list difference):
      definition:
        xs -- [] = xs
        xs -- (y:ys) = remove xs y -- ys
        remove [] y = []
        remove (x:xs) y = xs, if x = y
          = x:remove xs y, otherwise
      remove is auxiliary function for (--)

#### 5.4.2 Reverse

    definition:
      reverse [] = []
      reverse (x:xs) = reverse xs ++ [x]

#### 5.4.3 Second duality theorem

    recursive definitions: 
      foldr (⊕) a [] = a
      foldr (⊕) a (x:xs) = x ⊕ (foldr (⊕) a xs)
      foldl (⊕) a [] = a
      foldl (⊕) a (x:xs) = foldl (⊕) (a ⊕ x) xs
    second duality theorem:
      let (⊕) and (⨂) be binary operators and a a value st:
        x ⊕ (y ⨂ z) = (x ⊕ y) ⨂ z
        ⊕ and ⨂ associate with each other 
        x ⊕ a = a ⨂ x
      then:
        foldr (⊕) a xs = foldl (⨂ ) a xs

### 5.6 Combinatorial functions

    combinatorial:
      they involve selecting or permuting elements of a list in some manner
    initial segments:
      inits returns all initial segments of a list
        ex
          inits "era"
          ["", "e", "er", "era"] 
      definition
        inits [] = [[]]
        inits (x:xs) = [[]] ++ map (x :) (inits xs)
    subsequences:
      ex:
        subs "era"
        ["", "a", "r", "ra", "e", "ea", "er", "era"]
      definition:
        subs [] = [[]]
        subs (x:xs) = subs xs ++ map (x :) (subs xs)
    interleave:
      all possible ways of inserting an element into a list
      ex:
        interleave "e" "ar"
        ["ear", "aer", "are"]
      definition
        interleave x [] = [[x]]
        interleave x (y:ys) = [x:y:ys] ++ map (y :) (interleave x ys)
    permutations:
      ex:
        perms "era"
        ["era", "rea", "rae", "ear", "aer", "are"]
      definition:
        perms [] = [[]]
        perms (x:xs) = concat (map (interleave x) (perms xs))
    partitions:
      xss is a partition of xs if:
        concat xss = xs
      ex: ["era"] and ["e","ra"] are partitions of "era"
      parts: returns all partitions of a list
      ex:
        parts "era"
        [["era"], ["er","a"],...]

## ch07: Infinite Lists

    applications:
      infinite lists can be used to model sequences of events ordered in time
        ex: interactive processes

### 7.1 Infinite lists

### 7.1 Infinite lists

    [1..]
    take n [1..] = [1..n]
    [m..]!n = m + n
    map factorial [1..] = scan (*) 1 [1..]
    filter (<10) (map (^2) [1..])
    [1,4,9
      doesn't finish
    takewhile (<10) (map (^2) [1..])
    [1,4,9]

### 7.2 Iterate

    fⁿ: a function composed with itself n times
      f⁰ = id
      f¹ = f
      f² = f · f
    write fⁿ as (power f n)
    definition of power:
      power f 0 = id
      power f (n+1) = f · power f n
    iterate:
      iterate f x = [x, f x, f² x, ...]
      ex
        iterate (+1) 1 = [1,2,3,...]
        iterate (*2) 1 = [1,2,4,...]
        iterate (div10) 2718 = [
    uses
      [m..] = iterate (+1) m
      [m..n] = takewhile (≤n) (iterate (+1) m)
      digits = reverse · map (mod10) · takewhile (≠ 0) · iterate (div10)
        extract digits of a positive integer
        ex
          digits 2718 
            = reverse · map (mod10) · takewhile (≠ 0) [2718,271,27,2,0,0...]
            = reverse · map (mod10) · takewhile (≠ 0) [2718,271,27,2]
            = reverse [8,1,7,2]
            = [2,7,1,8]
      group n = map (take n) · takewhile (≠ []) · iterate (drop n)
        break a list into segments of length n
    uses with map, takewhile, iterate
      corresponds to
        while (arg != [])
          arg = drop n arg
          result += take n arg
      capture this patterns as a generic function: unfold
        unfold h p t = map h · takewhile p · iterate t
      corresponding operations:
        hd (unfold h p t x) = h x
          thus h of unfold corresponds to hd
        tl (unfold h p t x) = unfold h p t (t x)
        nonnull (unfold h p t x) = p x
    definition of iterate:
      opt1: inefficient
        iterate f x = [power f i x | i <- [0..]]
        computes each of (power f 0 x), (power f 1 x) independently
      opt2: recursive
        iterate f x = x : iterate f (f x)
        ex:
          iterate (*2) 1 = 1 : iterate (*2) ((*2) 1)
            = 1:2:iterate (*2) ((*2) 2)
        similar to scan:
          both compute each element of the output list in terms of preceding element

### 7.3 Example: generating primes

### 7.4 Infinite lists as limits

    example problem:
      filter (<10) (map (^2) [1..])
      [1:4:9:Na]
      why is this so?
        a theory of infinite lists is required
    limits: one way of dealing with infinite objects
    infinite object: limit of an infinite sequence of approximations
    ex: infinite list [1..] is limit of the following sequence:
      Na
      1:Na
      1:2:Na
      ...

### 7.5 Reasoning about infinite lists

    lists:
      partial list: any list ending in bottom
        any list of the form x1:x2:...:xn:Na
        thus: every infinite list is the limit of an infinite sequence of partial lists
      finite list: ends in []
        ex: 1:2:3:[] = [1,2,3]
      partial list: ends in Na
      infinite list: does not end at all
        ex: [1,2,3,...]
      continuity property:
        if xs1,xs2,xs3,... is an infinite sequence 
          with limit xs and f is a computable function
          then f xs1, f xs2, ... is an infinite sequence whose limit is f xs
        ex: [1..] is the limit of the sequence given above
          we can compute map (*2) [1..] as:
            map (*2) Na = Na
            map (*2) (1:Na) = 2:Na
            map (*2) (1:2:Na) = 2:4:Na
            ..
            limit of this sequence is infinite list: [2,4,6,...]
          note that:
            no equation defining map matches map (*2) Na because Na does not match either [] or (x:xs)
            this follows from case exhaustion on map
          thus we have:
            map (*2) (1:Na) = (1*2):(map (*2) Na)      (map.2)
              = 2:Na     (map.0)
            (map.0) indicate case exhaustion on map
    consider expression: filter (<10) (map (^2) [1..])
      applying filter (<10) gives sequence:
        filter (<10) Na = Na
        filter (<10) (1:Na) = 1:Na
        filter (<10) (1:4:Na) = 1:4:Na
        filter (<10) (1:4:9:Na) = 1:4:9:Na
        filter (<10) (1:4:9:16:Na) = 1:4:9:Na
        filter (<10) (1:4:9:16:25:Na) = 1:4:9:Na
        ...
        limit is: 1:4:9:Na
      applying takewhile (<10) gives sequence:
        takewhile (<10) Na = Na
        takewhile (<10) (1:Na) = 1:Na
        takewhile (<10) (1:4:Na) = 1:4:Na
        takewhile (<10) (1:4:9:Na) = 1:4:9:Na
        takewhile (<10) (1:4:9:16:Na) = 1:4:9:[]
        takewhile (<10) (1:4:9:16:25:Na) = 1:4:9:[]
        ...
        limit: 1:4:9:[]

### 7.6 Cyclic structures

    data structures can be defined recursively
    consider: ones = 1:ones
      ones = 1:ones
        = 1:1:ones
        = 1:1:1:ones
        ...
      so ones is bound to infinite list [1,1,1,...]
      memory representation:
        1:->1
    consider: more
      more = "More " ++ andmore
        where andmore = "and more " ++ andmore
    
#### 7.6.1 Forever

    let forever x be infinite list [x,x,...]
    then:
      ones = forever 1
    definition:
      opt1:
        forever x = x:forever x
        correct but doesn't create a cyclic structure
      opt2:
        forever x = zs
          where zs = x:zs
        this produces cyclic structure

#### 7.6.2 Iterate

    definition that uses cyclic structure:
      iterate f x = zs
        where zs = x : map f zs

### 7.8 Interactive programs

    interactive program:
      f::[char] -> [char]
    ex: echo user input by capitalising
      capitalises = map capitalise
      capitalise x = decode (code x + offset), if 'a' ≤ x ∧ x ≤ 'z'
        = x, otherwise
        where offset = code 'A' - code 'a'
    ex: terminating an interactive program
      capitalises' = takewhile (≠ '#') · capitalises

#### 7.8.1 Modelling interaction

    consider sequences of partial lists:
      capitalises Na = Na
      capitalises ('H':Na) = 'H':Na
      capitalises ('H':'e':Na) = 'H':'e':Na
    techniques used to model infinite lists are suitable for modelling interactive programs

#### 7.8.2 Hangman

## ch08: New Types

    3 ways to combine types:
      function space operator -> to form the type of functions 
      tupling operator (α, ..., β) to form tuples of types
      list construction operator [α] to form lists of values of a given type
    4. way: construct new types directly

### 8.1 Enumerated types

    type definition: explicit enumeration of the values of a new type:
      ex: type day:
        day ::= Sun | Mon | Tue | Wed | Thu | Fri | Sat
      ubiquitous Na: assumed to be a value of every type
      constructor function: 7 new constants of the type day
    convention
      constructor: first letter capital 
      variable, value, type: first letter small

### 8.2 Composite types

    types whose values depend on those of other types
    ex:
      tag ::= Tagn num | Tagb bool
      constructors denote a function:
        Tagn :: num -> tag
        Tagb :: bool -> tag
      ex: (Tagn 3) is a value of type tag
    differences of constructors from other functions:
      there is no definition associated with constructor functions
        they are primitive conversion functions whose role is just to change values of one type into another
      expressions of the form (Tagn n) can appear as patterns on the lhs of definitions
        ex:
          numval :: tag ->
          numval (Tagn n) = n
          boolval :: tag -> bool
          boolval (Tagb b) = b
        ex: functions discriminating between alternatives of the type
          isNum (Tagn n) = True
          isNum (Tagb b) = False
          properties:
            isNum Na = Na
            isNum (Tagn Na) = True
            isNum (Tagb Na) = False
    ex: temp
      opt1: union type
        temp ::= Celsius num | Fahrenheit num | Kelvin num
          three alternatives
          one component type: num
        comparing temparatures: requires an explicit equality test
          eqtemp :: temp -> temp -> bool
      opt2: type synonym
        temp == (tag, num)
        tag ::= Celsius | Fahrenheit | Kelvin
      opt3: non-union type with multi argument constructor
        temp ::= Temp tag num
        tag ::= Celsius | Fahrenheit | Kelvin
          temp: new type
            non-union type: has only one alternative
    ex: file type
      file ::= File [record]
        values have form (File rs) where rs is a list of records
        to delete a record from a file:
          delete :: record -> file -> file
          delete r (File rs) = File (rs -- [r])
        other file processing functions:
          empty :: file
          insert :: record -> file -> file
          merge :: file -> file -> file
        advantage of defining file as a special type:
          additional measure of security
          synonym definition:
            file == [record]
            then every operation on lists is valid for files, no protection against misuse

#### 8.2.1 Polymorphic types

    type definitions can be parameterised by type variables
    ex:
      file α ::= File [α]
      previous type file: (file record)
    ex:
      pair α β ::= Pair α β
      ex:
        Pair 3 True :: pair num bool
        Pair [3] 0 :: pair [num] num

#### 8.2.2 General form

    t α₁ α₂ ... ::= C1 t11 ... t1k1 | ... | Cn tn1 ... tnkn
      C1, ... Cn are constructor names
      tij: type expression

### 8.3 Recursive types

  much of the power of the type mechanism comes from the ability to define recursive types

#### 8.3.1 Natural numbers

    nat ::= Zero | Succ nat
    ex: values of nat
      Zero
      Succ Zero
      Succ (Succ Zero)
    every natural number is represented by a unique value of nat
    arithmetic functions can be defined in terms of nat
      n ⊕ Zero = n
      n ⊕ (Succ m) = Succ (n ⊕ m)

#### 8.3.2 Lists as a recursive type

    recursive type definition of lists:
      list α ::= Nil | Cons α (list α)
        Nil is a value of (list α), and (Cons x xs) is a value of (list α) whenever x is a value of α and xs is a value of (list α)
      ex:
        Na
        Nil
        Cons Na Nil
    values of type (list α) have 1-1 correspondence with values of type [α]
      thus: lists need not be primitive type
      partial and infinite lists:
        Na
        Cons 1 Na
        Cons 1 (Cons 2 Na)
        ...

#### 8.3.3 Arithmetic expressions as a recursive type

    different from previous examples:
      this is non-linear
    non-linear types: trees
    if e is an arithmetic expression, then either:
      e is a number; or
      e is an expression of the form: e1 ⊕ e2 
        where e1 and e2 are arithmetic expressions
        ⊕ is one of + - * /
    type definition:
      aexp ::= Num num | Exp aexp aop aexp
      aop ::= Add | Sub | Mul | Div
    two constructors: Num and Exp
    aexp is non-linear
      because aexp appears in two places
    ex:
      (3+4)
        Exp (Num 3) Add (Num 4)
      3+4*5
        Exp (Num 3) Add (Exp (Num 4) Mul (Num 5))
    convert values of type aexp to numbers:
      eval :: aexp -> num
      eval (Num n) = n
      eval (Exp e1 op e2) = apply op (eval e1) (eval e2)
      subsidiary function: apply is defined by cases:
        apply Add = (+)
        apply Sub = (-)
        apply Mul = (*)
        apply Div = (/)

### 8.4 Abstract types

    introducing a new type
      = naming its values
      except functions
    each value = unique expression in terms of constructors
    Using definition by pattern matching
      expressions can be generated
    there is no need to name the operations associated with the type
    concrete types: 
      types in which values are prescribed but the operations are not
    abstract types:
      reverse
      defined by naming its operations
      the meaning of each operation has to be described
        approach opt1: algebraic specification
          states relationships between operations as a set of algebraic laws
        approach opt2: describe each operation in terms of the most abstract representation

#### 8.4.1 Abstraction functions

    representing one class of values by another:
      abstraction function abstr
    abstr:: B -> A
    if abstr b = a, then b represents the value a
    requirement:
      abstr is surjective
        every value in A has one representation b
    ex: correspondence between [α] and (list α)
      abstr :: (list α) -> [α]
      abstr Nil = []
      abstr (Cons x xs) = x : abstr xs
    ex: between num and nat
      abstr :: nat -> num
      abstr Zero = 0
      abstr (Succ x) = abstr x + 1
      not bijective:
        every natural number has corresponding nat
    ex: mathematical sets and lists
      abstr :: [α] -> set α
      abstr [] = {}
      abstr (x:xs) = {x} ∪ abstr xs

#### 8.4.2 Valid representations

    the function abstr:: B -> A gives complete information
      about how elements of A are represented by elements of B
      in particular if (abstr b) is well-defined, then b is said to be a valid representation
    explicit predicate: valid
      determines what representations are valid
    intention:
      arguments to abstr to be restricted for which (valid x) is true
    ex: representing sets by lists
      abstr :: [α] -> set α
      abstr [] = {}
      abstr (x:xs) = {x} ∪ abstr xs
      the following 3 definitions of a valid representation:
        valid1 xs = True
        valid2 xs = nodups xs
        valid3 xs = nondec xs
      pairs (abstr, valid1), (abstr, valid2), (abstr, valid3) describe 3 different representations 

#### 8.4.3 Specifying operations

    ex: addset adds an element x to a set s
      addset :: α -> set α -> set α
      addset x s = {x} ∪ s 
      insert implements this operation in terms of lists
        insert :: α -> [α] -> [α]
      specified by the equation:
        abstr (insert x xs) = addset x (abstr xs)
      specifications are pictured as commuting diagrams
            set---------------->set 
             ^    addset x       ^
             |                   |
       abstr |                   | abstr
             |                   |
             |    insert x       |
            list--------------->list
      diagram commutes
        both ways lea to the same result
      thus we require:
        abstr · (insert x) = (addset x) · abstr
      possible implementations of insert:
        insert1 x xs = [x] ++ xs
        insert2 x xs = [x] ++ [y| y <- xs; x ≠ y]
        insert3 x xs = takewhile (<x) xs ++ [x] ++ dropwhile (≤x) xs
      implementations for:
        insert1: (abstr,valid1)
        insert2: (abstr,valid2)
        insert3: (abstr,valid3)

#### 8.4.4 Queues

    suppose (queue α) is a collection of values of type α with following operations:
      start :: queue α
      join :: queue α -> α -> queue α
      front :: queue α -> α
      reduce :: queue α -> queue α
    relationships between operations:
      front (join start x) = x
      front (join (join q x) y) = front (join q x)
      reduce (join start x) = start
    these equations constitute algebraic specification

#### 8.4.5 Arrays

    abstract type defined by the following for operations:
      mkarray :: [α] -> array α
      length :: array α -> num
      lookup :: array α -> num -> α
      update :: array α -> num -> α -> array α
    relationships between operations:
      length (mkarray xs) = #xs
      length (update A k x) = length A
      lookup (mkarray xs) k = xs ! k
      lookup (update A j x) = x, if j = k
        = lookup A k, otherwise 

## ch09: Trees

    any data-type that exhibits a non-linear (branching) structure is called a tree

### 9.1 Binary trees

    two-way branching structure
      btree α ::= Tip α | Bin (btree α) (btree α)
    ex:
      Bin (Tip 1)
        (Bin (Tip 2) (Tip 3))

#### 9.1.1 Measures on trees




