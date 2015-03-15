---
layout: post
title: Javascript leading bang functions in Jade template
date: 2015-03-15 15:44:42
---

Last night I [added a share on Twitter button](https://github.com/mokagio/mokacoding/commit/1a4f3e0029b69e2ee0842a32ac018f165a8b169d) to the [mokacoding blog](http://www.mokacoding.com/).

[mokacoding](http://www.mokacoding.com/) runs on [metallo.js](https://github.com/mokagio/metallo.js) an [experiment of mine](http://www.mokacoding.com/blog/why-i-shouldnt-have-stopped-blogging-with-jekyll/) to blog using markdown files as the source, and jade templates for the final UI.

The way to add the Twitter Tweet button is through an `a` with a bit of javascrpit following it:

```html
<a class="twitter-share-button"
  href="https://twitter.com/share"
  data-url="https://dev.twitter.com/web/tweet-button">
  Tweet
</a>
<script>
  !function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');
</script>
```
Which I translate in Jade like this:

```jade
a.twitter-share-button(
  href="https://twitter.com/share"
  "data-url"="https://dev.twitter.com/web/tweet-button")= Tweet
script
  !function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');
```

It all compiled but I got this warning: `Warning: missing space before text for line <line> of jade file ...` where `<line>` was the line number of the `!function() { ... }()`.

But... what is a `!function() { ... }()` in the first place?

This kind of syntax is a more concise way that `(function() { ... })();` to write a _sefl-executing anonymous function_.

But why does it generate that warning when used in the Jade template?

Playing around a bit with Jade I found out that yes, it's a very powerful and neat template engine, but it have some suttle issues when parsing inline content.

[This SO answer](http://stackoverflow.com/questions/22181813/jade-missing-space-before-text-for-line-x-of-jade-file) opened my eyes on the solution. Since the warning is a parsing waring there must be have been something wrong with the way I wrote the template.

Jade offers the option to add a `.` after a tag to make the parser interpret the content as a _block of text_. Adding that to my code removed the issue :)

```jade
a.twitter-share-button(
  href="https://twitter.com/share"
  "data-url"="https://dev.twitter.com/web/tweet-button")= Tweet
script.
  !function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');
```

The [example](http://jade-lang.com/reference/plain-text/) used in Jade's documentation for the `.` is actually of a piece of Javascript, I cannot rant about this :)

### Food for thought

* How do self-executing anonymous funcionts work? How are they possible?
* Why was the Jade generating that error? [This](http://jade-lang.com/reference/code/) might be related.


