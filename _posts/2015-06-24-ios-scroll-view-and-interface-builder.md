---
layout: post
title: How to configure a UIScrollView with Auto Layout in Interface Builder
date: 2015-06-24 17:22:42
---

**TL;DR**:

- Add a `UIView` as the `UIScrollView` content view
- Pin the content view to the edges of the `UIScrollView`
- Add a equal width constraint between the content view and the `UIScrollView`'s superview. It could be an height depending on your layout.

The key thing to understand is that `UIScrollView` _evaluates its size at runtime based on the content - link needed_. For this reason when in IB you will _always see it as having 0 width and 0 height (really)_.

Using a content view with the same width or height as the superview where the scroll view is contained does the trick of allowing us to define the subview's Auto Layout constraints, but only if the subviews as a whole are pinned to the edges of the content view.

Finally, since we need to use the content view width to set the `UIScrollView` width, any padding inside the scroll view has to be achieved as margin between the subview's and the content view.

Enough talking! Here's some pictures:

![Pinned scroll view](https://s3.amazonaws.com/tech-journal/scroll-view-ib-1.png)

The scroll view is pinned to the view controller's view edges, but still we can't see it. That's because it computes its size at runtime based on its content.

![Pinned content view with equal width to main view](https://s3.amazonaws.com/tech-journal/scroll-view-ib-2.png)

The content view is pinned to the scroll view, and has the same width as the main view.

_Note:_ If you tried to redraw the content view frame now it would end up with 0 height, and that is because it has no subviews that Auto Layout can use to evaluate the height, and it's superview -the scroll view- evaluates it's height at run time, so it's 0 at this moment.

![Once content view has subviews with proper constraints everything works fine](https://s3.amazonaws.com/tech-journal/scroll-view-ib-3.png)

When adding subviews the important thing to remember is to pin them in a way to make it possible for Auto Layout to evaluate the height.

Done. There is a bit of prep work to do, but is certainly pays of to be able to rely on Auto Layout for our scroll view :)

[This post](http://natashatherobot.com/ios-autolayout-scrollview/) has been very helpful for me to understand the process.

### Food for thought

* How does this play with iOS 9 new `UIStackView`?
* Try this out with a equal height constraint between main view and content view. What changes?
* How would this work using code instead of IB?
