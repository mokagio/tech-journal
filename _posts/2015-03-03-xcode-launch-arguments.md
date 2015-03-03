---
layout: post
title: How to access iOS apps' launch argumens and environment variables from code
date: 2015-03-03 18:51:42
---

It's easy to forget that at the end of the day our apps are process started on the os platform. As such they can be passed launch arguments and environment variables.

## How to set launch arguments

Setting a launch argument or environment variable can be done from Xcode, through the "Edit Scheme..." menu, in the "Arguments" tab.

There you'll see two sections, "Arguments Passed On Launch" and "Environment Variables", guess what they do?

Launch arguments have only one input field, and need a `-` at the start, for example `-aLaunchArgument`. Environment variables have instead two fields, one for the name and one for the value.

I'll forward you to the [NSHipster's article](http://nshipster.com/launch-arguments-and-environment-variables/) on the topic for a more detailed look at how to set these options.

## How to access launch arguments from code

What's interesting is to have _your own_ arguments or environment variables to use when debugging. For example the reason I found out about this technique is that I needed to programmatically disable the onboarding when running acceptance tests on an app I'm working on.

As said at the start, our app are processes running on the os, so it makes sense that to access these options we need to use `NSProcessInfo`.

```objc
if ([[[NSProcessInfo processInfo] arguments] containsObject:@"-skip_onboarding"]) {
    // Load main screen
}
```

If you are developing on iOS (_haven't tried on OS X_) launch arguments will be set in the `NSUserDefaults` as well, so the above is equivalent to

```objc
if ([[NSUserDefaults standardUserDefaults] valueForKey:@"skip_onboarding"]) {
    // Load main screen
}
```

As for the environment variables, we can read them like this:

```objc
NSDictionary *environment = [[NSProcessInfo processInfo] environment];
if (environment[@"server_url"]) {
    // Set server url with the value in the environment
} else {
    // Set the default one
}
```

It's as simple as this.

### Food for thoughts

* Can launch arguments and environment variables be passed/set when using the Xcode command line tools?
* How does `NSProcessInfo` work? What else can we use it for?
* Is relying on launch arguments and environment variables really a clean way to modify the system behaviour for testing, or can we use better techniques?
