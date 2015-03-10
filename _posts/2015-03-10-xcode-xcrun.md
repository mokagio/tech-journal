---
layout: post
title: xcrun, the version manager for the Xcode toolchain
date: 2015-03-10 21:55:42
---

I'd been aware of `xcrun` existence for a while, and so it used in tools like shenzen, but it wasn't till I saw [this PR on specta](https://github.com/specta/specta/pull/135) that I understood the true power of this tool.

> xcrun  provides  a means to locate or invoke developer tools from the command-line, without requiring users to modify Makefiles or otherwise take inconvenient measures to support multiple Xcode tool chains.

> The  tool  xcode-select(1) is used to set a system default for the active developer directory, and may be overridden by the  DEVELOPER_DIR  environment  variable  (see ENVIRONMENT).

This means that people maintaing a library's compatibility with older versions of Xcode, or developing against a beta, can easily run their tests by using the `DEVELOPER_DIR` and `SDKROOT` environment variables like in the example from the mentioned PR:

```
$ DEVELOPER_DIR=/Xcode/5.1.1/Xcode.app rake test
$ DEVELOPER_DIR=/Xcode/6.1.1/Xcode.app rake test
$ DEVELOPER_DIR=/Xcode/6.2/Xcode-Beta.app rake test
$ DEVELOPER_DIR=/Xcode/6.3/Xcode-Beta.app rake test
```

Pretty handy hey!

I've made a mental note to update all my build tools to use `xcrun` o

I'm gonna update the build tools in my projects to use `xcodebuild` through `xcrun`, and always use it from now on.

# Food for thought

* What other things can be done with `xcrun`?
* Is there a way to show all the avilable sdks and valid `DEVELOPER_DIR` locations?
