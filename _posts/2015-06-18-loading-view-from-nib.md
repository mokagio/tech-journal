---
layout: post
title: Drawing custom view in Interface Builder, and loading it in code
date: 2015-06-18 10:13:42
---

I am kind of ashamed that after all this time I still get this stuff wrong, so here's the memo post. To make up for my silliness the code is in Swift 👍.

## Drawing a custom view in Interface Builder

The workflow to draw a custom view in IB in it's own `.xib` is the same you would use when dealing with scene subview in a Storyboard, mainly drag and drop stuff.

The important thing to keep in mind is that the subviews need to be connected to the **outlets of the view itself**, rather than the file owner, as one would usually do when working on a view controller.

![Interface Builder Screenshot](https://s3.amazonaws.com/tech-journal/connect-subview-outlet-to-custom-view.png)

## Loading the image from code

To get an instance of the custom view via code you cannot simply call `initWithFrame:`, because that wouldn't load the content of the `.xib` in the view.

This is how you can get an instance of custom view drawn in Interface Builder:

```swift
let view = NSBundle.mainBundle().loadNibNamed("View", owner: self, options: nil).first as! UIView
```

Usually this code would be called either from a view or a view controller, which is the `owner: self` parameter.

Also note the `as!` forced unwrap. We can do that because we are sure, _or at least should be_, that the main bundle contains a nib named View.

