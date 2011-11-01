---
layout: post
title: Mediator.js v0.2
permalink: /2011/09/mediator-js-v0-2/index.html
post_id: 383
categories: 
- design patterns
- Development
- Javascript
- Mediator.js
- Source code
---
Source: [Mediator.js](https://github.com/ajacksified/Mediator.js)

Original post: [Mediators for Modularized Asynchronous Programming in 
Javascript](http://thejacklawson.com/2011/06/mediators-for-modularized-asynchron
ous-programming-in-javascript/index.html)

As I was reading Addy Osmani's excellent post 
[Patterns For Large-Scale JavaScript Application 
Architecture](http://addyosmani.com/largescalejavascript/),
I came to the realization that I had made some past changes to my own 
Mediator.js library, but had failed to both upload the changes to Github _and_ 
use some of the tricks I've picked up since originally writing the library. So, 
version 0.2 is out, with the following changes:


* API changes
* "Subscribe" in place of "Add"
* "Publish" in place of "Call"
* Subscribe now takes a context object, to allow you to pass in what @this@ 
should be when the callback is called
* Mediator no longer forces you to use it as a singleton
* No longer looks for "type" as part of a data object; rather, uses channels to 
sub/pub
* Code a whole lot cleaner; objects to pass around subscriber info rather than 
numerically-indexed arrays
* Passes arguments you pass into publish onto subscriber callbacks, rather than 
a data object
* Example:

    var m = new Mediator();
    m.Subscribe("test", function(arg1, arg2){ console.log([arg1, arg2]); }
    m.Publish("test", "arg1", "arg2");
    // output > ["arg1", "arg2"]

* Meta changes
* Tests are now "real" tests, using Jasmine
* Updated Readme file to reflect new information
* Added minified js (and under 500 bytes, too)

I think the main difference between this and most available and/or 
roll-your-own Mediators is the predicate matching, although that may also be 
what makes this more / different than a traditional Mediator. Mediator+? A 
meatier mediator? Meatiator.

Anyway,[here's an example on 
jsfiddle](http://jsfiddle.net/ajacksified/dCwaK/3/). Try chatting, and using 
the same name for the from/to fields.  Shows predicates, subscriptions, and 
removing subscriptions.
