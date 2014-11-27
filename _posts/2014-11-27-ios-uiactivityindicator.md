---
layout: post
title: The sneaky UIActivityIndicator
date: 2014-11-26 16:23:00
---

I'd been starting at the screen, building rage, for almost a minute. The `UIActivityIndicator` I added as the `rightView` of a `UITextField` to give the user a visual indicator of a query to the server being in progress wasn't on screen. Not. At. All.

I did the setup via code, no Interface Builder, but I was 100% sure that the view should have been there.

I took a 5 minutes break and then check the `UIActivityIndicator` documentation more carefully.

And that's when I realised the sneakiness!

```swift
var hidesWhenStopped: Bool
```

>  If the value of this property is YES (the default), the receiver sets its hidden property (UIView) to YES when receiver is not animating. If the hidesWhenStopped property is NO, the receiver is not hidden when animation stops. You stop an animating progress indicator with the stopAnimating method.

_"[...] the receiver sets its hidden property to `YES` when the receiver is not animating."_ And guess what... I didn't change the `hidesWhenStopped` value to `NO`, nor call the `startAnimating` method. And because of this sneaky behaviour the indicator was hidden.



