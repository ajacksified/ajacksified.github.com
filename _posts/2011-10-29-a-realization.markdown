---
layout: post
title: A Realization
description: Finished converting from Wordpress to Jekyll
permalink: /2011/10/a-realization/index.html
categories:
- Meta
---

So, I was looking over the work that "I" did- why I put "I" in quotes will be 
revealed- putting together this blog and I was completely blown away by the
complexity that occurs just to display a simple html and css file.

Here's how it's all structured:

* Github hosts code that is
* Parsed by the Jekyll templating language which
* Compiles and aggregates posts written in Markdown that include
* The html5 boilerplate, which itself is worthy of several blogposts, because it contains all kinds of other javascript and css frameworks. But I also included
* Twitter's Bootstrap css template which is
* Passed back down through http to your browser

The sheer amount of engineering effort that has preceeded me to create this
structure is staggering. Each step of the way includes countless man-hours of
effort put into each system, and its subsystems, by people all over the world.
And not only that, but the whole technology stack (save bits of the hosting)
is open source, even down to the http protocol and, chances are, the browser
you're using. It's all pretty incredible.

To hop onto a tangent, I also find it pretty fascinating that even with this
vertigo-inducing complexity, its interface with me, the human, is quite simple.
Writing this post as as easy as banging out some text in my favorite text 
editor, vim, in a nicely human-readable markup, Markdown, and I run two commands
to publish the whole thing on to the internet, onto this massive tower of
complexity.

Need to stop before I start thinking about the complexity down to the hardware 
layer, whereupon my mind melts.
