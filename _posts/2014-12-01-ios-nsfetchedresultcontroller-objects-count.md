---
layout: post
title: Counting objects returned by NSFetchedResultsController
date: 2014-12-01 08:30:42
---

To get the total number of objects fetched by a `NSFetchedResultsController` you could do this:

```objc
NSFetchedResultsController *controller = ...
NSUInteger count = [controller.fetchedObjects count];
```

Which is fine... But this is better:

```objc
NSFetchedResultsController *controller = ...
[controller.sections.valueForKeyPath: @"@sum.numberOfObjects"];
```

Why? Because it uses the neat [KVC](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/KeyValueCoding/Articles/KeyValueCoding.html), which is an awesome yet under used tool.

I also have the gut feeling that it's more efficient that `count`, but I'd need to run some tests to be sure about it.