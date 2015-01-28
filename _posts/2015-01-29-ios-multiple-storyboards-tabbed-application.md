---
layout: post
title: Using multiple Storyboards on an iOS tabbed application
date: 2015-01-29 7:56:42
---

I'm personally not a big fan of Interface Builder and Storyboards, but it is undeniable that they make it easier to have a glance of the UI and of the flow on an app.

One problem I've seen is some projects is a very big Storyboard, with all the app in there. Such Storyboards are unmanageable, and you end up spending more time navigating through them, than doing actual development.

Apple doesn't provide a way to link different Storyboards together, yet. Fortunately there are some open source library that take care of it.

One example is [RBStoryboardLink](https://github.com/rob-brown/RBStoryboardLink), which also makes it easy to **setup multiple Storyboards for a tabbed application**.

The library lets us use a _placeholder_ view controller in a Storyboard, and use that to link to another Storyboard, we'll see how later.

To set it up simply add a view controller in the _main_ Storyboard, set its class to `RBStorybaordLink`, and add a "User Defined Runtime Attributes" of type String, key path "storyboardName" and value the name of the Storyboard (without extension) that needs to be linked. This can be done from the "Identy Inspector" section.

It's as simple as that!

Under the hood what `RBStoryboardLink` does is grabbing the runtime attributes the user has defined and use those value to instantiate the corresponding `UIViewController` and store it in a property in `-awakeFromNib`. Then in `-viewDidLoad` it embeds the stored view controller as a full screen child view controller. On top of all this there is code to copy all the settings related to Auto Layout, navigation bar, etc.

I think this is a clever _hack_, and very useful one!

## Food for thoughts

* How do the "User Defined Runtime Attributes" work? How does the runtime translate them from Interface Builder settings to property in available to the instance?
* RBStoryboardLink defines a set of custom segues, how do they work?



