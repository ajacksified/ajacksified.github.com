---
layout: post
title: New Game Conf Liveblog, Day 2
description: I'm at New Game Conf; here's what's going on, day 2
permalink: /2011/11/new-game-conf-liveblog-2/index.html
categories:
- html5
---
[Day 1](http://thejacklawson.com/2011/11/new-game-conf-liveblog/index.html)

This is a live blog for the second official day of 
[New Game Conference](http://www.newgameconf.com). I'll keep quick notes during
the presentations, and clean up grammar / confusing structures during breaks.
The Q &amp; A sections are paraphrased. Any quotes will be signified as such. 
Please leave corrections and requests for clarification in the comments here.

Also check out #ngc11 and #bbg on irc.freenode.net

Also also check out [New Game Conf Quotes](http://newgameconf.qotr.net/Quotes/List/)

I've used images that people post to Twitter; *none of them are mine*. I've used
small versions of the images and linked to the original posting.

## Notes on Chromebooks
Non-US residents will pick up physical devices. US residents will receive
vouchers to order Chromebooks, which will be shipped to your house.

##Keynote by Paul Bakaus
*Nov 1, 9:00 - 9:45*

CEO of Zynga Germany, creator of jQuery UI, creator of Aves Engine; heads
html5 initiatives in Zynga.

Zynga is the world's leading social develop; 232M monthly active users. Played
in over 166 countries in 17 languages.

Paul started because there were no html5 game engines; nothing suited his
needs, so he went out to create an engine. 

Problem: game developers are way faster than browser developers.

* html5 audio: no common codecs across browsers, audio playback is restricted
  differently across browsers (some don't have multichannel, some restrict
  what can start sounds to user actions)
* Device apis still missing: camera, file system, etc. Google is working
  towards access with latest version of Android. 20 device APIs available for
  mobile safari, 1500 for native apps.
* Debugging is difficult on mobile; good tools exist on desktop, especially
  webkit's debugger

Canvas, right now, is really only being used for charts and graphs. It isn't
applied to much more content; many games in production don't use canvas.

WebGL demos look like old games; partially because hard computation must be
done on CPU, in javascript. Another problem is that WebGL is a totally new
language to be picked up; there's only really one big WebGL library,
[three.js](https://github.com/mrdoob/three.js/).

Games that are too realistic (3d) have trouble appealing to the wide audience
that 2d games do.

New good things:

* WebSockets in iOS
* Hardware-accelerated canvas
* `webkit-overflow-scrolling: touch`

Theory 1: "good devs" don't want to learn html5
Theory 2: Companes are scared to ditch IE < 9 from their supported platforms

It isn't good to give people what they want on older browsers: they have no
incentive to upgrade, and thus we are stuck with old, dead tech.

> Try to come up with great new stuff that only works in modern browsers

> Traditional game devs get people to buy new hardware, and web devs can't 
> even get users to install free software.

Reminder: html5 was created as a markup language to create documents.
Not games. The W3C and WHATWG historically had no members from gaming
companies.

> Web devs are seldom good game devs, and vice versa

html5 gaming is very painful but very fun.

What we need most is creativity; we have capabilities and need to stop being
lazy. We need AAA game devs as well to not just port games, but invent new 
full-blown titles with html5.

Zynga's mission: connecting th eworld through games

Driving:

* W3C Participation
* Dialog with browsers
* Open-sourcing things ([github](https://github.com/zynga))

### Open Source

#### [Viewporter](https://github.com/zynga/viewporter)
Triggers native resolution whenever it can, and watches device orientation.
Solves obvious-sounding but difficult problems with handling resolution / 
orientation. Handles dpi as well. Available for iOS and Android.

#### [Scroller](https://github.com/zynga/scroller)
Handles panning (not scrolling) for html and canvas. 100% event-driven; only
logic, does not apply any translations. Supports DOM, Canvas, SVG, and WebGL.
Scrolling, zooming, snapping, and more. Extremely performant.

#### [Jukebox](https://github.com/zynga/jukebox)
Jukebox handles audio by dealing with sound sprites simple. Works around iOS
and multi-channel constraints; can pause / resume background audio if there
is a cosntraint around number of channels. Used in Zynga's production code


You have to be agile about development; techniques that helped performance
years ago no longer do. For example, direct DOM manip is now faster than
innerHTML, and inline styles don't cuser performance penalties.

Goal: hot swapping webgl / canvas / html

> Let's work together to make 2012 the year of html5 gaming.

## Diving Deep into Canvas by Grant Skinner
*November 2, 10:00 - 10:45*

Dev shop was originally considered a flash development shop; was approached
by Microsoft to see if they could build a fully html5 game for the IE9 launch.
Built Priates Love Daisies, and had to build a lot of their own tools using 
canvas.

Talk will be about Canvs, and specifically 2D Context, and challenges and
solutions; also, open-sourced lib [EaselJS](http://easeljs.com/).

Canvas: html5 element specifically for dynamic drawing with multiple contexts.

3D context: WebGL
2D Context: vector, bitmap, and pixel graphics

Canvas is broadly available (~80%), including mobile; consistently implemented,
even between browsers. Easy to learn; increasingly performant with hardware
acceleration.

![Grant Skinner](http://desmond.yfrog.com/Himg863/scaled.php?tn=0&server=863&filename=2v8ts.jpg&xsize=480&ysize=480 "Grant Skinner")

[source: @ajacksified](http://j.mp/uHmQ6i)

Canvas Libraries:

* Processing.js
* Three.js
* Crafty.js
* EaselJS

EaselJS:

* DisplayObject
    * Shape
    * Graphic
    * Bitmap
    * Bitmapsequence
    * Text
    * Container
    * Stage
* DOMElement
* Ticker
* ..more

EaselJS isn't a game engine as much as a graphics library. Built to have good
docs, as a generic visual-work-library, modular architecture for extended
functionality, performant with minimal overhead, and open-sourced on Github
with an MIT license

Canvas does not have anything for motion tracking. Some libs ignore mouse
movement, use tiles, or overlay DOM elements. EaselJS tracks specific pixels 
for accurate movement. This is done by transforming everything to a flat system
and manipulating pixel matrices.

Example:

    daisy.onPress = function(evt) {
      evt.target.alpha = 0.5;
      evt.onMouseMove = function(evt){ ... }
    }


Text in canvas is difficult; but, it can be overlaid with DOM elements which
are good at handling text. The UI can be built in the DOM, the game engine
in Canvas.

CSS transforms allow the same math to transform the DOM that transforms the
Canvas.

Integrating web technologies, like DOM and web APIs, allow you to add more than
what is traditionally possible with games.

Canvas performance: 

* Redraw only when / what is needed. 
* Choose a reasonable framerate. Consistency is more important than speed.
* Minimize vector and text draws
* Draw to offscreen canvases to cache, then pull in the static canvases into
  the main game canvas
* Reduce total pixel count
* Shadows are evil

![Canvas Buffer Demo](http://desmond.yfrog.com/Himg876/scaled.php?tn=0&server=876&filename=5w7v.jpg&xsize=640&ysize=640 "Canvas Buffer Demo")

[source: @ajacksified](http://j.mp/tFT3IS )

Desktop: Safari and IE9 have the best canvas performance; Chrome 14+ has 
hardware rendering, FireFox catching up

Mobile: iOS5 / iPad 2 is, by a huge margin, the best. iOS 4, Galaxy Tab 10.1,
Playbook is far slower.

Oft-requested features for EaselJS:

* Draw boundaries
* Fast shaders for canvas(see [css3 shaders](http://www.adobe.com/devnet/html5/articles/css-shaders.html))
* quads / perspective / 2.5D (some kind of perspective distortion)
* Drawing DOM elements
* State introspection

Tools for canvas: 

* Sprite sheets: Zoe, Flash Pro "Reuben", SWFSheet
* Profiling: browser dev tools, system gauges
* EaselJS Export panel (export from Flash to EaselJS / Canvas)

Many display options:

* SVG/DOM: ill-suited for interactive, performance-depenent games
* WebGL: Fast, but new, and steep learning curve
* Canvas: Ubiqutious, consistent, but lacks tools


Future vision: non-display-dependent. Single API for WebGL, Canvas, DOM.

