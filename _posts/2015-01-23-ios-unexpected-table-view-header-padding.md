---
layout: post
title: How to remove the unexpected padding on the header of a UITableView
date: 2015-01-23 6:3:42
---

Almost every time I lay down a navigation controller, with a root view controller that contains a table view the result is a table view with an unexpected padding on it's header.

### Why

The reason this happens is `UIViewController`'s property `automaticallyAdjustsScrollViewInsets`, which is _a Boolean value that indicates whether the view controller should automatically adjust its scroll view insets._, and defaults to `YES`.

The property has been introduced in iOS 7 and together with `edgesForExtendedLayout` makes the nice effect of content scrolling below a translucent navigation bar possible.

## How to fix it

So far I found two ways to solve this UI issue:

### Set `automaticallyAdjustsScrollViewInsets` to `false`

Setting the `automaticallyAdjustsScrollViewInsets` value to `false` solves the issue. 

In Interface Builder this can be done by unticking the "Adjust Scroll View Inset" option of the view controller where the table view is.

### Make the navigation bar non translucent

Another option is to make the navigation bar non translucent. Such a navigation bar doesn't trigger the scroll view insets adjustments in the view controller.

In Interface Builder this can be done by unticking the "Translucent" option of the navigation controller's navigation bar. I also found that the only way to actually select that component is through the view hierarchy tree on the left. 

## `UIViewController` containment

A final note on table views embedded via view controller containment. To avoid the header in those the we need again to set to `false` the `automaticallyAdjustsScrollViewInsets` property of the **parent** view controller.

This seemed counter intuitive to me at first. But if we think about how the containment process works, by extracting the view from the child and placing it in the parent, than it becomes obvious that the layout rules such as `automaticallyAdjustsScrollViewInsets` of the parent applies to the child as well.

## Food for thoughts

* Are there other, maybe better, way to fix this issue?
* Can it be avoided by laying down the views differently?
* Why the standard navigation controller + table view controller from the Interface Builder components library doesn't do this?
* Look deeper at the `automaticallyAdjustsScrollViewInsets` and `edgesForExtendedLayout` mechanics.

