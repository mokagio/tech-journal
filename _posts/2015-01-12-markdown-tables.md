---
layout: post
title: Rendering tables in Markdown
date: 2015-01-12 19:35:42
---

I **love** markdown! It's such a simple yet powerful way to write formatted text! 

When setting up the repository that will keep truck the [keyboard shortcuts](https://github.com/mokagio/Shortcuts) I'm gonna learn every day in my quest to not use the mouse anymore, I wanted to have a table in the markdown document that GitHub will render automatically.

Turns out you can't! If you take a look at the [documentation](http://daringfireball.net/projects/markdown/) there is no mention to tables. It actually uses the `<table>` HTML tag as an example on how to do inline HTML.

> But I saw READMEs with tables!

### Github Flavoured Markdown to the rescue

The second resource to check in matter of Markdown is the [GitHub Flavoured Markdown](https://help.github.com/articles/github-flavored-markdown/). These tiny additions the guys at GitHub have introduced are great. One could argue that they take away the simplicity of Markdown, but they definitely come handy sometimes.

You can see all the options for a Markdown table [here](https://help.github.com/articles/github-flavored-markdown/#tables).

#### Minimum viable markdown table

```
Header | Other header
--- | ---
some | text
_some_ | **bold text**
_some_ | _italic text_
```

### Food for thoughts

How to implement a Markdown parser.