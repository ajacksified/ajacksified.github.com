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

* [Emberwind html5 (Github Pages)](http://operasoftware.github.com/Emberwind/)
* [Emberwind (Github source)](https://github.com/operasoftware/Emberwind)
* [Emberwind home](http://www.timetrap.se/emberwind/index.php)
* [Erik's blog](http://my.opera.com/emoller/blog/)

![Erik Moller](http://desmond.yfrog.com/Himg739/scaled.php?tn=0&server=739&filename=vuomd.jpg&xsize=640&ysize=640 "Erik Moller")

Twitter poll, checking who here is:

* Traditional Console Developers (a few)
* Traditional PC Game Developers (a few more)
* Traditional Web Developers (most)

> I used to work at this company where we had this one web developer, and we 
> used to laugh at him.

Web developement was a totally different set of skills from game development.
