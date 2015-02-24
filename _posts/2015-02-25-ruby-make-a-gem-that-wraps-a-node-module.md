---
layout: post
title: How to make a Ruby gem that wraps a Node module
date: 2015-02-25 7:38:42
---

An alternate title for this post could be: **How to run non Ruby executable from a gem**, or **How to wrap a C executable, in a Node module, then wrap it again in a Ruby gem**,  but seriously it should be: **How to do something not very smart**.

In one of my client projects we are running acceptance tests using Calabash. The process is quite smooth but there is a bottle neck: if you want to run the tests on a device you need to install the latest build embedding the Calabash server manually.

This is quite annoying, and because of the manual labour involved is not really portable in a CI environment.

Lucky enough the [PhoneGap](http://phonegap.com/) community has a tool called [ios-deploy](https://github.com/phonegap/ios-deploy) that enables the deploy of an ipa to a connected device from the command line. As a note: ios-deploy is a fork of an older project called [fruitstrap](https://github.com/ghughes/fruitstrap) that is no longer maintained but still pops up when googling something like "run on device from command line".

Being PhoneGap a Javascript community ios-deploy is distributed via npm. Now, I love Node.js, and io.js, but the usual iOS project already has to deal with a `Podfile`, and if you want to handle your `CocoaPod` version properly a Gemfile too. Adding a `package.json` would be too much.

So what I did is packaging the ios-deploy node module into a gem. It all was possible because what the module actually does is packaging an executable file, generated from a C source.

This sounds awful, **and it is awful**, and you should think twice before doing it. But let's go on for the sake of sharing the experience...

After generating the gem scaffold with `bundle gem ios-deploy`, and `npm install ios-deploy`, my first step has bee to create the `bin` folder a naively simlinking the executable there.

This of course didn't work, the error I got was:

```
invalid multibyte char (UTF-8) (SyntaxError)
```

Which is an error a lot of people used to get before Ruby 2, and happens when the file is missing the encoding definition.

Of course! We're talking about Ruby gems here, and the executable in the `bin` folder has to be a ruby file.

The second step then was to create a very simple Ruby file that simply runs the executable form the `node_modules/ios-deploy` folder:

```ruby
absolute_pwd = File.expand_path(File.dirname(__FILE__))
exec "#{absolute_pwd}/../node_modules/ios-deploy/ios-deploy"
```

This worked fine, but when running something like `ios-deploy --list` the `list` option wasn't passed to `ios-deploy`. The reason is simple, the options are passed to the _Ruby_ executable, not to the _real_ one.

The fix for this was to pass down the options to the `ios-deploy` invocation made with `exec`, and this is one way to do it:

```ruby
absolute_pwd = File.expand_path(File.dirname(__FILE__))
command = "#{absolute_pwd}/../node_modules/ios-deploy/ios-deploy"
ARGV.each do |parameter|
  command += " #{parameter}"
end
exec command
```

That's it folks, how to wrap in a Ruby gem a Node module that wraps a C executable.

If you're curious about this the gem is acutally published, and you can find it [here](https://github.com/mokagio/ios-deploy-gem).

### Food for thoughts

* Instead of writing dirty stuff like this, wouldn't it be better if `ios-deploy` was distributed via homebrew?
* Can the code be written in a better way? I don't think `exec` is the best way to run shell commands from a Ruby script.
