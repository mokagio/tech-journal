---
layout: post
title: Swift Scripting
date: 2015-08-02 10:48:42
---

[Swift](https://developer.apple.com/swift/blog/) is a wonderful programming language. Recently I [submitted a pull request to Swift's framework](https://github.com/thoughtbot/Curry/pull/4), adding a script to generate some code. Being the framework on Swift it was only fitting to have the script written in Swift too.

Writing simple scripts in Swift 2.0 is as easy as making a `script.swift` file and running it using `xcrun swift script.swift`. For example

```swift
// script.swift
print("Hello World")
```

```bash
$ xcrun swift script.swift
Hello World
```

Note that to run a Swift 2.0 script you need to use the version of the SDK that is shipped with Xcode 7. At the time of writing Xcode 7 is in beta, so you need to manually select that SDK:

```bash
$ sudo xcode-select -s /Applications/Xcode-beta.app/Contents/Developer
```

Now for something more useful than an hello world.

## Read the script arguments

```swift
// $ script.swift John

let arguments = Process.arguments // Array of String
let scriptName = Process.arguments[0]
let firstArgument = Process.arguments[1]

print("Hello \(firstArgument)")
```

## Write to a file

We can easily use Apple's frameworks in Swift's scripts

```
import Foundation

let currentDirectory = NSFileManager.defaultManager().currentDirectoryPath
let filePath = currentPath = currentDirectory.stringByAppendingPathComponent("file.txt")

do {
  try "Hello World".writeToFile(filePath, atomically: true, encoding: NSUTF8StringEncoding)
} catch _ {
  print("An error occurred while writing to \(filePath)")
}
```

## Where to go from here

Getting started with Swift scripting is simple, but the tooling is not yet at the level of a language like Ruby, or even simple Bash scripting. To be fair, Swift has been around for just one year, and is not fully open source yet. So what we can do with the language is very impressive.

On top of that, until Swift won't run on Linux, choosing it for scripts makes it not very portable.

I would not recommend using Swift as your main scripting language, but if you need to write simple little scripts in the context of an iOS or OS X project it is a very good option to keep the script in the same language as the project.

