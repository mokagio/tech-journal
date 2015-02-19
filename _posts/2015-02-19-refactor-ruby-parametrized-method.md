---
layout: post
title: Parametrized method refactoring example
date: 2015-02-19 16:27:42
---

Refactoring should be a daily habit. Having a refactoring day or a "big refactoring" sprint is bullshit. Technical debt should be fought consistently during the development process, not accumulated until it's too big to deal with.

Because refactoring is so important it think it should be practiced deliberately. With this I mean that one should always look at the code with a critical eye, in search of places where it could be improved.

A good place where to find inspiration is ["Refactoring: Improving the Design of Existing Code"](http://martinfowler.com/books/refactoring.html) by Martin Fowler. Another good place is the [refactoring.com](http://refactoring.com/catalog/) website, which comes directly from the content of the book.

I think that if one practices refactoring deliberately, they'll eventually master most of the techniques and apply them effortlessly.

## Parametrized method

The [parametrized method] refactoring technique should be applied when:

> Several methods do similar things but with different values contained in the method body.

And you can implement it like this:

> Create one method that uses a parameter for the different values.

I had this code:

```ruby
Then /^I see the "([^"]*)" title$/ do |title|
  check_element_exists "view:'UINavigationItemView' marked:'#{title}'"
end

Then /^I see the "(.*?)" button$/ do |label|
  check_element_exists "view:'UIButton' marked:'#{label}'"
end

Then /^I see the "(.*?)" label$/ do |label|
  check_element_exists "view:'UILabel' marked:'#{label}'"
end
```

Those cucumber steps, written to run on a calabash iOS tests suite, look similar and do exactly the same thing.

I applied the parametrized method and wrote this

```ruby
Then /^I see the "(.*?)" (title|button|label)$/ do |label, view_type|
  types_to_classes = {
    "title" => "UINavigationItemView",
    "button" => "UIButton",
    "label" => "UILabel"
  }
  view_class = types_to_classes[view_type] || "UIView"

  check_element_exists "view:'#{view_class}' marked:'#{label}'"
end
```

The line count is almost the same, but this parametric version removes code duplication, and if I'll need to add another view type I'll only have to add an entry to the map and an option to the RegEx matcher.

### Food for thoughts

* How can this method be improved even more?
* Is there any reason why the refactoring shouldn't have been used?
