---
layout: post
title: New Game Conf Liveblog
description: I'm at New Game Conf; here's what's going on
permalink: /2011/10/new-game-conf-liveblog/index.html
categories:
- html5
---

This is a live blog for the first official day of 
[New Game Conference](http://www.newgameconf.com). I'll keep quick notes during
the presentations, and clean up grammar / confusing structures during breaks.

Also check out #ngc11 and #bbg on irc.freenode.net

##Keynote by Richard Hilleman
*Nov 1, 9:00 - 10:00*

Richard Helleman is VP CCD at Electronic Arts.

Started a little late to liveblog this, so here's a synopsis and high points:

* Called the current web a Playstation 2 grade platform
* html5 needs platform predictibility, killer game to be successful
* Estimates that 75% of EA platform games (e.g. Playstation 2 games) could be converted relatively easily to web 

![Opening Keynote](http://s1-05.twitpicproxy.com/photos/large/439077657.jpg "Opening Keynote")

[photo source: @newgameconf](http://j.mp/s9pkUG)

##Debugging and Optimizing WebGL Applications by Ben Vanik &amp; Ken Russel
*Nov 1, 10:15 - 11:00*

> WebGL support in browsers opens the door to immersive 3D content, especially games, which is delivered with no end user installation, runs on multiple platforms and requires no porting effort.
Debugging and tuning WebGL applications for highest performance can be challenging, in particular due to the low-level nature of the WebGL and OpenGL APIs. This session will introduce the WebGL Inspector and explore several complex real-world applications to show how they achieved their results and how the tool can be used to learn more about an application through a way most never see.
Techniques that will be covered include batching of geometry, using texture atlases, pipelining data, reducing the amount of data transferred to the GPU, offloading computation to the GPU, and using web workers to parallelize applications. Workarounds and gotchas will be described for the differences between WebGL and other common implementations (such as OpenGL ES on iOS) and limitations imposed for security reasons.

[WebGL Examples](http://webglsamples.googlecode.com/hg/newgame/2011/)

I herpderped the markdown on these lists. Will fix after the talk.

* WebGL is coming out of "Experimental"
* Pros and cons of Webgl
* Pros
* Directly exposes GPU
* Very high performance
* Not a plugin
* Cons
* Much lower level than DOM
* Harder to learn, debug, optimize

###Debugging Tips

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

###[WebGL Inspector](http://benvanik.github.com/WebGL-Inspector/) by Ben Vanik

Showing off webgl inspector with aquarium demo. Logs and shows all calls, steps
through draw calls to see scene reconstructed in order. Highlights redundant calls
that don't actually change meaningful state. Isolates draw calls, and you can see
what was affected directly and get information about the call, such as sample
state, vertex attributes, texture urls, etc.

###GPU Optimization

* Retest often; automated tests where possible
* Reduce the number of draw calls per frame; could block GPU. Watch for
  highlights in WebGL Inspector
* Use RequestAnimationFrame for a more robust framerate; fallback with setTimeout if you have to
* Disable unused WebGL features, such as blending, alpha texturing, etc
* Link infrequently; shader verification / translation can take a long time, esp. on Windows
* Change Framebuffers, not Renderbuffers, which require a lot of validation (this
  is counter to iOS guidelines)
* [Graphics Pipeline Performance](http://http.developer.nvidia.com/GPUGems/gpugems_ch28.html)

###Optimizing Drawing (Scenes)

* Canvas is drawn, sorting by z-index
* Sorting by z-index is terrible for WebGL, which should be sorted by state, then depth, by using
  the depth buffer

Depth buffers can sort fragments per pixel; is relatively cheap for the PGU.
Depth testing can increase performance dramatically, even in 2d apps.

* Draw objects ordered by most expensive first; blending/clipping first
* Sort scene ahead of time and maintain as sorted list of possible
* Generate content that can be easily batched by merging buffers / textures
* Draw opaque objects front-to-back, then layer on translucent objects back-to-front
* Make sure to use index buffers, which enable additional GPU performance features 
    like caching
* Dynamically changing index buffers requires re-validation in WebGL. This is bad.
* Always ask yourself: can this be a constant? Make it so.
* Compute early, one matrix multiply on CPU is better than thousands on GPU
* Estimate numbers; lower precision, prefer "close enough" to "perfect"
* Lower level of detail (for example, for objects in the distance
* Use mipmapping
* Schedule math between sampling so that the GPU is not idle
* Scale canvas to smaller and css to set width/height larger if you have a set
  size
* GPU hardware is massively parallel; use if possible
* Pushing frames from GPU can cause stalls; try to spread updates across multiple frames.
  Number of uploads don't matter, size of frames does.
* Double / triple buffer if possible; makes frame upload consistent
* Decouple GPU and CPU; re-send GPU older data if CPU hasn't caught up yet

###Conclusion
Use tricks wisely; not all are useful everywhere. Start simple, test and
debug continuously.

>  10:54 ajacksified_ I heard you like spinning cubes
>  
>  10:55 segdeha So I put a cube in your cube
>  
>  10:55 ajacksified_ so you can spin your cube while you spin your cube

[Relevant](http://webglsamples.googlecode.com/hg/newgame/2011/05-pipelining.html)

### Q&amp;A

When will be a good time to start using WebGL for clients?

*You can begin to be it on it now. Consider progressive ehancement with canvas fallbacks.*

WebGL on Mobile, and battery life?

*WebGL is fairly similar to native code. It uses the GPU, and it will drain the
battery more than straight CPU, but that's the tradeoff for more power.
requestAnimationFrame helps.*

Does WebGL give you a way to use a secondary display?

*That is up to browser implementation. WebGL renders into the canvas directly.*

## Lessons from the (Mobile) Trenches by Justin Quimby
*Nov 1, 11:15 - 12:00*

> Moblyng has been developing HTML5 games for the past 3 years. 
This talk is an overview of the lessons learned from that experience 
covering multiple code frameworks, business partners, and revenue models.

Justin Quimbly: COO of Moblyng

### Moblyng

* 13 html5 games
* Startup in Redwood City
* Started with porker games, word games, war-style rpgs
* Playdom: Social City, Sorority Life, Mobsters
* WeeWorld: WeeMee Life
* Facebook: Social Poker Life

### Strategy of Mobile Development

Point of html5: accessibility to all devices

Social games are a great fit for light, engaging games. Big market opportunity
and distribution opportunities in the mobile space, especially for people
already on Facebook. These can be augmented with native apps, wrappers around
the html5 games.

Products run on all devices, because of html5. Doesn't matter who "wins" the
mobile OS war.


> "How many of you building HTML5 games are targeting mobile?" Everyone raises their hand. #ngc11 Mobile HTML5 games = huge opportunity

[source: @joemccann](http://j.mp/sfvH8q)

#### Technology Framework

Huge numbers of mobile, internet-connected devices being shipped.

> There is a huge market for playing games on your refrigerator.

* No canvas - too slow / unreliable. Uses javascript / css
* Moblyng shell handles native hooks
* Benefits of wrapper: native app in app stores for distribution and consumer
  access
* Right now, if you need super blazing-fast peformance, you're going to have to
  write native code. Intense apps and light apps are different products for
  different platforms.
* Invest in a ton of varying mobile devices, so you dont have to wait and can
  experience the hardware personally
* Devices are expensive: paying premium prices for brand-new devices
