---
layout: post
title: Debugging an sqlite3 failure afert upgrading with CocoaPods
date: 2015-02-02 20:59:42
---

In the book [The Passionate Programmer](https://pragprog.com/book/cfcar2/the-passionate-programmer) author Chad Fowler suggest that developers should keep a _Panic Journal_, where they track moments when they were faced with bugs and "paniked", and how they finally came out of the pile of poo. I think this is incredibly helpful, and here's my first panic post in my tech journal.

One of my clients is using an [SQLite](https://www.sqlite.org/) database in their app, and they're using the [`sqlite3`](https://www.sqlite.org/download.html) library to interface with it.

They were using a drag-and-dropped, out of date, version of the library, so one of the first things I did was replacing it with a [CocoaPods installed one](https://github.com/clemensg/sqlite3pod).

Aaaand... the database stopped working!

The first step of my debugging process was **understanding if the library change was responsible for the failure**. Thanks to my _small atomic commits_ I was able to prove that that was the reason for the bug.

The second step was **identifing the failing method**. It turned out to be this call:

```objc
if(sqlite3_prepare_v2(database, [strSelectSql UTF8String], -1, &selectStatement, NULL) == SQLITE_OK) {
  // ...
}
```

The returned value wasn't `SQLITE_OK`. This was yet another proof that the database setup was now wrong. 

The next step was **checking the changelog** to make sure there were no breaking changes between the version of the library the project was using and the new one I just installed. This ended up being a dead end, but I was expecting it, because the version numbers were different only at the patch level.

I then checked in the library repo if there was any issue or PR regarding this, but with no luck.

I finally **googled the error on the method** and found out this [Stack Overflow question](http://stackoverflow.com/questions/17540340/sqlite3-prepare-v2-sqlite-ok), with one of the answers suggesting to use the `sqlite3_errmsg` method to get more information on the real error.

This was probably one of the key steps of the debugging process: **get more information on the error**.

The result of calling that method was `"no such module: FTS4"`. I had no idea of what that ment, but again Google came to the resque.

> FTS3 and FTS4 are SQLite virtual table modules that allows users to perform full-text searches on a set of documents. The most common (and effective) way to describe full-text searches is "what Google, Yahoo, and Bing do with documents placed on the World Wide Web"

Reading more about it and how to use it I discovered that the FTS4 module can be through a flag. It seemed strange to me that I had to manually do that on a library installed via CocoaPods, specially for such a useful, and I think common, feature. So I went back to the pod repo, had a look around and discovered that the pod is actually organised with many subspecs, one for each option.

There we go then, I replaced the naive `pod 'sqlite3'` with a more specific `pod sqlite3/fts` one, and everything went back to normal.
