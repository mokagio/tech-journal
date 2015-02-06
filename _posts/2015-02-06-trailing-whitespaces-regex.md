---
layout: post
title: A RegEx to highlight trailing whitespaces in Vim
date: 2015-02-06 20:30:42
---

Today I had a very interesting chat with [Timothy Mukaibo](https://twitter.com/TimothyMukaibo) about vim, and of course we ended up exchanging `.vimrc` tips.

One of the interesting bits of his `.vimrc` is that any extra whitespace at the end of a line are highlighted. I found this very useful and added it to mine straightaway!

<blockquote class="twitter-tweet" data-cards="hidden" lang="en"><p>Improving my vimrc cherry picking from <a href="https://twitter.com/TimothyMukaibo">@TimothyMukaibo</a>&#39;s one. Thanks mate for sharing! 👍 <a href="https://t.co/vBornRfIeJ">https://t.co/vBornRfIeJ</a></p>&mdash; Giovanni Lodi (@mokagio) <a href="https://twitter.com/mokagio/status/563503650618605568">February 6, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The extra whitespaces are picked through a RegEx, and in my ongoing quest to sharpen my RegEx skills I'm now gonna look at it:

```
/\s\+\$/
```

* `\s` is the matcher for a single whitespace character
* `\+` is the "greedy" one or more quantifier
* `\$` matches the end of the line

Summing it all up we get "match one or more whitespace characters at the end of the line", which is exactly what we want.

### Food for thoughts

* Understand the syntax used to set this in the `.vimrc`
* Understand why if the `hi` instruction comes after the `match` one then the trick doesn't work
* Try to find out if it's possible to have it running only in normal mode
