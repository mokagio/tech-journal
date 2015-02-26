---
layout: post
title: How to prevent code from running in the iOS Simulator
date: 2015-02-26 15:37:42
---

_I always forget this syntax!_

```objc
#import <TargetConditionals.h>

//...

- (void)methodThatRunsOnlyOnDevice() {
    #if !TARGET_IPHONE_SIMULATOR
        // Do stuff that would work only on the device
    #else
        // Maybe log that this method is not running because on the simulator?
    #endif
}
```

I try to avoid preprocessor macros as much as possible, they are a [code smell](http://qualitycoding.org/preprocessor/), and add yet another path that needs to be tested, but that is intrinsically hard to test.

That said, in some cases they can be handy. For example when you have a piece of code that absolutely cannot run on the Simulator. In that case the only way to make sure this doesn't happen is checking for the `TARGET_IPHONE_SIMULATOR` conditional.
