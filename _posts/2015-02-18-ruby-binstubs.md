---
layout: post
title: What's a binstub?
date: 2015-02-18 19:21:42
---

I recently started working in Ruby for real again, apparently nowadays using rmv is not cool any more, and people seem to like rbenv.

One of the concepts rbenv introduced me to is [binstubs](https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs).

Reading the wiki we understand that:

> Binstubs are wrapper scripts around executables whose purpose is to prepare the environment before dispatching the call to the original executable.

_Bin-stubs_ did you get it? I didn't at the start, but I'll blame it to the fact that I'm not a native English speaker.

That's about it really... Since in the Ruby world executables might need _extra stuff_ to do their job we use a binstub to make sure that the stage is properly set for them to act as expected.

This example, from the guide linked above, makes it clearer:

```ruby
#!/usr/bin/env ruby
require 'rubygems'

# Prepares the $LOAD_PATH by adding to it lib directories of the gem and
# its dependencies:
gem 'rspec-core'

# Loads the original executable
load Gem.bin_path('rspec-core', 'rspec')
```

### When binstubs come handy

Binstubs come very handy when we want to make sure that all the components of our application run on the same environment.

For [example](https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs#manually-created-binstubs) if you have a Rails application you could manually generate a binstub to run the Unicorn server, this will make sure that the version of Unicorn running is using exactly the same Ruby version and Gemset of the Rails application.

### Food for thoughts

* Look back at older projects and revamp them using binstubs. (_Who am I kidding, I'm not gonna do it..._)
* Use binstubs in new Ruby projects. (_This is more likely!_)
* Look deeper into how rbenv handles binstubs.






