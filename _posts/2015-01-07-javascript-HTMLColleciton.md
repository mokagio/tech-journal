---
layout: post
title: HTMLCollection Javascript interface
date: 2015-01-09 07:42:42
---

While researching why the naive code in ["Get links in page without using jQuery"](http://mokagio.github.io/tech-journal/2014/12/07/javascript-get-links-without-jquery.html) wasn't working I found out about Javascript's `HTMLCollection` interface.

> An `HTMLCollection` in the HTML DOM is live; it is automatically updated when the underlying document is changed.

`HTMLCollection` is very simple, as it has only 3 methods: `length`, `item`, and `namedItem`.

What I find interesting about it is that it's _live_ feature, as in it updates automatically.

Another interesting bit is the `namedItem` method, and the fact that doing

```js
collection.namedItem("name")
```

is the same as

```js
collection["name"]
```

And the same holds true for `item`.

#### Food for thoughts:

* This `item` to `[]` sugar works in `NodeList` and `Array` too. How does Javascript make it possible?
* Why is `HTMLCollection` defined as a Javascript interface instead of a type in the documentation page? Is it just a figure of speech or there is something different it the implementation?

