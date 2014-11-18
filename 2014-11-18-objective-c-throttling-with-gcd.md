# Throttling a block execution using GCD

I've been using [this category](https://gist.github.com/jwilling/6012128) to throttle a query request to the server, fired from a search bar.

It uses GCD, which I'm not super familiar with, so I promised myself to come back onto it and have a line by line look at it.

```objc
@implementation NSObject (Throttling)

+ (NSMutableDictionary *)mappingsDictionary {
    static NSMutableDictionary *mappings = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        mappings = [NSMutableDictionary dictionary];
    });

    return mappings;
}

+ (void)runBlock:(void (^)())block withIdentifier:(NSString *)identifier throttle:(CFTimeInterval)bufferTime {
    if (block == NULL || identifier == nil) {
        NSAssert(NO, @"Block or identifier must not be nil");
    }

    dispatch_source_t source = self.mappingsDictionary[identifier];
    if (source != nil) {
        dispatch_source_cancel(source);
    }

    source = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, dispatch_get_main_queue());
    dispatch_source_set_timer(source, dispatch_time(DISPATCH_TIME_NOW, bufferTime * NSEC_PER_SEC), DISPATCH_TIME_FOREVER, 0);
    dispatch_source_set_event_handler(source, ^{
        block();
        dispatch_source_cancel(source);
        [self.mappingsDictionary removeObjectForKey:identifier];
    });
    dispatch_resume(source);

    self.mappingsDictionary[identifier] = source;
}

@end
```

Here's a line by line look at the code.

```objc
if (block == NULL || identifier == nil) {
    NSAssert(NO, @"Block or identifier must not be nil");
}
```

This first line simply checks for the `block` and `identifier` validity.

```objc
dispatch_source_t source = self.mappingsDictionary[identifier];
```

After that it gets a `source` object, of type `dispatch_source_t`. I assume `_t` stands for type.

Now, the [Apple Concurrency Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html) tells us:

> A dispatch source is a fundamental data type that coordinates the processing of specific low-level system events.

Ok, gotcha!

`source` is retrieved from a mappings dictionary, which is lazy loaded and made a _singelton_ through the use of `dispatch_once`.

```objc
if (source != nil) {
    dispatch_source_cancel(source);
}
```

If `source != nil` then `dispatch_source_cancel(source)` is called. It's easy to guess what this method does, but let's check the [documentation](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/index.html#//apple_ref/c/func/dispatch_source_cancel) anyway:

> Asynchronously cancels the dispatch source, preventing any further invocation of its event handler block. Cancellation prevents any further invocation of the event handler block for the specified dispatch source, but does not interrupt an event handler block that is already in progress. The optional cancellation handler is submitted to the target queue once the event handler block has been completed.

So that means that in case we get be a `source` from the mappings that source is cancelled.
This is the basic of the throttling: if the method is called twice in a row within the time buffer, the action scheduled by the first call is unscheduled.

```objc
source = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, dispatch_get_main_queue());
```

Once the throttle check is done, the `source` is (re)assigned, using the `dispatch_source_create` function. The arguments are these:

* _`type`_:`DISPATCH_SOURCE_TYPE_TIMER`: A dispatch source that submits the event handler block based on a timer. The handle is unused (pass zero for now). The mask is unused (pass zero for now).
* _`handle`_: `0` because of the timer type.
* _`mask`_: `0` because of the timer type.
* _`queue`_: `dispatch_get_main_queue()` The dispatch queue to which the event handler block is submitted.

`dispatch_get_main_queue()` returns the serial dispatch queue associated with the application’s main thread. _I love Apple's APIs because of how expressive they are!_

So what this line of code does is getting a dispatch source that will submit its the block based on a timer on the main main thread. Makes sense.

```objc
dispatch_source_set_timer(source, dispatch_time(DISPATCH_TIME_NOW, bufferTime * NSEC_PER_SEC), DISPATCH_TIME_FOREVER, 0);
```

The `dispatch_source_set_timer` method "Sets a start time, interval, and leeway value for a timer source."

* _`source`_: `source`
* _`start`_: `dispatch_time(DISPATCH_TIME_NOW, bufferTime * NSEC_PER_SEC)`: The start time of the timer. 
* _`interaval`_: `DISPATCH_TIME_FOREVER`
* _`leeway`_: `0`: The amount of time, in nanoseconds, that the system can defer the timer.

One note the `start` parameter: The start time parameter also determines which clock is used for the timer. If the start time is `DISPATCH_TIME_NOW` or is created with `dispatch_time`, the timer is based on `mach_absolute_time`.

Now let's have a look at `dispatch_time(DISPATCH_TIME_NOW, bufferTime * NSEC_PER_SEC)`.

`dispatch_time`: Creates a `dispatch_time_t` relative to the default clock or modifies an existing `dispatch_time_t`. Pass `DISPATCH_TIME_NOW` to create a new time value relative to now.

This simply sets the get a number of nanoseconds from now equal to the given `buffer`.

```objc
dispatch_source_set_event_handler(source, ^{
    block();
    dispatch_source_cancel(source);
    [self.mappingsDictionary removeObjectForKey:identifier];
});
```

Sets the handler block for `source`. The event handler is submitted to the source's target queue in response to the arrival of an event.

This means that in our case the event handler will be submitted, therefore execute, when the time set by `dispatch_source_set_timer`.

The handler block itself does three things:

1. Runs the `block`.
2. Cancels the `source`.
3. Removes it from the mapping dictionary.

In one sentence: runs the block and removes its reference, as its not needed anymore.

```objc
dispatch_resume(source);
```

At first glance this doesn't make sense. Why would we _resume_ something that hasn't been started yet? But the documentation of the method have the answer:

> New dispatch event source objects returned by dispatch_source_create have a suspension count of 1 and must be resumed before any events are delivered. This approach allows your application to fully configure the dispatch event source object prior to delivery of the first event. In all other cases, it is undefined to call dispatch_resume more times than dispatch_suspend, which would result in a negative suspension count.

```objc
self.mappingsDictionary[identifier] = source;
```

Finally we add `source` to the map, using `identifier` as its key.