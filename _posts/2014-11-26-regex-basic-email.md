---
layout: post
title: Regular Expression to "validate" an email address
date: 2014-11-26 08:30:00
---

I tweeted about an interesting blog post regarding Regex and email validation.

<blockquote class="twitter-tweet" lang="en"><p>Just stop validating email addresses <a href="http://t.co/Cmbfw8zIHb">http://t.co/Cmbfw8zIHb</a></p>&mdash; Giovanni Lodi (@mokagio) <a href="https://twitter.com/mokagio/status/537059238036520961">November 25, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The point of the post makes is _**don't even try to validate an email address**_ because basically everything with one @ is a valid email address.

Nevertheless there are several Regexs in there that I'm not familiar with, so lets dig deeper!

```ruby
/\A[^@]+@([^@\.]+\.)+[^@\.]+\z/
```

`\A` and `\z` are patterns to match the beginning and end of a string. 

`[^@]` is a character set `[]` for any character except `^` the character `@`.

`+` means: match one or more of the preceding token. So in our case one or more set of characters that don't include `@`.

`@` simply matches `@`.

**Recap:** so far we're matching the start part of the email address and it's @.

`([^@\.]+\.)+` deserves to be split:

* `()` is a capturing group, and it _groups multiple tokens together and creates a capture group for extracting a substring or using a backreference._ That's why it's followed by `+`.

* `[^@\.]+` as seen before is a character set for any character except `@` and `\.`. `\.` stands for is the escaped `.`, we need to escape it because `.` is a special Regex character that _matches any character except line breaks_. Again we have the `+`, which we explained already.

* `\.` at the end of any number of group of characters that do not include `@` or `.` we need to have a `.`.

**Recap:** this second part matches the second half of the email address, and says any number of groups of characters that doesn't include `@` or `.` and end with `.`. Something like mokagio42@_email.provider.that.is.nice._com.

* `[^@\.]+` we've already seen all the elements of this group, and it's function is to match the "termination" of the email address.

#### Wrapping it up

Given an email address like `moka.gio42+tech-journal@e-mail.address.com` we have:

* `moka.gio42+tech-journal` matched by `[^@]+`
* `@` matched by `@`
* `e-mail.address.com` matched by `([^@\.]+\.)+[^@\.]+`
