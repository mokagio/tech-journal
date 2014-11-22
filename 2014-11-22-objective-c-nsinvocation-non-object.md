# Using NSInvocation with non object arguments

```objc
SEL selector = ...;
NSMethodSignature *signature = [someObject methodSignatureForSelector:selector];
NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
invocation.selector = selector;
invocation.target = ...;
[invocation setArgument:&floatValue atIndex:2];
```

