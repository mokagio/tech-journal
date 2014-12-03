---
layout: post
title: Git rev-list and rev-parse
date: 2014-12-03 16:25:42
---

I've setup a very convenient automated build number generation for iOS projects, following [this post](http://blog.jaredsinclair.com/post/97193356620/the-best-of-all-possible-xcode-automated-build) and using the script in [this gist](https://gist.github.com/johankool/c33cffc0727b13f22f25).

The build number is evaluated by counting the number of commits on the current branch, which is very neat since that number is always gonna increase.

Unless... you are in feature branch and decide to rebase! To tackle this issue the script appends the branch name to the build number. _This is done only in Debug mode, because it's assumed you won't build a Release build from anywhere else than master._

All this is possible thanks to Git awesomeness. The commands used are:

* `git rev-list --all`
* `git rev-parse --abbrev-ref HEAD`

### `GIT-REV-LIST(1)`

> Lists commit objects in reverse chronological order

The basic usage is `git rev-list <commit>...`. 

And what it does is basically listing all commits reachable traversing the parent links form the give commit, or list of commits. It can exclude from the list all the commits reachable from a specified by adding a `^` commit in front of it, or in a range (`..` and `...`) of commits.

It also has a big bunch of options and it's used in commands as different as `merge` and `bisect`.

The `--all` option:

> Pretend as if all the refs in refs/ are listed on the command line as `<commit>`.

The `refs/` above is a folder in `.git`, and contains all the [references](http://git-scm.com/book/en/v2/Git-Internals-Git-References).

What I assume `--all` does then is pretending we've added all the commits by hand. _But I might be wrong._

### `GIT-REV-PARSE(1)`

> Pick out and massage parameters

Say what?!

> Many Git porcelainish commands take mixture of flags (i.e. parameters that begin with a dash -) and parameters meant for the underlying `git rev-list` command they use internally and flags and parameters for the other commands they use downstream of `git rev-list`. This command is used to distinguish between them.

Porcelainish??? So basically `rev-parse` is used to make parse (derp!) parameters and flag meant for other commands.

I guess then that the real deal in this line will be `--abbrev-ref HEAD`. The `--abbrev-ref` option is listed under the _"Options for Output"_ and what it does is returning:

> A non-ambiguous short name of the objects name. The option core.warnAmbiguousRefs is used to select the strict abbreviation mode.

I suppose this option is used for safely retrieve names under the hoods of some other git commands, and it's used in the script as a way to retrieve the name of the current branch.

---

Every time I look into how git works I'm blown away!