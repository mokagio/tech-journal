---
layout: post
title: NodeList Javascript type
date: 2015-01-07 18:51:42
---

Note: this is a followup to ["Get links in page without using jQuery"](http://mokagio.github.io/tech-journal/2014/12/07/javascript-get-links-without-jquery.html), where I posted a snippet of code that didn't work...

If you use vanilla Javascript methods like `querySelectorAll` beware that the returned type is gonna be a [`NodeList`](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) rather than an `Array` like you, or at least I, would have expected.

A `NodeList` is a _collection of nodes_. Derp.

It behaves very much like an `Array`, but we can't call `forEach` or `map` on it. The reason for this is that this type prototype chain doesn't include `Array`.

There are some [workarounds](https://developer.mozilla.org/en-US/docs/Web/API/NodeList#Workarounds) to use `forEach` on a `NodeList`, but I rather go with an old classic:

```javascript
var external_links = document.querySelectorAll('a:not([href^="#"])');
for (var i = 0; i < external_links.length; i++) {
   external_links[i].setAttribute("target", "_blank");  
}
```