---
title: "Notes from Book: Category Theory for the Sciences - David I. Spivak"
date: 2017-11-23T22:27:24+03:00
draft: false
description: ""
tags: ["book_notes", "functional_programming"]
categories: ["software", "functional_programming"]
type: post
url:
---

My reading notes from the book. 

<!--more-->

<!-- toc -->

Note: All figures are taken from the book. 

## ch01: Introduction

![](/assets/img/ss-23.png)

    ~/Dropbox/public/img/ss-23.png
    ontology logs: ologs
      to give structure to the kinds of ideas that are often communicated in graphics
      it encompasses a database schema
    category theory
      communication of ideas between different fields in mathematics
      different branches can be formalized into categories
      categories are connected by functors
    transfer of proofs
      theorems proven in one category
      transferred through a functor to yield proofs in another category
    functor is like
      conductor of mathematical truth
    use in scientific models
      study of basic building blocks
      certain structures show up again and again
        vector spaces
        also hierarchies, daat models, self-similarity
    
### 1.1 A brief history of category theory

    invented in early 40s by Samuel Eilenberg and Saunders Mac Lane
      to bridge topology and algebra
      there were already links (such as cohomology theory)
      but they compared different links with one another precisely
      first: extract fundamental nature of these two fields
        ideas fit to other mathematical disciplines as well
    first use: clarifying lanugage for existing mathematical ideas
      1957 Grothendieck used it to build new mathematical fields: new cohomology theories
        that made insight into behavior of algebraic equations
    Bull Lawvere: foundation for all mathematical thought
      19th century: foundation was set theory
      Lawvere showed category of sets is one category but not the center of mathematical universe
    1980 Joachim Lambek: types and programs form a specific kind of category
      new semantics on how programs combine to create other programs
    Eugenio Moggi: brought monads into programming
    Daniel Kan, Andre Joyal
    language of choice for algebra
    main use: bridge

### 1.2 Intention of this book
    
    applied mathematics is much smaller than 
      applicable mathematics
    abstract conceps can be communicated to scientists
    journals: substantial experties
      it is impossible to read a methods section and repeat the experiment
    first goal: reusable methodologies can be formalized
      ct is incredibly efficient
        as a language for experimental design patterns
        researches has ability to think about models that is impossible without it
        forces a person to clarify her assumptions
      value: conceptual chaos is a major problem
        creativity demands clarity of thinking
          how pieces fit together

### 1.3 What is requested from the student
    
    only way to learn mathematics: doing exercises

### 1.4 Category theory references

    most books: for mathematicians or computer scientists

## ch02 The Category of Sets

    theory of sets
      invented as a foundation for all mathematics
      serves as a basis to build intuition about categories
    this chapter
      sets, functions, commutative diagrams
      ologs
    category of sets: Set

### 2.1 Sets and functions

    [a thing] is put into -> [a bin]

#### 2.1.1 Sets

    a set X: a collection of elements x ∈ X
      we can tell if x = x' or not
    notation 2.1.1.1. symbol ∅ denotes set with no elements, also {}
      N: natural numbers
      Z: natural numbers annd their negatives
    if A and B are sets
      we say A is a subset of B, also A ⊆ B
        if every element of A is an element of B
    for any set A
      ∅ ⊆ A and A ⊆ A
    set-builder notation to denote subsets
      ex: set of even integers: {n ∈ Z| n is even}
      ex: set of integers greater than 2:
        {n ∈ Z | n > 2}
    symbol ∃: there exists
      ex: set of even integers
        {n ∈ Z | n is even} = {n ∈ Z | ∃ m ∈ Z st 2m = n}
    symbol ∃!: there exists a unique
    symbol ∀: for all
    colon equals notation ":=": define x to be y

