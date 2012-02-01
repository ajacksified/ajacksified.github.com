---
layout: post
title: Getting Backbone
description: In which I discuss my ultimately pleasant expeirence Backbone.js
permalink: /2012/01/getting-backbone/index.html
categories:
- javascript
---

Backbone is a javascript MVC-like (we'll get to that in a minute) framework
with a goal of giving your javascript some structure. It provides a lightweight
collection of objects for binding data from your server to visual elements on
your web browser, providing a seperation of concerns and helping you write DRY
code.

Get Backbone at [the website](http://documentcloud.github.com/backbone/#) or
[the Github](https://github.com/documentcloud/backbone/).

In the spirit of Backbone's version 0.9.0 release, which is effectively a RC
for the 1.0 release, I'd like to talk about how I make sense of Backbone and
use it in my day-to-day development cycle. I come from a background using MVC
frameworks, with Rails, [PHP](https://github.com/Olivine-Labs/Mint), and .NET
with WPF and Silverlight applications; and thus I will start with my first
epiphany towards using Backbone effectively:

__Backbone is not MVC.__

There is no "C". There's a Router, which used to be called a Controller early
in Backbone's history, but the Router functions purely as a way to map views
to URLs. There is no "path" to post data to, although you can use parameters
in your route. It is, quite simply, a map between routes and functions.

__In Backbone, the popular paradigm is to have your models do the talking.__

Your models will sync themselves to the server (which is super easy if you have
a RESTful persistance layer that Backbone can automatically hook itself into.)
Your views will accept models, then make changes back to them (say, based on a
form) or do things like call the model's persistance methods like `save` or
`fetch`. Thanks to version 0.9.0, the models even have a default method to
parse JSON retrieved from the server, so you don't even have to massage the
data on the server- just let your model handle it. We're talking smart models,
not dumb models.

Your backbone application may look something like this:

1. User hits URL
2. Page instantiates router object
3. Router object finds what route you're at, news up a model, which syncs its data
  from the server, and sticks it into a view
4. The view hooks model up to the UI, the user performs actions on the form, which 
  performs actions on the model, which syncs to the server
5. User clicks link, which maps to the router, continues the cycle

But wait. You *might not even use a router.* For example, maybe you want the
awesomeness of Backbone, but you don't plan on building a single-page app, or maybe
you plan on building one later but can't do it all at once. That scenario might look
more like:

1. User hits URL
2. Page instantiates model with data from the server (so we don't have to make
  two trips), instantiates view with this model
3. View hooks model up to the UI, performs actions on the model, which syncs to
  the server
4. User clicks link, which goes to a different page

The traditional controller is more baked into the models, and the router is entirely
for convenience if you want to manage history to enable some back-button functionality
for the user or if you want to store a bookmarkable state such as if you're building a
single-page-app.

Backbone's starting to look a whole lot more flexible now, more as a framework for
splitting up DOM manipulation and Data manipulation and less as a complete web
application composing kit. It *can* do that. But it doesn't have to if you're
not building it that particular way, or if you're working on it but can't get
there *today*. It's mostly a matter of making your a tags map to hash or history
changes instead of real URLs, and wazam, you have a singple page app with a fallback to
plain old HTML, a fancy, trendy application, and a RESTful service- everything
you need for an accessible, scalable front-end.
