---
layout: post
title: Using bundle exec
date: 2015-02-04 21:32:42
---

Handling different Ruby versions and gem dependencies can get out of control quit easily. I recently switched from rvm to rbenv, but this is a story for another post.

While reading about the best practices in using Ruby projects I saw that most people recommend running gem commands using `bundle exec`. 

What is `bundle exec`, and why should we use it to run our commands?

If we look at [`budle-exec(1)`] we read:

> bundle-exec - Execute a command in the context of the bundle

The mentioned bundle is the set of gems installed and manager by [Bundler](http://bundler.io/). Executing a command in the context of the bundle then means that the binary that will run is gonna be the one of the gem specified in the `Gemfile`.

But _executing a command in the context of the bundle_ is important? Let me give you an example. Lately I'm spending most of my time on two iOS projects, on is for a client, and one is personal.I like to always be on the bleading edge of CocoaPods, so I've installed the latest beta, 0.36.0.beta.2. This is not something I would do with this client though, with them I'm a very responsible developer which plays it safe, so I want to run on the latest stable, 0.35.0. By using a Gemfile in the projects root folder that specifies the CocoaPods gem version, and running `bundle exec pod ...` instead of simply `pod ...` I'm sure that the right version will be used.

Using a Gemfile is the best practice when working on projects on a team (or even by yourself). Using `bundle exec` is the best practice to run commands in a reliable and confident context.

