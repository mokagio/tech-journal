---
layout: post
title: Issues with making UITabBar accessible
date: 2015-02-17 6:47:42
---

**TL;DR** if you have a `UITabBar` which buttons don't have titles, the only way to set an accessibility label is to use a dirty hack to actually set the title and hide it.

One of my clients has an app with a tabbed navigation, and the icons in the tab bar don't have titles. So far so good, but to run the acceptance tests I need accessibility labels in order to locate the elements to tap.

To complicate the things the `UITabBarController` comes form a Storyboard.

### The things I tried:

1. Storyboard - failed
2. UIApparence - failed
3. `UITabBarItem` - failed
3. Subclassing

#### Storyboard

There is no option in the Storyboard to set the accessibility label of the tab bar items.

#### UIApparence

If you inspect a tab bar in [Reveal](http://revealapp.com/) you'll notice the buttons are no usual buttons, but `UITabBarButton` instances. This is as far as I can tell a private class, and therefore we cannot directly access it, nor set values using the `UIApparence` protocol.

I decided to drill down in the hierarchy, and to use `UIApparence` on the `UILabel`.

```objc
[UILabel appearanceWhenContainedIn:[[UITabBar class], nil] setTextColor:[UIColor clearColor]];
```

Both this, other combinations of UI components, and using `tintColor` instead, either fail or result in funny behaviours, like the button starting without title, then adding a title when tapped, then keeping the button when another button is tapped.

#### UITabBarItem

I then tried to set the `accessibilityLabel` value of the `UITabBarItem` that the framework uses to configure the view. This failed as well.

#### Subclassing and hacking the view

I finally settled for this dirty hack: the bar button have a title, but it's pushed off screen so that only Voice Over and my acceptance tests know that it's there.

The way I'm doing it is by looping on all the buttons in the `awakeFromNib` method of a `UITabBarController` class made for the occasion, and the reason for that is that it comes from a Storyboard.

```objc
- (void)awakeFromNib {
  [super awakeFromNib];
  [self hideItemTitles];
}

- (void)hideItemTitles {
  [self.tabBar.items enumerateObjectsUsingBlock:^(UITabBarItem *item, NSUInteger idx, BOOL *stop) {
    item.titlePositionAdjustment = UIOffsetMake(0, 1000);
  }];
}
```

I'm quite surprised by the state of accessibility in `UITabBar` a UIKit component that has been there form the start.

### Food for thoughts

* Do more experiments in order to find a nicer way to hide the title in the bar button
* Do more experiments in order to see if using the `UITabBarItems` is actually an option
* What would happen if there was no Storyboard configuring the view? Would it be possible, or at least simpler, to set accessibility?
