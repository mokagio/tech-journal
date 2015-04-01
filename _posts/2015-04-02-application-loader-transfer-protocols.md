---
layout: post
title: Application Uploader Transfer Protocols
date: 2015-04-02 08:22:42
---

I recently started using Application Uploader and at first I'd been struck by **how slow it was** to upload the packages, and by the high rate of failures due to timeout that I got.

At first I thought it was because of the poor connection I have, but it turned out to be [a problem with Application Uploader itself]()!

Apparently the DAV transfer protocol creates issues, so it needs to be disabled in the Settings > Advanced panel.

![Application Uploader Advanced Settings]()

But what are those protocols anyway?

My intention for this post was to have a paragraph per protocol and describe them, but the information I could find about them was very little 😳. As far as I can see Signiant and [Aspera](http://asperasoft.com/technology/transport/) are two proprietary secure transport protocols, the latter being property of IBM, while DAV, or [WebDAV](http://en.wikipedia.org/wiki/WebDAV), is open.

### Food for thought

* Where can we find more information about these transfer protocols?
* Why DAV creates issues
* Is DAV actually the same thing as WebDAV?
* Are there other similar protocols?
