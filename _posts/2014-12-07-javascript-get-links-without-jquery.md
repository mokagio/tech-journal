---
layout: post
title: Get links in page without using jQuery 
date: 2014-12-07 12:04:42
---

When I first started messing around with the web I fell in love with jQuery. It felt so powerful and versatile, and simplified the code I was writing a lot.

The idea I have had the time was that Javascript ~= jQuery.

Some time has passed, and now I spend most of my time working in environments where it's clear that there's no _one size fits all_ framework, and that having to import a monolithic massive framework just to do one tiny thing is almost certainly a bad idea.

Recently, working on a static blog based on a template, I had write a bit of Javascript to select all links that are do not target locations in the page and make them `target="_blank"`.

What I did at first was:

```js
$('a:not([href^="#"])').attr("target", "_blank");
```

Please not `a:not([href^="#"])`, which is in my opinion a very nice combo of CSS selectors :)

I was pleased with the one-liner, but the fact that jQuery was involved to do something that simple smelled a bit.

The way to do the same thing in vanilla Javascript require a bit more typing, but is simple nevertheless.

```js
var external_links = document.querySelectorAll('a:not([href^="#"])');
external_links.forEach(function(link) {
    link.setAttribute("target", "_blank"); 
});
```
