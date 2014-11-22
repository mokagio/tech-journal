---
layout: post
title: Using NSInvocation with non object arguments
date: 2014-11-22 00:00:00
---

```objc
SEL selector = ...;
NSMethodSignature *signature = [someObject methodSignatureForSelector:selector];
NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
invocation.selector = selector;
invocation.target = ...;
[invocation setArgument:&floatValue atIndex:2];
```

