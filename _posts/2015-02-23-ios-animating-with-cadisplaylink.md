---
layout: post
title: Animations with CADisplayLink
date: 2015-02-23 19:25:42
---

When animating views on a CocoaTouch application we usually rely on the [`+ animateWithDuration:animations:`](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/#//apple_ref/occ/clm/UIView/animateWithDuration:animations:) method of `UIView`. This approach works remarkably well for growing/shrinking, hiding/showing, and moving around UI elements, but what if we need a finer control? What if we need to implement something like a _game loop_, where every frame the screen is updated based on the new state of the world model?

In those cases `+ animateWithDuration:animations:` is not enough. We need a timer object that periodically "_ticks_" and updates our complex view.

I said timer, but don't even think about using NSTimer!

The class that does the job is [CADisplayLink](https://developer.apple.com/library/prerelease/ios/documentation/QuartzCore/Reference/CADisplayLink_ClassRef/index.html).

> A `CADisplayLink` object is a timer object that allows your application to synchronize its drawing to the refresh rate of the display.

And this is why `CADisplayLink` is the go-to when we need to do a model based animation. With it we can run our view update code in sync with the screen refresh.

Here's a snippet on how to setup `CADisplayLink`, since I always forget about it...

```objc
@property (nonatomic, strong) CADisplayLink *timer;
@property (nonatomic, assign) NSTimeInterval lastTimestamp;

//...

- (void)startScreenUpdate {
    self.timer = [CADisplayLink displayLinkWithTarget:self selector:@selector(tick:)];
    [self.timer addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSDefaultRunLoopMode];
}

- (void)tick:(CADisplayLink *)sender {
    if (self.lastTimestamp == 0) {
        // first tick, nothing to do
        return;
    }

    CGFloat elapsedTime = sender.timestamp  - self.lastTimestamp;
    self.lastTimestamp = sender.timestamp;

    [self updateMovingObjectPositionWithElapsedTime:elapsedTime];

    //...
}
```

### Food for thoughts

* Provide an example where there's actually moving stuff.
* Look deeper into why `CADisplayLink` is better suited that `NSTimer` to animate the view.