#### 2.1.2 Functions

    if X and Y are sets
      then a function f from X to Y
      denoted f:X->Y
      is a mapping that sends each element x ∈ X to an element of Y denoted f(x) ∈ Y
      X: domain of function f
      Y: codomain of f
    slogan 2.1.2.1
      given a function f: X -> Y
      we say: 
        X: a set of things
        Y: a set of bins
      function tells us in which bin to put each thing
    ex 2.1.2.4:
      suppose 
        X is a set 
        X' ⊆ X is a subset
      consider function: X' -> X
        given by sending every element of X' to itself
      notation:
        i:X'⊆ X
          i is the name of the associated function
    image of f:
      given f:X->Y
      elements of Y that have at least one arrow pointing to them: are in the image of f
      im(f) := {y∈Y| ∃ x ∈ X st f(x) = y}
    image of f is always a subset of its codomain
      im(f) ⊆ Y
    composable:
      given:
        f: X -> Y
        g: Y -> Z
        ie: codomain of f is domain of g
      then: f and g are composable
        [X] f-> [Y] g-> [Z]
      denoted:
        composition of f and g
        g · f: X -> Z
    image of set A:
      if A ⊆ X
        then: i: A -> X
      given f: X -> Y
        then: we can compose
          [A] i-> [X] f-> [Y]
          f · i: A -> Y
      image of this function is denoted
        f(A) := im(f·i)
    notation 2.1.2.9: represents
      let 
        X be a set
        x ∈ X an element
      there is a function {😃 } -> X
        that sends 😃  → x
      say:
        this function represents x ∈ X
      denote:
        x: {😃 } -> X
    ex 2.1.2.10
      let
        X a set
        x ∈ X an element
        x:{😃 } -> X function representing it
      given f:X->Y
      what is f·x?
        function {😃 } -> Y
          that sends 😃  to f(x)
        it represents the element f(x)
    Remark 2.1.2.11
      g·f(a) means g(f(a))
        do g to whatever results from doing f to a
      diagrammatic order:
        another way to write composition
        f;g: A -> C
          means: "do f, then do g"
      given an element a ∈ A
        represented by a:{😃 } -> A
        we have an element:
          a;f;g
    Hom_{Set}(X,Y)
      let: X,Y sets
      set of functions X->Y
        denoted as: Hom_{Set}(X,Y)
      note:
        two functions f,g:X->Y are equal
          iff ∀ x ∈ X, f(x) = g(x)
    ex 2.1.2.12
      let A = {1,2,3}, B = {x,y}
      how many elements does Hom_{Set}(A,B) have?
    identity function on X
      id_X: X -> X
      st ∀ x ∈ X, id_X(x) = x
    definition 2.1.2.14: isomorphism
      let X, Y be sets
      function f:X->Y is an isomorphism
        denoted [f:X] ≅-> [Y]
      if ∃ g:Y->X st.
        g·f = id_X and f·g = id_Y
      we also say
        f is invertible
        g is the inverse of f
        X and Y are isomorphic sets
        X ≅ Y
        one-to-one correspondence
    application 2.1.2.16
      isomorphism between Nuc_{DNA} nucleotides in DNA and Nuc_{RNA} nucleotides in RNA
      boths sets have 4 elements
        so there are 24 different isomorphisms
        only one is useful in biology
      another isomorphism: Nuc_{DNA} ≅ {A,C,G,T} and Nuc_{RNA} ≅ {A,C,G,U}
        so we can use those letters as short form for nucleotides
      convenient isomorphism [Nuc_{DNA}] ≅-> [Nuc_{RNA}] is given by RNA transcription:
        A→U, C→G, G→C, T→A
      another isomorphism: [Nuc_{DNA}] ≅-> [Nuc_{DNA}] the matching in double helix, given by:
        A→T, C→G, G→C, T→A
      protein production:
        a function from the set of 3-nucleotide sequences to the set of amino acids
        it cannot be isomorphism because 
          4^3=64 triplets of RNA nucleotides
          but only 21 eukaryotic amino acids
    proposition 2.1.2.18
      1. any set A is isomorphic to itself
      2. if A is isomorphic to B, then B is isomorphic to A
      3. if A ≅ B and B ≅ C, then A ≅ C
    ex 2.1.2.19
      even if two sets are isomorphic, they cannot be treated as the same
      to be treated as same,
        we need to have in hand a specified isomorphism
    ex 2.1.2.20
      find A st for any set X, there is an isomorphism of sets
        X ≅ Hom_{Set}(A,X)
    notation 2.1.2.21
      ∀ n ∈ N, define a set ṉ := {1,2,3,...,n}
      ṉ the numeral set of size n 
      ex:
        2_ = {1,2}, 1_ = {1}, 0_ = ∅ 
      let A be any set
      function f:ṉ->A can be written as a length n sequence:
        f = (f(1),f(2),...f(n))
      called: sequence notation for f
    ex 2.1.2.22
      a. A = {a,b,c 2017-01-28}. 
        if f:10_->A is in sequence notation as (a,b,c,c,b,a,d,d,a,b)
        what is f(4)?
    definition 2.1.2.23: cardinality of finite sets
      A a set. n ∈ N 
      A has cardinality n
      denoted
        |A| = n
      if there exists an isomorphism of sets A ≅ ṉ
      if A has cardinality n, then A is finite
      other wise A is infinite
      we write |A| ≥ ∞ 
    proposition 2.1.2.25
      if there is an isomorphism of sets f:A->B
        then |A| = |B|
    
