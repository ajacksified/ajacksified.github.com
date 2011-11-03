---
layout: post
title: New Game Conf Liveblog
description: I'm at New Game Conf; here's what's going on
permalink: /2011/11/new-game-conf-liveblog/index.html
categories:
- html5
---

*Note: Combined the first and second day for a mega-wall-o-text.*

## Day 1 - November 1, 2010

This is a live blog for the  
[New Game Conference](http://www.newgameconf.com). I'll keep quick notes during
the presentations, and clean up grammar / confusing structures during breaks.
The Q &amp; A sections are paraphrased. Any quotes will be signified as such. 
Please leave
corrections and requests for clarification in the comments here.

Also check out #ngc11 and #bbg on irc.freenode.net

Also also check out [New Game Conf Quotes](http://newgameconf.qotr.net/Quotes/List/)

I've used images that people post to Twitter; *none of them are mine*. I've used
small versions of the images and linked to the original posting.

Some of the slides and are also linked at [confswag](http://confswag.com/2011/newgame/)

##Keynote by Richard Hilleman
*Nov 1, 9:00 - 10:00*

Richard Helleman is VP CCD at Electronic Arts.

Started a little late to liveblog this, so here's a synopsis and high points:

* Called the current web a Playstation 2 grade platform
* html5 needs platform predictibility, killer game to be successful
* Estimates that 75% of EA platform games (e.g. Playstation 2 games) could be 
converted relatively easily to web 

![Opening Keynote](http://s1-05.twitpicproxy.com/photos/large/439077657.jpg "Opening Keynote")

[photo source: @newgameconf](http://j.mp/s9pkUG)

##Debugging and Optimizing WebGL Applications by Ben Vanik &amp; Ken Russel
*Nov 1, 10:15 - 11:00*

[Slides and Samples](http://webglsamples.googlecode.com/hg/newgame/2011/)

> WebGL support in browsers opens the door to immersive 3D content, especially 
> games, which is delivered with no end user installation, runs on multiple 
> platforms and requires no porting effort.  Debugging and tuning WebGL 
> applications for highest performance can be challenging, in particular due to 
> the low-level nature of the WebGL and OpenGL APIs. This session will introduce 
> the WebGL Inspector and explore several complex real-world applications to show 
> how they achieved their results and how the tool can be used to learn more 
> about an application through a way most never see.
> 
> Techniques that will be covered include batching of geometry, using texture 
> atlases, pipelining data, reducing the amount of data transferred to the GPU, 
> offloading computation to the GPU, and using web workers to parallelize 
> applications. Workarounds and gotchas will be described for the differences 
> between WebGL and other common implementations (such as OpenGL ES on iOS) and 
> limitations imposed for security reasons.

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
through draw calls to see scene reconstructed in order. Highlights redundant 
calls that don't actually change meaningful state. Isolates draw calls, and you 
can see what was affected directly and get information about the call, such as 
sample state, vertex attributes, texture urls, etc.

###GPU Optimization

* Retest often; automated tests where possible
* Reduce the number of draw calls per frame; could block GPU. Watch for
  highlights in WebGL Inspector
* Use RequestAnimationFrame for a more robust framerate; fallback with 
  setTimeout if you have to
* Disable unused WebGL features, such as blending, alpha texturing, etc
* Link infrequently; shader verification / translation can take a long time, 
  esp. on Windows
* Change Framebuffers, not Renderbuffers, which require a lot of validation 
  (this is counter to iOS guidelines)
* [Graphics Pipeline Performance](http://http.developer.nvidia.com/GPUGems/gpugems_ch28.html)

###Optimizing Drawing (Scenes)

* Canvas is drawn, sorting by z-index
* Sorting by z-index is terrible for WebGL, which should be sorted by state, 
  then depth, by using the depth buffer

Depth buffers can sort fragments per pixel; is relatively cheap for the GPU.
Depth testing can increase performance dramatically, even in 2d apps.

* Draw objects ordered by most expensive first; blending/clipping first
* Sort scene ahead of time and maintain as sorted list in app memory if possible
* Generate content that can be easily batched by merging buffers / textures
* Draw opaque objects front-to-back, then layer on translucent objects 
  back-to-front
* Make sure to use index buffers, which enable additional GPU performance 
  features like caching
* Dynamically changing index buffers requires re-validation in WebGL. This is 
  bad.
* Always ask yourself: can this be a constant? Make it so.
* Compute early, one matrix multiply on CPU is better than thousands on GPU
* Estimate numbers; lower precision, prefer "close enough" to "perfect"
* Lower level of detail (for example, for objects in the distance
* Use mipmapping
* Schedule math between sampling so that the GPU is not idle
* Scale canvas to smaller and css to set width/height larger if you have a set
  size
* GPU hardware is massively parallel; use if possible
* Pushing frames from GPU can cause stalls; try to spread updates across 
  multiple frames.
  Number of uploads don't matter, size of frames does.
* Double / triple buffer if possible; makes frame upload consistent
* Decouple GPU and CPU; re-send GPU older data if CPU hasn't caught up yet. If
  you perform the same translation sequence twice, re-send the old GPU data and
  fake it.

###Conclusion
Use there tricks wisely; not all are useful everywhere. Start simple, test and
debug continuously.

>  10:54 ajacksified_ I heard you like spinning cubes
>  
>  10:55 segdeha So I put a cube in your cube
>  
>  10:55 ajacksified_ so you can spin your cube while you spin your cube

[Relevant](http://webglsamples.googlecode.com/hg/newgame/2011/05-pipelining.html)

### Q &amp; A

When will be a good time to start using WebGL for clients?

*You can begin to be it on it now. Consider progressive ehancement with canvas 
fallbacks.*

How does WebGL affect mobile battery life?

*WebGL is fairly similar to native code. It uses the GPU, and it will drain the
battery more than jut using the CPU, but that's the tradeoff for more power.
requestAnimationFrame helps because it throttles requests.*

Does WebGL give you a way to use a secondary display?

*That is up to browser implementation. WebGL renders into the canvas directly.*

## Lessons from the (Mobile) Trenches by Justin Quimby
*Nov 1, 11:15 - 12:00*

> Moblyng has been developing HTML5 games for the past 3 years. 
> This talk is an overview of the lessons learned from that experience 
> covering multiple code frameworks, business partners, and revenue models.

Justin Quimbly: COO of Moblyng

### Moblyng

* Developed 13 production html5 games
* Startup in Redwood City
* Started with poker games, word games, war-style rpgs
* Playdom: Social City, Sorority Life, Mobsters
* WeeWorld: WeeMee Life
* Facebook: Social Poker Life

### Strategy of Mobile Development

Point of html5: accessibility to all devices

Social games are a great fit for light, engaging games. There is a big market opportunity
and distribution opportunities in the mobile space, especially for people
already on Facebook. These can be augmented with native apps, which can be no more than
wrappers around the html5 games.

Products run on all devices, because of html5. Doesn't matter who "wins" the
mobile OS war.

> "How many of you building HTML5 games are targeting mobile?" Everyone raises 
> their hand. #ngc11 Mobile HTML5 games = huge opportunity

[source: @joemccann](http://j.mp/sfvH8q)

#### Technology Framework

Huge numbers of mobile, internet-connected devices being shipped.

> There is a huge market for playing games on your refrigerator.

* No html5 `canvas` - too slow / unreliable. Moblyng uses javascript / css
* Moblyng shell handles native hooks
* Benefits of wrapper: native app in app stores for distribution and consumer
  access
* Right now, if you need super blazing-fast peformance, you're going to have to
  write native code. Intense apps and light apps are different products for
  different platforms.
* Invest in a ton of varying mobile devices, so you dont have to wait and can
  experience the hardware personally
* Devices are expensive: paying premium prices for brand-new devices
* Make devices readily available for rapid testing
* Android fragmentation is an issue; have to target 2.1+ for mass penetration
* Some phones may not implement spec properly; one older Motorola was totally
  broken. Was fixed, but you have to be ready to respond to hardware 
  manufacturers.
* Consumers won't ever do what you expect them to do. Being behind on updates 
  is a problem.
* Can't count on the same screen resolution; have desktop / tablet / phone
  versions

> If you need super blazing-fast peformance, you're going to have to
> write native code. Intense apps and light apps are different products for
> different platforms.


#### Discovery and Distrubtion

* If nobody can find it, your product won't be successful
* Android market getting a lot better

### Things to Avoid

* App startup sounds - players are probably playign somewhere where they don't
  want to be disturbed. Meetings, bathrooms, bed, etc.
* Device orientation matters - portrait / landscape. Portrait's the way to go
  to play games during the work day. Discreetness!

### Things to Do

* Hands-on Demo. Show users that the app works and is fun.
* Speed matters, especially on mobile devices.
* Launches should take &lt; 10 seconds.
* Touch response speed. Is the interface responsive?
* Scroll speed; no stuttering, no delay.

### Other Notes

* Mobile metrics suck; cache clears on iOS power cycle, Android 
  inconsistencies, hard to verify
* Better off rolling your own metric solution
* Work on great relationships with hardware manufacturers to test your app
  before new hardware/software releases to reduce surprise problems
* General dev practices; source control, make dev / build / deployment systems 
  are the same
* Automate build / deployment (css / js minification, for example)
* Set up IE8 / 9 stations
* Cross-training; designers / developers
* Swap devices between people (android, winmo, ios, etc.)
* Work on relationships with other devs who you interface with
* NO ABSOLUTE POSITIONING. You're fired.
* jQuery, WebGL, Canvas slow or non-functional on a lot of devices
* Android shipments have passed iOS shipments. For maximum availability,
  support Android, not just iOS.
* Minimize technical risk. Use stable bits of html5. Consider that technologies 
  may be alienating consumers.

## Q &amp; A

Can you put perspective on company size of Moblyng, and how that impacts your
decisions? Where would you prioritize with a smaller headcount?

*Moblyng has around 35 employees. The key to all of this is the game itself.
At the end of the day, if your game sucks, noone's gonna care. Your team is
initially going to probably be split 2-1; two for the game, one for system
integration. Closer to the ship date, that ratio should split as you
integrate with social systems. After shipment, it will reverse again as you
respond to metrics and game mechancis updates.*

What do you know about iOS vs Android users paying more?

*It depends on the game type more than the platform.*

Do you still target devices outside of Android and iOS?

*We target devices outside of Android and iOS; the lion's share are in iOS
and Android. Because we're venture-backed, we want to go big, and build games
that work on various devices. The html5-running refrigerator validates our 
strategy; will our game work there?*

Focusing on user acquisition, can you speak to the difference in different
advertising schemes?

*Part of the reason we target high distrubition is because it's hard to
estimate; it depends on the product.*

Can you talk a bit about how the platforms you choose to support influences your
development process? Do you focus on one and expand to other platforms?

*The evolution was focusing on phone; and now, it's foucsing on three platforms,
phone, tablet, and desktop. Part of our initial product plan is to mock up UIs
for each platform; user flows are somewhat different between platforms. We look
at everything from day one.*

Can you speak to experiences in doing work in-house vs. using contractors?

*A good rule of thumb is: anything you can throw away 90% of, and still be ok,
is a good thing to outsource. Artwork is a good idea. If you say "we need 100
swords", you may get 10 that are good. Development should happen in-house.
Face-to-face communication is important.*

What would you say to people who want to focus on a specific technology,
instead of taking on more platforms?

*There is a huge market; if you want to focus on a specific technology,
and do it well, then that's a good strategy too. For us, in our history
we've always focused on distribution. Either is a viable business
strategy.*

Can you talk about how beta testing has influenced development?

*Automated client-side testing is difficult, because resource use for the game
leaves little room. We do automated testing on desktops. Automated testing is
less important than having a good game. Make sure you can make money on a game,
then spend time automating tests.*


## Lunch Break

![The weather in San Francisco is amazing!](http://s1-04.twitpicproxy.com/photos/large/439186499.jpg)

[Source: @robhawkes](http://j.mp/tUJBw9 ) 

##Fieldrunners HTML5: Bringing a Hit iOS Game to the Web by Darius Kazemi
*Nov 1, 1:15 - 2:00*

[FieldRunners](http://fieldrunnershtml5.appspot.com/)

> Just this past summer, Bocoup and Gradient Studios worked with Subatomic 
> Studios to port their smash-hit iOS tower defense game Fieldrunners to HTML5. 
> This post mortem will cover porting OpenGL ES to WebGL, using the Web Audio API 
> for game audio, integrating microtransactions and DLC, and a detailed look at 
> graphics performance.

Darius Kazemi is a game developer at Bocoup in Bosten; did FB games at Blue Fang, 
foudned / ran game analytics middleware Orbus Gameworks, data analyst at Turbine on
MMORPGs.

Fieldrunners was released oct 2008, one of the first hit iOS games. Much more
polished, higher fidelity than other games. Was ported to Nintendo DSi, PSP,
PS3, etc. Bocoup was contacted to convert it to html5 / Chrome Web Store, which
began July 2011.

* Took 12 weeks to port
* 3 weeks prepro and testing
* 1 programmer, 3 QA, 2 producers

### Technologies Used

* WebGL
* Web Audio API
* Html5 Audio
* Google In-App Purchaes
* JS
* Python
* Go

### Target Platforms

* Chrome / Chrome Web Store

### Plan of Attack

2-week milestones; built demos at end of each milestone.

* Milestone 1
    * Tech Design Doc
    * Workflow (check-in flow)
    * Production risk assessment
* Milestone 2
    * C++ to JS conversion
    * Particle system prototypes
         * high risk - no clue if would destroy framerates until it was built
* Milestone 3 - beta
    * Finish game
        * Optimizations
        * Gameplay
* Milestone 4 - beta
    * Monetization

First steps: get stuff to render; parse animation files, load images, render
to WebGL; build sprite demo.

Basic functionality came next; porting tower functionality, entities, particle
system.

Awesome particle effects made for rare units, like flame towers; was able to
mitigate performance issues by limiting number of special, GPU-heavy units.

Took demo, packaged it up, cleaned off any private data, and posted it on blog to watch
people's framerates in order to test on different configurations. Used
Google Analytics event API to send framerate data for tracking through Google
Analytics every 8th frame. Allowed comparision between browsers, operating
systems, and other user breakdowns for average user framerate. Ended up gathering 
framerate data *and* the demo generated buzz about the game.

The target framerate was 30fps.

The next step was porting maps, enemies, and menus. Menus accounted for a
majority of the work for this milestone. 

Next was making it feel "webby"; SD / HD modes, detecting WebGL, Web Audio,
window size, and general responsiveness; setting up hosting and caching.

> It's  weird to see what happens when the game dev and web dev worlds collide,
> and see what sparks happen.

### Things Learned

* Cached requests dramatically reduced CPU Usage.
* Wrote a Python script for a machine port; a big mess of regexes did a lot of the
  work, which was later cleaned up manually.
* Used Web Audio APi for SFX, which was released with Chrome 14. HTML5 Audio was
  used for background music. Targetting Chrome specifically made this easy.
* Pack data into arrays where it makes sense.
* Javascript scope was generally a headache; use a good JSHint to help
  mitigate issues.
* Fault Tolerance: sometimes http requests fail, resulting in a black screen.
  Attempt to load everything up to 3 times; test on a server that behaves in a
  faulty way; include heavily delayed responses as well (up to 500ms latency)
* In many situations, execution is dependent on content being loaded, such as
  menu screens. Wait for / chain onload events.
* Decision to launch only on Chrome makes sense in order to deliver a
  high-quality game in a timely manner. Pick two: browser reach, quality, or
  cost. 

### Q &amp; A

Is using WebGL Overkill?

*We used WebGL because it was an easy port; our existing game was built with
OpenGL.*

During the development, did you feel like WebGL was the best choice?

*Because we were porting the already aggressively-optimized OpenGL game
built for iOS, the performance was already excellent.*

If you didn't have the original code in WebGL, would you still use WebGL?

*It's all about the context of your project; if the original game wasn't in
OpenGL, we would have probably spent time testing canvas, and looking closely
at the markets and seeing what browsers could run WebGL. It would have been a
more difficult decision.*

What three things did you take away, that you wish you knew before you started
the project? Also, what can do we do to not have to compromise so much on the
reach-quality-cost triange?

*We need more compliance between browsers. Features like audio have to conform
to the lowest-common-denominoator between the platforms you're targetting.
It would be great if everyone had the same standards; that was a huge
stumbling block to getting it on more browsers. Having browsers that auto-
update would be useful as well, so you know where users are at. As far as
some takeaways- I wish I knew server management was as time-consuming
as it was for us.*

Was the caching issue with game assets, or something else?

*Yes, that was about using the appropriate server headers so that I only have
to download if I have to.*

Is audio your biggest issue?

*It is. We want awesome, synchronous audio in all browsers.*

## SONAR - WebGL, JavaScript, and Gaming by Sean Middleditch and Jason Meisel 

*November 1, 2:15 - 3:00*

[Slides](https://docs.google.com/presentation/d/110MxOqut_y7KOW1pNwIdcccisIA3ooJwVR-xm-ecuc4/view)

> SONAR is one of the first complete HTML5 games implemented with WebGL, 
> targeting the Chrome platform via the Chrome Web Store. The first part of this 
> talk covers the technical details of our development environment, including our 
> engine architecture and asset pipeline, as well as the problems we ran into 
> with the HTML5/WebGL platform and the workarounds we deployed. The second part 
> is a classical post-mortem.

### Engine Composition

Behavior composition - built so that there were few dependencies between behaviors, and behaviors
could be stacked

Pass-by-referene - be careful of unintended references, where you may
accidentally modify a value

Use Chrome Profiler to help find places for optimization, but test 
"optimizations" carefully

Avoid creating temporary variables to reduce garbage collection pressure

Keep in mind object typing in Javascript; when using the "+" operator, for instance. 
"1" + 1 = 11

### WebGL

WebGL doesn't use plugins, mostly cross browser (not IE), hardware 
accellerated. SONAR was 2D, wanted to move to 3D, so went with WebGL

#### Pros

* Easy to use
* [LearningWebGL.com](http://learningwebgl.com)
* [WebGL Inspector](http://benvanik.github.com/WebGL-Inspector/) is awesome

#### Cons

* No DirectX 10+ or OpenGL 3+ features
    * Geometry shaders, etc
* Debugging is difficult
    * Bad error reporting

### Javascript's Design

* Not originally designed for real-time applications
* Needs fast SIMD Math for graphics, physics, and game logic
* Real-time applications
    * Hard; only have 16ms per frame for calculations
    * Garbage collector is a killer
    * Dynamic VBO updates are inefficient
    * Offload as much as possible to the GPU

#### Garbage Collector

* Avoid GC at all costs
* No allocation means no collection
* Causes of allocation / GC:
    * "new" is easy to spot and avoid. Don't use it if possible
    * Object and Array literals are harder to avoid
    * Creating new closures creates a new object; bad in loops
    * DOM element manipulation
    * Hidden things in 3rd party libs
* Pre-allocate objects
* Use Chrome debugger to watch memory 
* "private globals" by using a consturctor that returns an object which is a
  subset of the parent

### Art pipeline

Make transition from blender -> game as easy as possible

* Blender
    * Pros
        * Free
        * Nice animation features
        * Extensive Python API
    * Cons
        * Revision change lead to lack of documentation
        * Few pro artists are trained in Blender

Tried using both JSON and binary formats for files; binary was hard to work with in
Javascript, and thus was not worth it. JSON was good enough and far easier to work
with. Don't bother with binary until you're sending to the GPU.

Meshes were loaded into vertex buffer objects

Used animation JSON files for describing animation frames

Javascript was not integration-friendly; difficult to work with binary files,
poor support for loading files from disk, difficult to get models into engine
for preview

Should have used an intermediary format (FPX) for 3D models like, so that artists
were not locked down to using Blender

### html5, CSS, Canvas for 2D games

Assets need to be built for menus, debugging, editors, etc. CSS3 can be used
for advanced animations for interfaces.

Rendering text in webgl is tricky; html overlays make it easy. Composite WebGL,
html, and canvas together in layers.

Note: be wary of alpha on your webgl canvas. Also, moving 2D elements on top
of WebGL can cause framerates to drop. There are simple fixes.

### Editor / Content Creation Pipeline

* Tile-based editor
* Line-based doors, glass
* Triggger zones
* Entitiy placement

Having a seperate editor from the game made testing slow; an editor should have 
been built into the game.

#### Technology

* Canvas for tiles
* jQuery for UI controls
* Seperate app from the game
* Custom scripting
    * Allowing javascript in game opens security (XSS) issues; better to hack
      together something

### Web APIs for games

APIs that are on their way that are helpful:

* Web Audio
    * Chorus effects, 3d positioning, no flash
    * [w3 audio spec](https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html)
* FileSystem
    * Store files locally, no cookies or lost data
    * [Filesystem spec](http://ww.html5rocks.com/en/tutorials/file/filesystem/)
* MouseLock
    * Mandatory for FPS and useful for action games
    * [Mouselock api](http://chromestory.com/2011/10/developer-news-mouse-lock-api/)
* Joystick
    * Gamepad support
    * [Chromium Joystick api](http://www.chromium.org/developers/joystick-api)

### Q &amp; A

What gave you the inspiration to make it a papercraft visually-styled game?

*It actually started as a graphics bug, where our charaters were flatfaced,
and our designers really liked the idea.*

Have you actually cut out any papercraft models?

*We considered it, but we haven't had the chance.*

## Cross Platform Game Programming with PlayN by Lilli Thompson

*November 1, 3:15 - 4:00*

[PlayN](http://code.google.com/p/playn/)

Began as console developer, now a software engineer with chrome and PlayN
advocate

PlayN allows you to compile out versions of your single-codebase game onto
multiple platforms: write in GWT / Java, export to html5, flash, and Android.
Used to be called "ForPlay". 

*--ed: You can guess why that got changed.*

Angry Birds was written with PlayN; announced with "Kick Ass Game Programming
With GWT" talk

Tries to solve the problem of picking a platform to port to; does the porting
for you. Solves problems with html5 non-compliance.

GWT is a framework that focuses on removing unused code, doing compile-time
checking of syntax, inlines, optimizaes, obfuscates output, and adjusts
the code for browser quirks.

Uses whatever's best available: WebGL, Canvas2D, CSS 3; also abstracts audio.

"Service Provider Interface" structure - core platform, with platform-specific
implementation modules. PlayN is not a game engine - it is a platform
abstraction. You add game enginy things on top, such as Box2D. Components
include the game loop, I/O system, and Asset Management.

Build game engine by extending PlayN-provided interface, which gives you
access to the game loop / i/o system / asset management structures.

PlayN provides a way to access the local device for its resolution:

    graphics().setSize(
      graphics().screenWidth()

      graphics().screenHeight()
    )

### Compositing Layers

* Layer
    * SurfaceLayer
    * ImageLayer
    * CanvasLayer
    * GroupLayer
        * 1-n child layers

#### ImageLayer

High performance, render images and forget. Good for static
images like UI, logos, overlays. Doesn't get added to scene graph.

#### CanvasLayer

Mirrors much of the html5 canvas api. Procedural circles, lines, points, etc.
Slow, but can be optimized; make as small as possible, translate in place.

#### GroupLayer

Groups layers.

### I/O Abstractions

* Pointer
    * General, non-specific input device; most widely available
* Mouse
* Touch
* Keyboard

Cross-playform input: support things like touch-based zoom where available

    if(platformType().equals(Platform.Type.ANDROID){
        touch().setListener( ... )
    }

### Asset Management

Resource management through the asset manager:

    Image getImage(String path)
    Sound getSound(String path)
    // etc

Sound abstraction: html5 version uses web audio api where avilable via
gwt-voices; falls back to flash.

### Data Storage

Android local store, html5 local storage, whatever's available. Simple API that
uses calls like `setItem(key, value)`, `removeItem(key)`, `getItem(key)`.
Includes JSON abstraction for storing / retrieving data.

### Compiling to platforms

To compile, extend the class you're porting to, and use the platform-specific
core objects. Centeral codebase lives seperately from platform-specific code,
which passes in objects like HtmlStorage or FlashAudio as interfaces to the
game loop.

`public class MyGameHtml extends HtmlGame`

### PlayN for the Future

Framework allows easy extension to new platforms. Takes any Java-based library
and can compile it down to all platforms.

More native input abstractions, better asset managmeent and audio featutes,
integration with other products- in-app payments, other software ecosystems

### Q &amp; A

What happens when you don't support a particular piece of functionality?

*It's open source, so if it isn't working for you, consider adding the
functionality; that's available to you. Another option is to compile
another project that imports the extra code."*

What's compelling about this is that Java's a nice language. How do you do the
compilation to, say, Flash?

*It's different on each platform; it translates to legible actionscript, then
compiles a swf. It compiles into obfuscated Javascript, so it doesn't work
that way always.*

What if your phone doesn't have a GPU?

*We don't have set requirements; our default path is to compile to Android
OpenGL. Android Canvas is available, but not as fast; I'm not sure if that
is an option during Android compilation.*


## From Apple Store to CWS by Miguel Angel Pastor

*Nov 1, 4:15 - 5:00*

[Slides](http://www.mandreel.com/downloads/From_Apple_Store_to_Chrome_Web_Store.pdf)

> This talk covers our experience porting more than 7 iOS games to the Chrome 
> Web Store. Topics discussed include: using Mandreel to convert C++ into 
> JavaScript, using Google App Engine to host applications, saving data in the 
> cloud, implementing IAP and Facebook Connect, differences between Firefox and 
> Chrome, dealing with huge JavaScript files, and application caching.

Co-founder of Onan Games; small company (5 people, all engineers, no designers)

Began porting iOS games to html5 using a framework called andreel, which 
converts C++ / Objective C to html5 and flash. Targets all browsers, and the
conversion process takes only a few days.

Published games (available in chrome web store) 

* [Monter Dash](https://chrome.google.com/webstore/detail/cknghehebaconkajgiobncfleofebcog)
* [Great Little War Game](https://chrome.google.com/webstore/detail/dogopgiahgigenndhchnnahmibmjghbn)
* [Gun Bros](https://chrome.google.com/webstore/detail/ciamkmigckbgfajcieiflmkedohjjohh)
* [Big Time Gangsta](https://chrome.google.com/webstore/detail/ajplbhgiljhgjomddcnchfoimakkbmkc)
* [Contract Killer](https://chrome.google.com/webstore/detail/meklndaflopgghbomkdpofehonfclipi)
* [Mad Skills Motocross](https://chrome.google.com/webstore/detail/lgoenphchcpbgebeednhcdmepodabeig)
* [Bug Village](https://chrome.google.com/webstore/detail/pabppflkalbniedjechdomdnofnogcfh)
* [A Space Shooter for free](https://chrome.google.com/webstore/detail/epbeobdmeddlnkokfiaijkfabecpmifa)
* [Kroll](https://chrome.google.com/webstore/detail/efjdaaaepgacfpadimoljoefkmnnkpkm)

Chrome web store options: packaged application, website

Chrome default max size: 5MB; installed apps have unlimitied storage

Can do synchronous loading with XHR

### Application Manifest Cache

The manifest cache file lists resources for the app to load. Progress event is
available to update user to download status for larger downloads.

To update new files, modify the mainfest cache file; update verion number.
This reloads all the files.

App engine limits max file size to 32MB, so large files must be split.

### FileSystem API

Only works in chrome; allows filesystem access within a sandbox. Request amount
of storage, which the user can deny / allow, and allows you to create files
and load resources asynchronously.

For performance:

* Pack files into one large file for faster downloads
* Compress with LZMA
* Use XHR async loading
* Store uncompressed data in FileSystem API
* Keep uncompressed data in memory (not feasible for large games)

iOS games store game progess in internal memory; html5 apps can use
localstorage. Another option is to use XHR to store progress remotely.

Can generate JS larger than 10mb; AppEngine can only handle 1mb. Load
compressed JS via XHR.

Mandreel supports OpenGL ES 1.1, PVR textures; does not have fixed pipeline 
support. PNGs are best for textures.

In-app payments can be done through google IAP API

iOS uses OpenAL for sound. Started trying to load sounds with the html5 audio
tag, but it doesn't work well with many simultaneous sounds. Web Audio API
gives needed support. Flash is a fallback.

App engine issues:

* Static files can't add custom headers
* Max application file size is 150MB (use blobs)
    * Check for "206" response code when pulling blobs
    * Audio tags won't work properly

### Q &amp; A

With the issues you've had, if you went back in time, woudl you do it again?

*Absolutely.*

## Extreme Mobile HTML5 Graphics Performance by Sam Abadir

*November 1, 5:05 - 5:25*

> This short-form talk will discuss how to use hardware accelerated 
> DirectCanvas to take games from 2 frames per second to 30. This will show you 
> how to use DirectCanvas while running on mobile and deploy the same code to 
> desktop browsers using the standard HTML5 Canvas.

Sponsored by [AppMobi](http://appmobi.com)

AppMobi is an alternate mobile browser that provides better canvas, sound, and
other gamey things. Supports ImpactJS and PhoneGap, and apps can be packaged
with AppMobi to provide things like phone vibration.

* html5 was built for desktop, but mobile is more important
* DirectCanvas is an html5 stop-gap
* The DOM is your enemy; gets in the way of game rendering

Found that DirectCanvas had an average performance increase of 500% vs.
normal canvas by bypassing the DOM with DirectCanvas

### Using DirectCanvas

Seperate:

1. DOM context (menuing, page layout)
2. DirectCanvas context (gameplay renddering)

Communicate between contexts AppMobi.canvas.execute() or AppMobi.view.execute()

DirectCanvas renders under the dom context, so dom shouldn't have background
images, colors, etc.

### Sound

Supports multi-channel sound

## The End of Day 1

Day 1 of the conference is over; time to head next door for Microsoft-sponsored open bar.

## Day 2 - November 2, 2011

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

[slides](http://gskinner.com/talks)

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

* [Processing.js](http://processingjs.org/)
* [Three.js](https://github.com/mrdoob/three.js/)
* [Crafty.js](http://craftyjs.com)
* [EaselJS](http://easeljs.com/)

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

### Q &amp; A

How are you driving all of the animation?

*You have three options; setinterval, settimeout, and requestanimationframe.
We use settimeout or requestanimationframe; RAF is usually said to be more
performant, but we've seen the opposite.*

## The State of HTML5 Games in Asia by Robert Van Os &amp; Chen Qi
*November 2, 11:00 - 11:45*

> This talk will discuss markets in China, Japan, and South Korea. Spil Games 
> is the first company to launch HTML5 mobile games in China and Japan, and we 
> have extensive experience with daily operations and customer feedback. We'll 
> discuss how HTML5 games make money in Asia -- and yes, they do make money! We 
> focus on mobile, and we'll cover lessons learned from HTML5 mobile games.

![Robert Van Os, Chen Qi](http://desmond.yfrog.com/Himg861/scaled.php?tn=0&server=861&filename=83858346.jpg&xsize=640&ysize=640)

[source: @ajacksified](http://j.mp/traja9 )

Traditional game stores, iOS market, Android market limits distribution. html5
provides distribution opportunities. Users don't care what technology something
is built in; they just want to play.

Company overview for [spilgames](http://www.spilgames.com/): spilgames is a 
large-scale distribution platform with a focus on age-based audience;
teens, girls, and family. 140M uniqe visitors a month, 19 different languages.
Targets localization as part of the core game experience.

* 85M of the 140M users are female
* 2.5B page views / month
* In companies:
    * \#1 in Brazil, Mexico, Indonesia, Argentina, Poland, Italy, Columbia,
      and Sweden
    * \#1 in US for female game players
* Split fairly evenly across continents
* First company with html5 mobile game portal
* Not a social network - socially available. Share high scores, achievements,
  etc across social networks

### Asia

Focuses heavily on html5 on mobile. Two parts: game portal, and distribution
network. Partners with local social networks like Baidu, Tencent. This allows
for a much larger distribution. Was the first html5 mobile game portal in China
and the first html5 game application in Mixi, the largest social network in
Japan.

Co-founded html5 dev community in China; 40k developers

Huge number of mobile users in China;: US has 295M mobile users; China has 900M

China has much smaller 3G penetration; Japan and South Korea has good penetration
(Japan about the same as the US)

Smartphones rapidly increasing in South Korea (40% of phones), Japan (10%)
and China (4%). In 2015, 200M of phones will be smartphones. Japan will have
over 76M. This means a *huge* potential market.

Interacting with local social networks is crucial for html5 games; Facebook is
very unpopular in Asian countries (blocked in China); small userbase in Japan,
South Korea, etc. The top local social networks support html5 apps.

From 2005 to 2009, microtransactions grew heavily as compared to purchasing
software up-front. The market for virtual items is huge; USD$1.3B in sales
on the DeNA network, $1-2B on Facebook, $0.6-1B on Zynga.

> In Japan, there are more mobile users than PC users.

> ARPU in Japan for DeNA network is 15x that of Zynga. Wow. #ngc11
[source: @kentquirk](http://j.mp/sGbryd)

Regulation can be an issue - China has the "Great Firewall", which can make
using cloud services difficult (or impossible.) You have to host your game from
within China, with regulated licensing. Most sites are blocked by default in
China; you have to have a license number on your website.

In order to deal with Korea, you also have to conform to Japanese regulations
and deal with strict content regulations.

You can use a communication service, but it can cause communication issues;
speed of development and communication can become an issue. It's easier to
have a local presence.

### Best Practices

#### App Discovery

> Don't just bet on an app store; be everywhere!

* Only have one substantial Android market (the Google market; alternatives
  such as the Amazon market exists, but splintering is an issue)
* Huge Android market fragmentation
* Apple has substantial userbase
* Restricted open web; business licensing, government restrictions; many users
  use start pages to begin with. Advertising can be done on these start pages,
  but cost ramps up heavily over time. This is why owning your own portal is
  key to success.

*--ed: like old AOL's start page?*

#### Payments

* Western: local payment methods, mobile payment methods like SMS, Offer-based
  to target groups
* Eastern: Credit cards unpopular (but rising); SMS payments popular

#### Audience

Localizing is more than just language; but also cultural differences.

> In China, users may want to harvest rice; in Ireland, users may harvest 
> potatoes

Keep time zones in mind; peak hours for users is different. 4pm in China, where
users play at the end of the work day; 7pm in Japan, where users play once they
get home.

In Western cultures, you can usually get away with games in English in
non-English countries (France, Spain, etc.) because most people understand
English. Language is much more fragmented in Eastern markets.

### Tips

* Compatibility issues exist, but as Android upgrades come along, it gets much 
  better (specifically with Android 2.3.
* Lots of difference between iOS and Android; find a balance between
  functionality and user experience. For example, multi-touch in older versions
  of Android isn't available.
* Android marketshare in Korea is 70% (10M); smaller but growing in Japan and China
  (5% [5M] and 2% [20M])
* Chinese and Japanese users are used to playing games in portrait mode
* Many users play games as convenient, with one hand (on the train, etc.)

Still some challenges:

* html5 audio
* canvas performance
* saving data traffic for users
* browser adoption is high, but user adoption of new browsers is low

### spilgames Offerings

* Global infastructure platform
* Feature detection API
* Localization support
* Authentication
* Payments
* Hosting
* Social graph (Friends, events, notifications); can cross-post across social
  networks

### Q &amp; A

In 2011, are you seeing more Facebook penetration (in comparison to existing
networks like Mixi?

*No. It's not taking over nearly as rapidly as other networks we've seen.*

## Lunch!
*Nov 2, 11:45 - 1:00*

![Lunch](http://desmond.yfrog.com/Himg860/scaled.php?tn=0&server=860&filename=jmng.jpg&xsize=640&ysize=640 "Lunch")

[source: @ajacksified](http://j.mp/vvCaSV)

## Porting Emberwind to HTML5 by Erik Moller

> Emberwind is a platform game published on Win/Mac/iOS. This talk is the story 
> of how Erik Möller took its 100,000 line C++ code-base and turned it into an 
> HTML5 game running on desktop, mobile and even TVs!

Links: 

* [Slides](http://t.co/q1qMP1Yd)
* [Emberwind html5 (Github Pages)](http://operasoftware.github.com/Emberwind/)
* [Emberwind (Github source)](https://github.com/operasoftware/Emberwind)
* [Emberwind home](http://www.timetrap.se/emberwind/index.php)
* [Erik's blog](http://my.opera.com/emoller/blog/)

![Erik Moller](http://desmond.yfrog.com/Himg739/scaled.php?tn=0&server=739&filename=vuomd.jpg&xsize=640&ysize=640 "Erik Moller")

[source: @ajacksified](http://j.mp/s8BrS7)

Twitter poll, checking who here is:

* Traditional Console Developers (a few)
* Traditional PC Game Developers (a few more)
* Traditional Web Developers (most)

> I used to work at this company where we had this one web developer, and we 
> used to laugh at him.

Web developement was a totally different set of skills from game development.

AAA titles saturate both GPU and CPU; unlikely to see AAA titles in html5
soon due to the level of abstraction for html5.

Goal was to convert Emberwind to html5, totally flash-free. It's valuable for
Opera to develop an application with their own technology, just like a game
console company would never build a console without testing it out with their
own software. It also helps find bugs, and determine what needs to be
optimized.

Emberwind has been tested in IE, FF, Chrome, Safari, and Opera;  on mobile,
Opera, Android browser, and Safari. It's open source on Github. 

![Emberwind Demo](http://desmond.yfrog.com/Himg741/scaled.php?tn=0&server=741&filename=h97iqu.jpg&xsize=640&ysize=640 "Emberwind Demo")

Emberwind has an in-game switch between canvas and WebGL, allowing testing
either easily. Canvas renders in an off-screen buffer to emulate some WebGL
features, so performance suffers somewhat.

Emberwind has 8 audio channels. To make this flash free, sound sprites were
used with multiple audio tags. Worked for Emberwind, which was fairly simple;
looking forward to Audio API.

Built-in debugger that shows canvas layers being built one-at-a-time. WebGL was
far more efficient than Canvas, and can draw most of the screen in a single
draw call; this was made easier by using texture atlases.

### Lessons Learned

Porting javascript was easy.

*--ed: probably because Javacript rocks.*

Time invested in tools for the rest of the team was a good investment. The
designer was able to go away for a couple weeks, and return with a mostly-
completed game. But do this pragmatically - particle effects were easier to
hardcode.

Reduce draw cals and state changes as much as possible.

Cheat where possible: all that matters is the end user experience. Estimate
physics using spheres for all the things. (Assume a spherical cow...)

Texture atlasses == CSS Sprites

### New Demo: 'Odin'

* WebGL game with lightmaps, normal maps, animations, skinning, shadow maps,
  and shader preprocessors
* Works with COLLADA source assets
* Open source on [Github](http://github.com/operasoftware/Odin)  

*-- ed: the pictures don't do it justice; very smooth-looking 3D asset loading
and editing.*

!["Odin"](http://desmond.yfrog.com/Himg615/scaled.php?tn=0&server=615&filename=y7evky.jpg&xsize=640&ysize=640 "Odin")
!["Odin"](http://desmond.yfrog.com/Himg532/scaled.php?tn=0&server=532&filename=hper.jpg&xsize=640&ysize=640 "Odin")

<iframe width="853" height="480" src="http://www.youtube.com/embed/wlu9-3zMctM" frameborder="0" allowfullscreen></iframe>

Took a video; not the best quality, and not very long, but you can see how
smooth the animation is.



WebGL allows you to specify vertex indeces; but Windows has performance issues.

### A &amp; Q

Have you looked into shader preprocessing?

*Not yet; I thought ours was a simple approach, bt I'm sure that there are
better alternatives to what we used. If anyone has any suggestions, feel free
to file bugs or fork the Github project.*

How easy or difficult was it to use your art asset pipeline?

*I had a couple of issues with COLLADA initially; the support is somewhat
spotty, unless you're using a plugin. But, exporting to COLLADA and running
a python script to output assets was pretty easy. It'll be easier once we get
runtime conversion in.*

The Odin demo, are you doing the animation in Javascript?

*Yes, that's done in Javascript; some of the devices like set-top boxes can't
handle too much work in the shaders.*

You had 36k triangls in the Odin demo; what's a reasonable scene size?

*There is a lot of difference in performance between the set-top boxes, mobile
phones, and desktop boxes; it depends on the platform you are developing for.
Javascript doesn't make much of a difference in performance; the layer is thin
between the javascript and the driver.*

Can you discuss the specs of the set-top box your Odin demo was for?

*I'm not sure.*

What was the process like converting from the 100k lines+ of C++ code? Was it
automated?

*It was all manual; we had the C++ source on one screen, and Javascript on
another. It took three unexperienced interns two months to port everything.*

When will Opera Mini (Mobile) have WebGL supprt?

*All of the technology is made in core, and trickles down to the platform team;
it will be in Opera 12 for the desktop, and will make its way down.*

## Using Google Closure for Lean and Mean Javascript Projects by Richard Anaya
*November 2, 2:00 - 2:45* 

[@richard\_anaya](http://www.twitter.com/richard_anaya)

> Creating high quality optimized code in javascript has many challenges. Why 
> not have a powerful tool by your side? This session will teach you about Google 
> Closure: a powerful set of tools from Google to help make large javascript 
> projects ( like game engines! ) easy to manage and optimize. We'll be talking 
> about code optimization, localization, templates, and documentation.


![Richard Anaya](http://desmond.yfrog.com/Himg739/scaled.php?tn=0&server=739&filename=buvhb.jpg&xsize=640&ysize=640 "Richard Anaya")

How do you build a large project with quality code that is maintainable over time?

We're trying to figure out how to use modern technology; modern compilers,
design patterns. We're trying to bring large, bold ideas to the web with code
that is increasingly complex; and we want to be proud of clean, elegant code.

### Javascript

* Good
    * Speedy
    * No types; dynamicness lends to rapid development
    * OOP through prototypes
    * Cross-platforms
* Bad
    * No types; less compile-time checking
    * Inconsistencies across browsers
    * Little help for organization *--ed: RequireJS / AMD?*
    * Code is an open book; minifiers only help so much

### Google Closure

Closure is a production-ready collection of tools:

* Closure Compiler
* Closure library (jQuery-like library)
* Closure templates (high-performance templates)
* Closure Linter (style checking)

Collection of tools to help make consistent code, and gives the user the
absolute minimum needed to run an app. 

#### Optimizations
The compiler helps concatenate dependencies and minimize unused paths to send
to the client. Closure compiler reads the code to find its intent, to optimize
execution paths and remove dead code.

#### Typing
Closure also figures out intent of code to provide type-switching for strings /
integers (to avoid 1 + "1" = 11 type issues)

#### Templates
Template files are written seperately, and Closure reads these files and can
quickly parse and return formatted text. Helps seperate language from templates
for internationalization. Give template sections the `desc` attribute, and the
closure compiler outputs XML files that can be localized.

    {namespace template.typerunner}

    {template .instructions}
      <h1>{msg desc="TypeRunner}</h1>
    {/template}

#### Compiler Tips

* JSDoc your code often; add comments to classes / methods
* Look at [Closure Library](http://code.google.com/closure/library/) for examples for everything
* Follow the [Google javascript guidelines](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* gjslint early and often
* Test in advanced mode often

### Using closure

Expose compiled code publicly using exports 
`goog.exportSymbol('typerunner.TypeRunner')` allows you to define externally-
usable things (to the window). Also, "externals" define what symbols should not
be renamed during compilation.

The gdream: turning good habits and quality code to well-optimized, obfuscated
code.

### Q &amp; A

I found myself using uglifyjs because I wasn't willing to buy into some of the
exports paradigms.

*There are some minification and truncation things you can do at the local
scope; but there aren't a ton of advantages of closure over other systems if
you're just compiling your code with simple compliation. You'll want to use
the other Closure offerings to really get the most out of optimizations.*

How would one use Closure with unit testing systems?

*Within the Closure compiler, there is a unit testing framework; if you're
looking to using all Google libraries, you can use the framework within
Closure.*

I come from using RequireJS; the workflow is build code, optimize, then minify;
can you run the Closure code without compiling anything?

*You can run the code, if you run using a harness that includes the Closure
libraries, but it's meant to be run as compiled. We do this to try to make
browser testing easy, as you'll only need to include a single file. Compliation
is very quick. The Compilation is good practice.*

I've got a large project in javascript already with a bunch of jsdoc
annotations; running it through Closure, I get a ton of warnings. What would
you recommend to how I could produce my old documentation extensions to use
with Closure compiler?

*People have gone into the Closure code and done their own modifications to the
compilation process; I'd recommend using the Google Closure linter and go step-
by-step over your code to see if everything follows Google standards. I
personally haven't done much modification there, but there are people who have.*

## Prizes and stuff
People got free O'Reilly books, Windows phones, and announced the winners of
the New Game Conf coding contest.

## An Initiative and Framework for 3D Gaming with HTML5 by Alan Kligman and Bobby Richter
*Nov 2, 3:00 - 3:45*

[Mozilla Paladin](https://wiki.mozilla.org/Paladin)

[Presentation Slides](https://github.com/benrito/paladin_presentations/blob/master/Paladin_NewGame.pdf)

> Paladin is an initiative by the Mozilla community at the intersection of 3D 
> gaming, JavaScript framework and library development, and the browser. We're 
> tied into the bits of the web that are up-and-coming, and intend to weaponize 
> them for gaming. And where the web is missing critical gaming support, we aim 
> to fill those gaps.
> 
> Three pieces are already spinning up: a framework written in JavaScript to 
> support 3D gaming in HTML5, a first game to help drive development of that 
> framework, and a web joystick API for Firefox. The framework currently offers 
> 3D modelling via CubicVR, physics by ammo.js (a cross-compilation of Bullet), 
> loading via require.js, and sound. More subsystems are likely to be up and 
> running by the time of this talk, where I'll review our goals, progress, and 
> opportunities to get involved.

![Alan Kligman and Bobby Richter](http://desmond.yfrog.com/Himg740/scaled.php?tn=0&server=740&filename=atqog.jpg&xsize=640&ysize=640 "Alan Kligman and Bobby Richter")

Started as the demo [Flight of the Navigator](http://mzl.la/fotn-ff) to push
forward

* FF Audio Data API
* WebGL
* Emscripten
* JS Enahancements
* Processing.js, Popcorn.js

Paladin aims to "weaponize" the web platform for 3D gaming by collaborating on
tools, libraries, and open standards.

### Gladius

Game engine writen in js / html5. Relies on libraries for graphics and physics;
graphics provided by [CubicVR.js](http://www.cubicvr.org/), and physics provided 
by [ammo.js](https://github.com/kripken/ammo.js/). Built to encompass modern
game engine design; modularity allows replacing chunks of functionality through
interfaces. Also provides an entity component system, allowing developers to
compose packages of components and extend the engine for specific games.

#### CubicVR.js

[Demos](https://github.com/cjcliffe/CubicVR.js/wiki/Examples-and-Demos)

Written along-side three.js; has grown into its own 3D engine, seperate from
but used in Gladius.

Asset pipeline: use Blender for modeling / animation, with COLLADA export 
with scene data in XML / json.

### RescueFox

First game prototype; quick and dirty demo, uses CubicVR.js and ammo.js. Helped
to figure out how to interface with APIs.

### Supporting APIs / Frameworks

* [html5 api spec for gamepad support](http://moz.la/mozgamepad)
* [BrowserID](https://browserid.org/)
* Mouse-lock API 
* Upcoming: Node editor for animations / composting

Getting involved:
* [Paladin Wiki](http://wiki.mozilla.org.Paladin)
* #paladin on irc.mozilla.org
* paladin-dev on Google Groups

[@alankligman (Alan Kligman)](http://www.twitter.com/alankligman)
[@secretrobotron (Bobby Richter)](http://www.twitter.com/secretrobotron)

### Q &amp; A

What are the biggest gaps you see in the WebGL API that prevent you from
getting games ready?

*We've been making do with what we have available today. There has been some
noise about compressed textures, but all of CubicVR runs really well. We
haven't seen any big gaps; things like compressed textures are nice to have,
but WebGL is great for its 1.0 and I'm looking forward to the next revision.*

## Web Audio API and other new features by Rachel Blum
*Nov 2, 4:00 - 4:45*

> There are lots of cool things in HTML5. Even better, there are lots of cool 
> things being _added_ to HTML5 and Chrome all the time, quite a few with a focus 
> on games. This talk is going to showcase some very recent and still-in-progress 
> features, including a long look at the Web Audio API.

![Rachel Blum](http://desmond.yfrog.com/Himg576/scaled.php?tn=0&server=576&filename=jwcx.jpg&xsize=640&ysize=640 "Rachel Blum")

Goal: develop on single platform. Moved to Chrome for the ability to push
forward standards and help make html5 better.

Good resouce: [caniuse.com](http://caniuse.com)

Graphics, physics improving rapidly; WebGL is pushing 3D graphics forward.

WebSockets allow low-latency, full-duplex UTF8 communication to servers for
realtime apps / games. Chrome 15 now supports binary, and UDP is being
discussed for WebSockets.

The asset pipeline is being improved with WebStorage, indexedDB, File API, and
Application Cache APIs.

File System API gives you a local, sandboxed filesystem to cache files such as
binary blobs; is Chrome-only, but hopefully will be adopted by other browsers.
It caches the entire app locally; is beneficial so that assets don't have to be
loaded during gameplay.

Add Cache Manifest file to specify files for caching; also allows fallback
files in case user is disconnected. Chrome allows debugging cache through
about:appCacheInternal. The Javascript console can also show what has been
cached. [html5rocks](http://www.html5rocks.com/en/tutorials/appcache/beginner/)
is a good resource.

### New Things

* Page Visibility API: detects if a user has the page in focus, to allow things
  like pausing
* Fullscreen API; allow fullscreen, chromeless apps; Chrome 16
* Connection checking API; ability to detect if online or not.
* requestAnimationFrame; allows better synchronization with CSS / SVG
  animation, browser doesn't render if page is not in focus
* Web Audio API; low-level audio API

### Web Audio API

Previously had `<bgsound "something.mid" >` and `<embed src="stuff.wav">`

Now have audio, but:

* Codec support varies across browsers
* Latency issues
* Glitches
* No effects, filters, or other low-level manipulation

When developing a low-level audio API, had many constraints:

* Don't want to use javascript, which is busy doing other things (app logic)
* Audio processing is more suited to CPU than GPU; many CPUs have audio
  processing functions built-in. Also, not all devices have a GPU.

Desired features, built into the Audio API

* Long sounds can be streamed through an audio source
* Javascript source nodes; you can generate wave forms in javascript
* Sample-accurate scheduling
* Gain control; allows cross-fading between channels
* Filter effects; low-pass, high-pass, band-pass, low shelf, high shelf,
  peaking / EQ
* notch
* allpass
* DelayNode for chorus effects
* DynamicCompressorNode for control / sweetening of the mix
* AudioPannerNode gives 3d positioning of sounds
* ConvolverNode, provides room ambience and other special effects
* Node Graph - doppler effect

### Behind the Scenes

* 2008: early prototype built based on OpenAL
* 2009: 
    * June: API design, find limitations of OpenAL, look for good design for the 
      web
    * September: Webkit &amp; js bindings, demoed together with Box2D
    * December: continued design work, presentation with Apple WebKit team
* 2010:
    * January: initial web audio integration; decisionon was made that web audio
      work should be done on seperate branch
    * May: Audio incubator with Mozilla Audio Data 
    * September: redesign &amp; refactor, merge approval with WebKit
    * Late 2010: merge with main webkit branch
* 2011:
    * Early:
        * W3C Audio Group
        * Chrome 10: OSX Web Audio behind a flag
        * Lots of porting
        * Chrome 12 brought on Windows and Linux
        * [Plink](http://labs.dinahmoe.com/plink/) and [ToneCraft](http://www.chromeexperiments.com/detail/tonecraft/)
    * Late:
        * September: Chrome 14 exposes Web Audio by default
        * October: [AudioJedit](http://audiojedit.herokuapp.com/), sample 
          editor in html5 w/ Web Audio API
        * October: Apple enables Web Audio in nightlies

### Participation

* Participate in W3C Groups
* Write your own code
* File bugs ([crbugs.com](http://crbugs.com))
* Talk to Developer Relations

### Upcoming Features

#### Mouse Lock

* [Web Events WG](http://www.w3.org/2010/webevents/)
* [Draft W3C Spec](http://dvcs.w3.org/hg/webevents/raw-file/default/mouse-lock.html)
* [crbug.com/72754](http://crbug.com/72754)
* [Pepper API sample: ppapi/examples/mouse\_lock](http://src.chromium.org/viewvc/chrome/trunk/src/ppapi/examples/mouse_lock/)

Why so long?

* Time to first draft: June 2011
* W3C spec &amp; discussions
* Coordination between WebKit teams
* Security implications, defining user experience
* Permissions UI

#### Joystick API

* W3C Draft Spec
* crbug.com/79100

#### WebRTC

Plugin-free high quality real-time voice / video communication in the browser;
BSD licensed

Features:

* Echo cancellation
* Voice Engine
* Video Engine
* Peer-to-peer connection stack

Resources:

* [webrtc.org](http://webrtc.org)
* [W3C Draft Spec](http://dev.w3.org/2011/webrtc/editor/webrtc.html)


#### Other Stuff

* Orientation lock - lock phone screen position
* Hardware Feature Detect
* High-performance timers
* In-app bug reports ([crbug.com/83642](http://crbug.com/83642))

### Q &amp; A

You mentioned that indexDB isn't suitable for putting in large binary assets;
why's that?

*Let me rephrase that: you can do that, but it seems like it's overkill. You
don't need a transactional database to hold game data. I don't have any
performance numbers, but it seems like it would be slower than raw file access.*

I'd like to hear a little more around your thought process around the 
variability of hardware, and how performance is managed. PC games have 
come up with some solutions, like minimum specs. Can we use that model?

*I think we can do some things, but we can't actually spec out what exactly
you would need to run a web app. The amount of devices is so vast that any
specs are unlikely to be accurate. You might be able to say what features you
support, that a device needs to support those features - but not a hardware
performance requirement.*
