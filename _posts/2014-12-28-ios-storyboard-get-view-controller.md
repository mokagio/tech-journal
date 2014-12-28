---
layout: post
title: Instantiate Storyboard View Controller in code
date: 2014-12-28 23:10:42
---

Oh, Interface Builder and Storyboards... I really don't like these tools! But year after year Apple pushes more and more for developers to use them, and keeps improving them. Also many fellow devs that I respect rely on them. That's why when working on my latest side project (_stay tuned_) I'm pushing myself to use them.

Despite that resolution I incurred in a situation where it was necessary to instantiate a view controller defined in the Storyboard via code.

This is a very simple task:

```swift
let storyBoard = UIStoryboard(name: "Main", bundle: nil)
var viewController = storyBoard.instantiateViewControllerWithIdentifier("ViewControllerIdentifier") as ViewControllerClass
// present the view controller as needed
```

Two very simple lines... that I shall never use again, and find a way to do it entirely via Storyboard!
