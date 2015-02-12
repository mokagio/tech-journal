---
layout: post
title: Copy then paste multiple times in Vim
date: 2015-02-12 22:9:42
---

Today I found myself having to do a bit of copy-paste in Vim, and found out that the paste operation _has no memory_, meaning that once you `p`aste something, if you `p`aste again nothing is gonna happen.

But I needed to paste the same word multiple time all over my file. [This](http://stackoverflow.com/questions/7163947/vim-paste-multiple-times) StackOverflow question came to the resque.

```
xnoremap p pgvy
```

But what does this combo actually do?

`p` pastes, `gv` reselects the previous Visual area, and finally `y` yanks (copies) what's selected, which is the previous Visual area, which is what we just pasted.

As an aside, the way I found out about `gv` was by using `:help gv`, Vim is sooo cool!

After all this a real pro Vim user could tell me that that's an abuse, and that Vim offers better ways to do it, but that's a question for the food for thoughts section.

### Food for thoughts

* Is there a better Vim way to paste multiple times?
* What does `xnoremap` does?
