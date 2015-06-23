---
layout: post
title: How to delete a git tag from local and remote
date: 2015-06-24 7:42:42
---

Deleting a tag from a repo is a very unusual task, and this is why is easy to forget how to do it.

Given a tag `my-tag`, you can delete it from the local repo like this:

```
git tag -d my-tag
```

And from a remote named i.e. `origin` like this:

```
git push origin :refs/tags/my-tag
```

And here's a one liner:

```
tag="my-tag"; git tag -d $tag; git push origin :refs/tags/$tag
```

