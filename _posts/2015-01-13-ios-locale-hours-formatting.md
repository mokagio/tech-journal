---
layout: post
title: Reliable locale based hours formatting with NSDateFormatter
date: 2015-01-13 17:55:42
---

How do we make sure that a formatted date in our iOS application will show 23:10 or 11:10 PM depending on the user's settings?

`NSDateFormatter` comes with a very nice class method to generate a format string given a template and a locale (and options that).

```swift
class func dateFormatFromTemplate(_ template: String,
                          options opts: Int,
                           locale locale: NSLocale?) -> String?
```

> A localized date format string representing the date format components given in template, arranged appropriately for the locale specified by locale.

But this is not enough, we need to use the `jj` symbol. The [Unicode Date Format Patterns](http://www.unicode.org/reports/tr35/tr35-31/tr35-dates.html#Date_Format_Patterns) documentation says that:

> it requests the preferred hour format for the locale (h, H, K, or k)

For the record:

* `h` matches the 12-hour-cycle, and goes from 1 to 12
* `H` matches the 24-hour-cycle, and goes from 0 to 23
* `K` matches the 12-hour-cycle, and goes from 0 to 11
* `k` matches the 24-hour-cycle, and goes from 1 to 24

_I see that the capital letters start from 0, but why didn't they keep h for 12 and k for 24?_

That's it!

```swift
import Cocoa

let now = NSDate()

let aTwentyFourHoursLocale = NSLocale(localeIdentifier: "nl_NL")
let aTwelveHoursLocale = NSLocale(localeIdentifier: "us_US")

let dateFormatter = NSDateFormatter()
dateFormatter.dateFormat = NSDateFormatter.dateFormatFromTemplate("jj", options: 0, locale: aTwentyFourHoursLocale)

let twentyFourHourString = dateFormatter.stringFromDate(now)
// => a 24 hours formatted hour (18)

dateFormatter.dateFormat = NSDateFormatter.dateFormatFromTemplate("jj", options: 0, locale: aTwelveHoursLocale)

let twelveHourString = dateFormatter.stringFromDate(now)
// => a 12 hours formatted hour (6 PM)
```
