---
layout: post
title: Using ReactiveCocoa to parallelize multiple requests
data: 2015-06-17 08:08:42
---

I've finally started using [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa), it was about time!

I just want to share how I used it to parallelize a bunch of network requests, and to notify the user when they're done.

This approach shows two possible ways to use ReactiveCocoa: signals combination, and collection _magic_.

```objc
- (void)viewWillAppear:(BOOL)animated {
  [super viewWillAppear:animated];

  [self showLoadingSpinner];
  [self getFeesForAllItemsWithCompletion:^{
    [self hideLoadingSpinner];
  }];
}

- (void)getFeesForAllItemsWithCompletion:(void (^)())completion {
  NSArray *signals = [self.allItems.rac_sequence map:^id(Item *item) {
    return [self getFeeForItemSignal:item];
  }].array;

  [[RACSignal combineLatest:signals] subscribeCompleted:completion];
}

- (RACSignal *)getFeeForItemSignal:(Item *)item {
  return [RACSignal startEagerlyWithScheduler:[RACScheduler scheduler] block:^(id<RACSubscriber> subscriber) {
      [APIClient getFeeForItemUUID:item.UUID
                           success:^(Fee *fee) {
                             [subscriber sendCompleted];
                           }
                           failure:^(NSError *error) {
                             [subscriber sendCompleted];
                           }];
  }];
}
```

The code above is probably nothing exciting if you're a RecativeCocoa PRO, but I'm quite pleased with it.

* It uses `map` to transform an array of `Item` into one of `RACSignal`.
* The signal creation is encapsulated in it's own method, making the code more readable.
* The method that performs the signal combination receives a completion callback object, rather than doing it itself. This makes it _reusable_ and single purpose.

## Food for thought

* Is this a proper use of ReactiveCocoa? Or is there any best practice or pattern to achieve this in a better way?
* How would this look in Swift 1.2 and ReactiveCocoa 3?
* How would this look in Swift **2** and ReactiveCocoa 3? Any difference?
