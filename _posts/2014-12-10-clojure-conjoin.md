---
layout: post
title: The Clojure "conjoin" function
date: 2014-12-10 17:06:42
---

Playing around with [Clojure Koans]() I encountered this:

```clojure
"Conjoining an element to a list isn't hard either"
(= (list :e :a :b :c :d) (conj '(:a :b :c :d) :e))
```

"Conjoining" sounds like a weird name... The dictionary simply treats it as a _join_ synonim:

> verb formal
> join; combine.
> 
> "an approach which conjoins theory and method"

Typing `(doc conj)` in the REPL gives more informations:

```clojure
user=> (doc conj)
-------------------------
clojure.core/conj
([coll x] [coll x & xs])
  conj[oin]. Returns a new collection with the xs
    'added'. (conj nil item) returns (item).  The 'addition' may
    happen at different 'places' depending on the concrete type.
```

Since "joining" and "adding" are very similar operations it kinda makes sense that this method is called "con-join" then. As in: this function _kinda_ joins elements.

This is both very interesting and scary. I'm not a big fan of methods that behave differently depending on the type.

### `conj` behaviour with different collection types

```clojure
;; array
user=> (conj [1 2 3] 4)
[1 2 3 4]

;; list
user=> (conj '(1 2 3) 4)
(4 1 2 3)

;; map
user=> (conj {1 2, 3 4} [5 6]) {5 6, 1 2, 3 4} 

;; set
(conj #{1 3 4} 2)
#{1 2 3 4}
```

This tiny exploration is source for a lot of food for thoughts:

* `conj` seems to be like what in an OO language a method of an interface would be. Is this correct? If so, how does this work in Clojure?
* How many different collection types are there in Clojure?
* How do they all behave when running `conj`?
* Can I write a custom type, with its own `conj` implementation?