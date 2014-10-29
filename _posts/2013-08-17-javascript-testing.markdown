---
layout: post
title: Javascript Testing
description: In which I share a presentation about javascript testing.
permalink: /2012/08/javascript-testing/index.html
categories:
- javascript
- node
- testing
- mocha
---

I shared a presentation at our engineering meeting this past Friday (at
Airbnb) whereupon I discussed how to write clean, fast javascript tests.
One can almost always write good tests
and achieve high test coverage by simply using unit tests and stubbing out
DOM APIs. That isn't to say that browser-based tests aren't
useful; it's to say that you can get a great testing ecosystem running
without complicated setup. Mocha will also, neatly, allow you to go ahead
and run all of these specs within browsers anyway, so if you do find it
advantageous, you have that option open to you.

I also threw in a couple slides about how you can use 
[Browserify](http://browserify.org/)
(or really, any kind of CommonJS-style module system, like require.js) to
write all of your code as modules to make both testing and browser
asset compilation awesome.

Hopefully the slides are clear without too much explanation or notes, but
if they're obtuse, feel free to contact me and I'll add appropriate
notes. The slides are built in Reveal.js, and are available (MIT
Licensed) at
[github.com/ajacksified/javascript-testing](https://github.com/ajacksified/javascript-testing).
