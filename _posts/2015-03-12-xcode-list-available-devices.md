---
layout: post
title: How to list devices available to Xcode
date: 2015-03-12 5:53:42
---

Using `xcodebuild`, or better [`xcrun xcodebuild`](http://mokagio.github.io/tech-journal/2015/03/10/xcode-xcrun.html) we can do things such as [running tests from the terminal](http://www.mokacoding.com/blog/running-tests-from-the-terminal/).

```
xcrun xcodebuild \
  -workspace "Coconut.workspace" \
  -scheme "Coconut" \
  -sdk iphonesimulator \
  -destination 'platform=iOS Simulator,name=iPhone 6,OS=8.1' \
  test
```

But what to write in the `-destination` parameter? How do we know the available devices that can be used as parameter?

The `instruments` cli tool from Apple has an answer to that.

`instrumenst` "_runs an Instrument template from the command line_", and one of the available options to pass is is `-s devices`, to "_show list of known devices, and then exit_".

```
instruments -s devices

Known Devices:
Gio’s MacBook Pro [<uuid>]
Gio's iPhone 6 (8.2) [<uuid>]
Resizable iPad (8.2 Simulator) [<uuid>]
Resizable iPhone (8.2 Simulator) [<uuid>]
iPad 2 (8.2 Simulator) [<uuid>]
iPad Air (8.2 Simulator) [<uuid>]
iPad Retina (8.2 Simulator) [<uuid>]
iPhone 4s (8.2 Simulator) [<uuid>]
iPhone 5 (8.2 Simulator) [<uuid>]
iPhone 5s (8.2 Simulator) [<uuid>]
iPhone 6 (8.2 Simulator) [<uuid>]
iPhone 6 Plus (8.2 Simulator) [<uuid>]
```

There you go, here's the list of known devices to the computer, and any of can be used as the parameters for the `-destination` argument.

### Food for though

* What other cool things can we do with instruments?
* How to use [`pick`](https://github.com/thoughtbot/pick) to select the destination parameter interactively
