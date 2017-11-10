---
layout: post
title: '"XMLHttpRequest" is Silly'
date: 2014-09-25 22:51:48.000000000 +01:00
---

Here's something that's been irritating me lately - Javascript's
`XMLHttpRequest` function. I don't normally spend a great deal of time doing
web development, but as part of a project at work I've been using the Google
Maps API to develop a web application to visualise some mapping data.

<!-- more -->

Developing applications using the Google Maps JavaScript API is great - the API
is easy to use, the documentation is well written, it's consistent and
everything's well named. However, the application I was writing required me to
use a number of AJAX calls to construct search requests - to be sent off to
another web API that returned a response that would be rendered live on the
webpage.

Something started to bug me; it struck me that the "XMLHttpRequest" function is
named pretty badly. Let's split the name into chunks and look at each part.

# `<rant>`

### "XML"

This first part of the function name is kind of... wrong. When one sends an
async request from the browser, the request can be any type of document. The
response could be XML, but the response could equally be JSON, an integer, a
binary structure, an image, and so on. The `XML` in the name is entirely
irrelevant to the logic of the function!

Another thing. AJAX stands for "Asynchronous Javascript and XML". Again, `XML`
is not relevant in the name! This has been realised to some extent; the term
"AJAJ" (Asynchronous Javascript and JSON) was coined in about 2006 to describe
asynchronous interactions with JSON, but this term hasn't really enjoyed
widespread usage - most people still refer to async requests as "AJAX".

My suggestion would be **AJAR** - **A**synchronous **JA**vascript **R**equests.
With "AJAR", there's no semantic relationship between the concept of
"asynchronous requests" and document type (XML, JSON, etc). Less confusing and
more sensible!

### "Http"

My gripe with this isn't so much that `HTTP` is in the function name - the
problem is with how it is capitalised. Why, oh why, if the acronym `XML` is
fully capitalised in the first part of the function name, do you then go on to
butcher the capitalisation of `HTTP` and treat it as if it were some half-baked
camel-cased abomination?!

The function should either be named with:

* a camel-cased `XML` (if `XML` is even necessary in the first place, that is - hint: *it isn't*)
* a fully capitalised "HTTP", or
* no capitalisation at all!

I don't actually know if `HTTP` is actually necessary in the name either.
Considering that `XMLHttpRequest` is a web technology, the `HTTP` in the name
seems fairly redundant.


### "Request"

Hooray! The only sensible part of the function name! No problems here.

### So, what would I do instead?

Personally, I'd consider some alternative names - maybe one of these:

* `XMLHTTPRequest` - A bit of a pain to type, but at least capitalisation is
  consistent. Also wrong.
* `XmlHttpRequest` - Doesn't look as nice, but easier to type - and still
  wrong.
* `httpRequest`/`HttpRequest` - Great! Clear and unambiguous, not misleading.
  Consistent capitalisation too!
* `request`/`Request` - Clear, unambiguous, and super concise. (Yay, less
  typing!)

My personal favourite alternative would be `request` or `Request`. *Why?*

* it isn't misleading, and there's no unnecessary waffle in the name
* it's easier to remember and quicker to type
* it's shorter, so there will be fewer bytes in JS files (faster load times!)
* there will be less murderous rage from me, every time I have to type it.

# `</rant>`

That was stressful, I'd better leave it there.