### 2.2 Commutative diagrams

    ex:
      [A] f-> [B]
      [A] h-> [C]
      [B] g-> [C]
      we say:
        a diagram of sets if A,B,C is a set and each of f,g,h is a function
        this diagram commutes if g·f=h
        we refer to it as a commutative triangle of sets, or as a commutative diagram of sets
    application 2.2.1.1
      biology:
        function from DNA to RNA
        function from RNA to amino acids
      we say: translation from DNA to amino acids
        following commutative diagram is this fact:
          [DNA] -> [RNA]
          [RNA] -> [AA]
          [DNA] -> [AA]
    consider:
      [A] f-> [B]
      [A] h-> [C]
      [B] g-> [D]
      [C] i-> [D]
      we say:
        diagram of sets if 
          each A,B,C,D is a set
          each f,g,h,i is a function
        this diagram commutes if
          g·f = i·h
          then we refer: commutative square of sets
          or commutative diagram of sets
    application 2.2.1.2
      given a system S
      there are two mathematical approaches:
        f:S->A
        g:S->B
      either results in a prediction:
        f':A->P
        g':B->P
      diagram commutes = both approaches give the same result

### 2.3 Ologs

    ologs
      serve as a bridge between mathematics and various conceptual landscapes
      ex:
        [A| arginine] is-> [D| an amino acid found in dairy]
        [A] has -> [E| an electrically charged side chain]
        [D] is -> [X| an amino acid]
        [A] is -> [X]
        [E] is -> [R| a side chain]
        [X] has -> [R]
        [X] has -> [N| an amine group]
        [X] has -> [C| a carboxylic acid]

#### 2.3.1 Types

    type: an abstract concept
      represented as a box
        containing a singular indefinite noun phrase
          [a man]
          [an automobile]
          [a pair (a,w) where w is a woman and a is an automobile]
        each box represents a type of thing
        label on that box: each example of that class
          thus: [a man] is not a single man, but the set of men. each example of which is called [a man]

##### 2.3.1.1 Types with compound structures

    compound structures: composed of smaller units
    ex
      [a man and a woman]
      [a food portion f and a child c such that c ate all of f]
    good practice to declare variables in a compound type
      ex
        [a man and a woman]
        [a pair (m), where m is a man and w is a woman]
    rules of good practice 2.3.1.2
      a type is presented as a text box
      text should
        begin with word "a"
        refer to a distinction made by the author
        refer to a distinction for which instances can be documented
        use common name for each instance
        declare all variables in compound structures
      nicknames:
        [Steve] stands as a nickname for [a thing classified as Steve]
        [arginine] for [a molecule of arginine]

#### 2.3.2 Aspects

    aspect of a thing x
      a particular way in which x can be regarded
    ex
      a woman can be regarded as a person
        being a person is an aspect
        [a woman] is-> [a person]
      a molecule has a molecular mass
        having a molecular mass is an aspect
        [a molecule] has a molecular mass-> [a positive real number]
    aspect: means function
      f:A->B
      domain A: the thing we are measuring
      codomain B: set of possible answers of the measurement
    ex
      [a woman] is-> [a person]
      domain: set of women (3 billion elements)
      codomain: set of persons (6 bil elements)
      no woman points to two different people nor to zero people
        so this is a function
    ex
      [a molecule] has a molecular mass-> [a positive real number]
      domain: set of molecules
      codomain: positive real numbers
      for each molecule: there is a corresponding positive real number
      no molecule has two different masses, nor no mass

##### 2.3.2.1 Invalid aspects

    to be valid: aspect must be a functional relationship
    ex:
      [a person] has-> [a child]
      a person may have no children, or multiple children. hence not a function

