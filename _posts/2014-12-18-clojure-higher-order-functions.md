---
layout: post
title: Higher order functions in Clojure
date: 2014-12-18 16:04:42
---

Working my way through the [Clojure Koans](https://github.com/functional-koans/clojure-koans/) I learned about _Higher Order Functions_.

The koan _meditation_ was this:

```clojure
"Higher-order functions take function arguments"
(= 25 (___
      (fn [n] (* n n))))
```

The question is: what's an higher-order function? 

[Wikipedia](http://en.wikipedia.org/wiki/Higher-order_function) tells me that an HOF is a function that does at least one of these:

* takes one (or more) functions as an input
* returns another function as an output

An example from calculus is the [derivative](http://en.wikipedia.org/wiki/Derivative) function.

Also all other functions are called first-order functions.

The solution to the koan above is know clearer: it needs to be a function that takes another function as argument, in this case a square functions, and applies to a number in order to output 25.

```clojure
(def function_of_five (fn [f] (f 5))
```

So when we write:

```clojure
(= 25 ((fn [f] (f 5))
      (fn [n] (* n n)))))
```

What happens it that the `f` becomes `(fn [n] (* n n))` and the `n` becomes `5`, which results into `25`.

A deeper look at Clojure's HOF can be found [here](http://christophermaier.name/blog/2011/07/07/writing-elegant-clojure-code-using-higher-order-functions).

I'm getting more and more in love with Clojure. I like how it's so different from what I'm used to as an OO developer. And I like how it's slowly bringing back some of the academic mathematic concepts I got in touch with (learned would be an overstatement) at uni and that I considered useless for my day to day job for a long time.