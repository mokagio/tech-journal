---
layout: post
title: Closer look to the basic Calabash RegExs
date: 2015-01-30 10:35:42
---

One of the tasks in my current project is writing acceptance tests for an iOS app using Cucumber and Calabash. It's quite cool to be able to mix Ruby and Objective-C. _Nope, on Swift yet, enterprise client... Who knows when they'll start using it._

Cumubers steps can be written using Regular Expressions, which is quite handy, and also gives me a way to hone my RegEx skills, which are quite limited at the moment.

Here's a closer look at some of the matchers used in the predefined steps from [calabash_steps.rb](https://github.com/calabash/calabash-ios/blob/develop/calabash-cucumber/features/step_definitions/calabash_steps.rb)

### `([^\"\*)`

```ruby
Then /^I toggle the "([^\"]*)" switch$/ do |name|
  ...
end
```

`^\"` will match any character that's not `"`, `[ ]*` will match any number of characters that are not `"`, and finally `( )` is simply a capture group. So this RegEx will match everything untill it finds a `"`, and because of the capture group the result will go into the `name` variable.

### `(\d+)`

```ruby
Then /^I press button number (\d+)$/ do |index|
  ...
end
```

`\d` stands for any digit, and `+` is the _1 or more_ quantifier. This expression will match any uninterrupted sequence of digits.

### `(foo|bar)`

```ruby
Then /^I pinch to zoom (in|out)$/ do |in_out|
  ...
end
```

The `|` symbol is the _alteration_, it acts like a logical OR, matching the expression at its left or at right.

### `(?:foo|bar)`

```ruby
Then /^I (?:press|touch) button number (\d+)$/ do |index|
  ...
end
```

The `?:` at the start makes this group _non capturing_, which means that the match will not be retained. In fact you can see that the only variable for the step in the example is `index`.

### `(?:foo)?`

```ruby
Then /^I (?:should)? see text containing "([^\"]*)"$/ do |text|
  ...
end
```

The `?` after the capture group is the optional quantifier, it matches 0 or 1 instances of it's token. This provides flexibility and code reuse in the syntax of the steps, in fact both "I see..." and "I should see..." can be used when writing the feature. 
