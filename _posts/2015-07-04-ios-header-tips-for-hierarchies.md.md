---
layout: post
title: Objective-C designated and unavailable initializers
date: 2015-07-04 10:36:42
---

## `NS_DESIGNATED_INITIALIZER`

```objc
// an header

- (instancetype)initWithSomeObject:(SomeObject *)someObject NS_DESIGNATED_INITIALIZER;
```

This will make `initWithSomeObject:` the [designated initializer](https://developer.apple.com/library/mac/documentation/General/Conceptual/CocoaEncyclopedia/Initialization/Initialization.html#//apple_ref/doc/uid/TP40010810-CH6-SW3). In a nutshell the designated initializer is the only one allowed to call `super`, and the one that sets all the variables and constants that need to be set on initialisation, the other initializers are only wrappers around the designated one.

Using `NS_DESIGNATED_INITIALIZER` we can make the compiler enforce the designated initializer behaviour for us.

There are several advantages in having a designated initializer, in my opinion the most important ones are **clarity** and **single responsibility**. Clarity, because by looking at it a reader can immediately see everything needed by the object on initialisation, all it's dependencies; and single responsibility, because we centralise the actual initialisation in one point.

## `__attribute((unavailable("...")))`

Before starting I would like to point out that **sublcassing is in most cases a bad idea**, and that composition should be used instead, as much as possible. That said sometimes we have to subclass, for example when using `UIViewController`s. There are also cases in which we might need to add an initialization parameter to the subclass.

We just saw how we can use `NS_DESIGNATED_INITIALIZER` to promote one `init...` method as the designated initializer, but this won't prevent consumers to call other `init...` methods. What would happen then if the superclass `init` was called? The instance wouldn't be created with the required objects 😱.

We can use `__attribute((unavailable))` to mark a superclass initializer as off limits:

```objc
// super class header

- (instancetype)initWithSomeObject:(SomeObject *)object NS_DESIGNATED_INITIALIZER;

// subclass header

- (instancetype)initWithSomeSuperDuperObject:(SuperDuperObject *)object NS_DESIGNATED_INITIALIZER;

- (instancetype)initWithSomeObject:(SomeObject *)object __attribute((unavailable));
```

This way the compiler will ensure that consumers of the subclass won't call the super class initializer.

You can add a message for the other developers using `__attribute((unavailable("message")))`, or be more concise and only use `__unavailable`.

