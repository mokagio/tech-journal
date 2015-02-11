---
layout: post
title: To break or continue in Objective-C
date: 2015-02-12 7:7:42
---

Nope, this is not a post on the choice to continue working in Objective-C or switching to Swift. It's just a post about the `break` and `continue` control transfer statements.

Yesterady I made a silly mistake that was promptly highlighted by the junior developer that used the silly code I wrote. A good reminder that there is always something to learn from everybody.

I had to work with a piece of code like this:

```objc
/**
 * Given an array of team codes pairs, create a Team object for each element, and a Match with the Team pair.
 */
NSMutableArrary *matches = [NSMutableArray array];
for (NSArray *teamCodes in defaultMatchesWithOnlyCodes) {
  Team *firstTeam = [TeamService teamForCode:teamCodes[0]];
  Team *secondTeam = [TeamService teamForCode:teamCodes[1]];

  Match *match = [Match matchWithTeam:firstTeam andTeam:secondTeam];
  [matches addObject:match];
}
```

That `defaultMatchisWithOnlyCodes` array came from a file read from disk, and for some reason some of the values stored in it where `nil`. This resulted in a crash later on when we tried to persist the `matches` array.

What I decided to do was to add a quick and simple check for `nil`:

```objc
NSMutableArrary *matches = [NSMutableArray array];
for (NSArray *teamCodes in defaultMatchesWithOnlyCodes) {
  Team *firstTeam = [TeamService teamForCode:teamCodes[0]];
  if (!firstTeam) { break; }

  Team *secondTeam = [TeamService teamForCode:teamCodes[1]];
  if (!secondTeam) { break; }

  Match *match = [Match matchWithTeam:firstTeam andTeam:secondTeam];
  [matches addObject:match];
}
```

This code is logically incorrect, and the reason would have been clear to me if my control flow statements knowledge hadn't been so rusty.

> `break`, when encountered in a for loop, makes the loop terminate and the execution of the program continue from the next instruction after the loop.

Using `break` like I did means that if the very first check for `nil` fails the loop is terminated, and non of the next, possible valid, iteration is executed, loosing all the data.

The correct statement to use would have been `continue`, in fact:

> `continue`, when encountered in a for loop, terminates the current iteration of the loop jumping to the conditional statement and increment step for the next iteration.

```objc
NSMutableArrary *matches = [NSMutableArray array];
for (NSArray *teamCodes in defaultMatchesWithOnlyCodes) {
  Team *firstTeam = [TeamService teamForCode:teamCodes[0]];
  if (!firstTeam) { continue; }

  Team *secondTeam = [TeamService teamForCode:teamCodes[1]];
  if (!secondTeam) { continue; }

  Match *match = [Match matchWithTeam:firstTeam andTeam:secondTeam];
  [matches addObject:match];
}
```

That's it. Now the code is correct and I don't look like a rookie anymore.

### Food for thoughts

* How could we rewrite the for look to avoid even having to thing about control flow statetmens?
* How does `break` and `continue` work in Swift
