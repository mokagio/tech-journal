---
layout: post
title: How and why you should use type aliasing in Swift
date: 2015-01-19 08:20:42
---

With the introduction of Swift iOS and Mac developers keen in writing good _semantic_ code saw a dream come true. The simplicity and power of the Swift's syntax really opens the door for a cleaner and more readable code. I recently read a post on [the meaning of "readable" code](http://blog.jessitron.com/2015/01/readable-or-reason-aboutable.html), so I wanna specify that here _readable_ means self-explanatory and that "reads like english" as much as possible.

One of the techniques I like to use to make code more readable is redefining basic types with domain specific names. I first read about this [here](http://www.frandieguez.com/blog/2012/12/object-calisthenics-write-better-object-oriented-code/), but I assume is a well known technique.

For example, given an `Athlete` class, the method `medalsWonCount` won't return an `Integer`, but a `Medals` type. 

Another example: given a `Contact` class, the `emailAddress` method won't return a `String`, but an `EmailAddress` type.

Wrapping primitive types and strings in domain specific types certainly adds an overhead at the time of writing, but makes for clearer code. And remember that [_code is read more time than it's written_](http://blogs.msdn.com/b/oldnewthing/archive/2007/04/06/2036150.aspx).

So, how do you redefine a type in Swift?

```swift
typealias Email = String

let email: Email = "giovanni.lodi42@gmail.com"
```