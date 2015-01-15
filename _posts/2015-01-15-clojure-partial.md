---
layout: post
title: The Clojure "partial" function - Part 1
date: 2015-01-15 21:41:42
---

I'm still progressing in my quest to expand my horizons by learning about functional programming. More hipster than the hipsters in the iOS community, instead of starting with Haskell, I've started with Clojure.

I've already written about Clojure a bit in this journal, and this post is gonna be the first of a series in which I tackle the concepts shown in [this post on HOF](http://christophermaier.name/blog/2011/07/07/writing-elegant-clojure-code-using-higher-order-functions), trying to master them.

The inspiration for this process came to me from the book "[So Good They Can't Ignore You](http://www.amazon.com/Good-They-Cant-Ignore-You/dp/1455509124)" by Cal Newport. In the book the author, a young mathematician professor, shares one of the _deliberate practice_ techniques that he uses to gain valuable skills: tackling very hard papers, decomposing them in smaller pieces, until he has mastered the concepts they expose.

So there we go: Clojure's `partial` function.

```clojure
user=> (doc partial)
-------------------------
clojure.core/partial
([f] [f arg1] [f arg1 arg2] [f arg1 arg2 arg3] [f arg1 arg2 arg3 & more])
  Takes a function f and fewer than the normal arguments to f, and
  returns a fn that takes a variable number of additional args. When
  called, the returned function calls f with args + additional args.
nil
```

First of all, we can see how `partial` is an higher order function, it ticks both boxes, it takes a function as input, and produces a function as output.

Given a function `f` that takes a number of parameters, we can call `partial` with `f` and a smaller amount of parameters. `partial` will return a new function, `g`, which accepts a variable number of parameters. When called `g` invokes `f` with the arguments given to `partial`, plus the ones given to `g`.

Here's an example:

```clojure
user=> (def splitter (partial * 0.5))
#'user/splitter
user=> (splitter 3)
1.5
```

Which is the same as writing:

```clojure
user=> (def other_splitter (fn [x] (* 0.5 x)))
#'user/other_splitter
user=> (other_splitter 3)
1.5
```

This example using a function that takes only 2, rather than n, arguments, helped me out understanding what `partial` does. The function made by `partial` simply calls the original function, with the first parameters already set.

### Where to go from here

_I time-boxed the posts in this journal to 25 minutes, and I've been on this for almost one hour trying to understand some of the concepts._

* Understand how to use partial with a custom made function that makes an average.
* Understand how to use partial with other HOF, the post uses it with `map`.
* Continue the mastery of the post.