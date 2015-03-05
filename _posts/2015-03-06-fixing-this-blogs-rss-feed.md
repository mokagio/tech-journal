---
layout: post
title: Fixing this blog RSS feed
date: 2015-03-06 8:38:42
---

When I tried to subscribe to my own RSS, actually [Atom](http://en.wikipedia.org/wiki/Atom_(standard)), feed I realised **it was broken**.

I have no idea why, and I'm about to _live blog_ my way through fixing it.

I just googled "RSS validator" and the first result is one of those old W3 validator websites that give you a nice badge to add to your page if it passes the validation.

So I'm going to head over to http://validator.w3.org/feed/check.cgi and see what it tells me...

![RSS Validator Warning Screenshot](https://s3.amazonaws.com/tech-journal/rss-validation-warning.jpg)

What?! If my feed is valid why can't I see my new posts using [Newsify](http://newsify.co/)?

Looking better at the report I see that there is actually some yellow there...

> Self reference doesn't match document location

Reading the help for this warning I see that when `rel=self` than the `href` attribut is expected to contain _a resource equivalent to the containing element_.

Looking better at the highlighted line I notice that the URL there is incorrect.

```
<atom:link href="http://mokagio.github.com/tech-journal/tech-journal/feed.xml" rel="self" type="application/rss+xml"/>
```

should actually be

```
<atom:link href="http://mokagio.github.com/tech-journal/feed.xml" rel="self" type="application/rss+xml"/>
```

And I have a confirmation of that by comparing my feed with [NSHipster's one](http://nshipster.com/feed.xml).

### How did this happen?

So, this tech-journal of mine runs on Jekyll. The `feed.xml` template uses this line to generate the Atom link:

```
<atom:link href="{{ "/feed.xml" | prepend: site.baseurl | p    repend: site.url }}" rel="self" type="application/rss+xml"/>
```

In my `_config.yml` I have:

```yml
baseurl: "/tech-journal"
url: "http://mokagio.github.com/tech-journal"
```

Why did I do this? No idea... but it's an easy fix:

```yml
baseurl: "/tech-journal"
url: "http://mokagio.github.com"
```

I just pushed to GitHub, and verified that I can now fetch my articles with the RSS reader app. Mission accomplished.

### The validation still gives the location warning

For the sake of curiosity I just run the validation again and the warning is still there. What the heck?!

But there's actually an answer for this:

```
curl http://mokagio.github.com/tech-journal/feed.xml

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

There you go, there must be some GitHub Pages redirection involved that confuses the validator.

### Food for thought

_This live blogging experiment hasn't gone really well, and I'm kinda bored now, so this posts closing section is just gonna be those questions that I don't feel like investigating at the moment_

* What's the difference between RSS and Atom?
* Is the redirection actually the reason for the validation warning?
* Why did it set `url` and `baseUrl` in such a silly way?
