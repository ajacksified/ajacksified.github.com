---
layout: post
title: New Game Conf Liveblog
description: I'm at New Game Conf; here's what's going on
permalink: /2011/10/new-game-conf-liveblog/index.html
categories:
- html5
---

Keynote by Richard Hilleman
---------------------------
*Nov 1, 9:00 - 10:00*

Richard Helleman is VP CCD at Electronic Arts.

Started a little late to liveblog this, so here's a synopsis and high points:

* Called the current web a Playstation 2 grade platform
* html5 needs platform predictibility, killer game to be successful
* Estimates that 75% of EA platform games (e.g. Playstation 2 games) could be converted relatively easily to web 

![Opening Keynote](http://s1-05.twitpicproxy.com/photos/large/439077657.jpg "Opening Keynote")

Debugging and Optimizing WebGL Applications by Ben Vanik &amp; Ken Russel
-------------------------------------------------------------------------
*Nov 1, 10:15 - 11:00*

I herpderped the markdown on these lists. Will fix after the talk.

* WebGL is coming out of "Experiemntal"
* Pros and cons of Webgl
* Pros
* Directly exposes GPU
* Very high performance
* Not a plugin
* Cons
* Much lower level than DOM
* Harder to learn, debug, optimize

Talking about debugging tips:

Check for OpenGL errors; restart with a good known base; start adding code 
back in until it breaks.

To debug shaders, remove functionality, output constant color (like red) in
the regions of the shader where you're trying to identify what's going on.

Try a library like:
* [webgl-debug.js](http://www.khronos.org/webgl/wiki/Debugging)
* Firefox web console
* Open web console

Handling Context Loss:
* Power loss on device, GPU reset, webgl-debug can help

[WebGL Inspector](http://benvanik.github.com/WebGL-Inspector/) by Ben Vanik.
Showing off webgl inspector with aquarium demo. Logs and shows all calls, steps
through draw calls to see scene reconstructed in order. Highlights redundant calls
that don't actually change meaningful state. Isolates draw calls, and you can see
what was affected directly and get information about the call, such as sample
state, vertex attributes, texture urls, etc.