##### 2.3.2.4 Reading aspects and paths as English phrases

    each arrow (aspect) [X] f-> [Y]
      read by order: 
        ex: [a book] has as first author -> [a perso]
          a book has as first author a person
    remark 2.3.2.5
      [a book] has as author -> [a person]
        not valid
        because it is not functional
    if it is obvious, you can drop labels on arrows
      ex:
        [A| a pair (x,y) where x and y are integers] x -> [B| an integer]
        [A] y -> [B'| an integer]
      label x is as a nickname for the full name "yields as the value of x"
    opt: use "which" before each arrow label

##### 2.3.2.6 Converting nonfunctional relationships to aspects

    ex:
      [a person] owns -> [a car]
        invalid
      valid form:
        [A| a pair (p,c), where p is a person, c is a car, and p owns c] p -> [a person]
        [A] c -> [a car]

#### 2.3.3 Facts

    fact: path equivalences in an olog
      this is what makes category theory so powerful
    path in an olog: head-to-tail sequence of arrows
      number of arrows: length of the path
      B0: source
      Bn: target
    two paths are equivalent:
      triangle in olog commutes
        A -> B -> C
        A -> C
      commutative diagrams
      ex:
        [A| a person] has as parents -> [B| a pair (w,m), w: woman, m: man]
        [B] yields as w -> [C| a woman]
        [A] has as mother -> [C]
    notation: a diagram commutes:
      A[f,g] ≅ A[h,i]
      means:
        starting from A, 
          doing f, then g
          is equivalent to
          doing h, then i
    ex:
      [A| a DNA sequence] is transcribed to -> [B| an RNA sequence]
      [B] is translated to -> [C| a protein]
      [A] codes for -> [C]
    ex 2.3.3.1:
      [a child] has -> [a mother]
      [a mother] is -> [a person]
      [a child] is -> [a person]
      this triangle doesn't commute
      if they would, we would say: "a child and its mother are the same person"
    ex 2.3.3.2:
      [a person] has as father -> [a man]
      [a man] lives in -> [a city]
      [a person] lives in -> [a city]
      noncommuting diagram

##### 2.3.3.4 A formula for writing facts as English

    paths: P, Q
    ex 2.3.3.5:
      consider olog
        [A|a person]
        [B| an address]
        [C| a city]
        [A] has -> [B]
        [B] is in -> [C]
        [A] lives in -> [C]
      to put this diagram commutes into English:
        two paths:
          F="a person has an address which is in a city"
          G="a person lives in a city"
          source of both: s="a person"
          target of both: t="a city"
        write:
          given x, a person, consider the following.
          We know that x is a person,
          who has an address, which is in a city,
          that we call P(x).
          We also know that x is a person,
          who lives in a city
          that we call Q(x).
          Fact: Whenever x is a person, we will have P(x) = Q(x).
        more concisely:
          A person x has an address, which is in a city, and this is the city x lives in.
    general:
      paths: P,Q
        P: [a0=s] f1-> [a1] f2-> [a2] ... fm -> [am=t]
        Q: [b0=s] g1-> [b1] g2-> [b2] ... gm -> [bm=t]
      every part l (ie. every box and every arrow) has an English phrase
        denoted: <<l>>
      englished as:
        Given x,<<s>> consider the following.
        We know that x is <<s>>,
        which <<f1>><<a1>>, which <<f2>><<a2>>,..., which <<fm>><<t>>,
        that we call P(x).
        We also know that x is <<s>>,
        which <<g1>><<b1>>, which <<g2>><<b2>>,..., which <<gm>><<t>>,
        that we call Q(x).
        Fact: Whenever x is <<s>>, we will have P(x)=Q(x).
    ex 2.3.3.6:
      olog
        [OLP| an operational landline phone]
        [N| a phone number]
        [C| an area code]
        [P| a physical phone]
        [R| a region]
        [OLP] is assigned -> [N]
        [N] has -> [C]
        [C] corresponds to -> [R]
        [OLP] is -> [P]
        [P] is currently located in -> [R]
      short form:
        [OLP] is assigned -> [N] has -> [C] corresponds to -> [R]
        [OLP] is -> [P] is currently located in -> [R]

##### 2.3.3.8 Images

    ex:
      set of mothers is the image of the "has as mother" function
        [P|a person]
        [P'|a person]
        [M|a mother]
        [P] has as mother -> [P']
        [P] has -> [M] is -> [P']

## ch03: Fundamental Considerations in Set

### 3.1 Products and coproducts

#### 3.1.1 Products

    definition 3.1.1.1
      product of X and Y
        denoted X ⨯ Y 
        is set of ordered pairs (x,y), x ∈ X and y ∈ Y
      X ⨯ Y = {(x,y)| x ∈ X, y ∈ Y}
    there are two natural projection functions:
      π1: X ⨯ Y -> X
      π2: X ⨯ Y -> Y
      olog
        [X⨯Y] π1 -> [X]
        [X⨯Y] π2 -> [Y]
    ex 3.1.1.8: 
      let
        +: Z ⨯ Z -> Z addition function
        ·: Z ⨯ Z -> Z multiplication function

##### 3.1.1.9 Universal property for products

![](/assets/img/ss-25.png)

    proposition 3.1.1.10 
      /Users/mertnuhoglu/Dropbox/public/img/ss-25.png
      for f:A->X and g:A->Y
      ∃ a unique function A -> X ⨯ Y
      st the following diagram commutes:
        [X ⨯ Y] π1 -> [X]
        [X ⨯ Y] π2 -> [Y]
        [A] f -> [X]
        [A] g -> [Y]
        [A] <f,g> -> [X ⨯ Y] 
      we say:
        this function is induced by f and g
        denoted:
          <f,g>: A -> X ⨯ Y
          <f,g>(a) = (f(a),g(a))
        that is:
          π1 · <f,g> = f
          π2 · <f,g> = g
          <f,g> is the only function for which this is so
    ex 3.1.1.13
      for every set A there is some relationship between the following three sets:
        Hom_{Set}(A,X)
        Hom_{Set}(A,Y)
        Hom_{Set}(A,X⨯Y)
      solution
        there is an isomorphism
        Hom_{Set}(A,X⨯Y) ≅ -> Hom_{Set}(A,X) ⨯ Hom_{Set}(A,Y)
      ex
        height function: from set P of people to Z of integers
        name function: from P to S of strings
        so, we have 
          an element of Hom_{Set}(P,Z) and 
          an element of Hom_{Set}(P,S) 
        from this we get an element of:
          Hom_{Set}(P,Z) ⨯ Hom_{Set}(P,S)
        ie: two pieces of information combined
          we pack height and name into a single datum
            ie. an element of Z ⨯ S
    ex 3.1.1.14
      let:
        π1: X ⨯ Y -> X
        π2: X ⨯ Y -> Y
        p1: Y ⨯ X -> Y
        p2: Y ⨯ X -> X
      construct swap map:
        s: X ⨯ Y -> Y ⨯ X
        write s in terms of π, p, ·, <, >
      solution:
        s = <π2,π1>
    
##### 3.1.1.16 Ologging products

    ex 3.1.1.17
      
#### 3.1.2 Coproducts

    definition 3.1.2.1
      coproduct of X and Y 
      denoted X ▄ Y
      is defined as disjoint union of X and Y
        ie. the set for which an element is either from X or Y
      two natural inclusion functions:
        i1: X -> X ▄ Y
        i2: Y -> X ▄ Y
    ex 3.1.2.2
      coproduct of X := {a,b,c} and Y := {1,2}
        X ▄ Y ≅ {a,b,c,1,2}
    coproduct of X and itself
      X ▄ X ≅ {a1,b1,c1,a2,b2,c2}
    names of the elements in X ▄ Y are not important
      inclusion maps i1,i2 are important
      which ensure we know where each element is from
    ex 3.1.2.3: Airplane seats
      X: an economy-class seat in an airplane
      Y: a first-class seat in an airplane
      X▄Y: a seat in an airplane
      [X] is -> [X▄Y]
      [Y] is -> [X▄Y]
    @mine
      coproduct is inheritance is-a relationship
      product is join table
    ex 3.1.2.4
      is [a phone] is coproduct of [a cell phone] and [a landline phone]?
      solution
        if there is no phone other than a cell phone and landline phone, yes. if not, no
    ex 3.1.2.5: disjoint union of dots
      /Users/mertnuhoglu/Dropbox/public/img/ss-26.png
    
![](/assets/img/ss-26.png)

##### 3.1.2.6 Universal property for coproducts

![](/assets/img/ss-27.png)

    proposition 3.1.2.7
      ~/Dropbox/public/img/ss-27.png
      the following diagram commutes:
        X i1 -> X ▄ Y
        Y i2 -> X ▄ Y
        X f -> A
        Y g -> A
        X ▄ Y {f g -> A
      this function is induced by f and g
      ie. we have
        {f g · i1 = f
        {f g · i2 = g
    slogan 3.1.2.8
      any time behavior is determined by cases, there is a coproduct involved
    why two-line symbol {
      because this is case notation


