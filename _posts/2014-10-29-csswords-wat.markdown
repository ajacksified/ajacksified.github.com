---
layout: post
title: CSS*WORDS and WATWATWAT
description: In which I share a couple of silly things I made.
permalink: /2014/10/csswordswat/index.html
categories:
- javascript
---

Lately, I've been sliding back into the habit of working late at night (or, at
least, certainly intending to.) However, a break between leaving work and the
late night makes it somwhat difficult to get back into the swing of things.

I find it often helps to build a simple, useless, weird project before getting
started to help get my mind in the right place. Some of my most recent projects
are:

### [watwatwat](http://watwatw.at)

It's a weird bag of clever and overengineered CSS that gives you something
that vaguely resembles what you might remember working on an old, gaussed CRT
looked like. You can type `wat ` over and over. It also served to teach me
about some of the weirder corners of the latest CSS specs, like
`webkit-mask-image` - I'm particularly proud of how I got the corners, and only
the corners, to blur on the "screen". (And using a radial gradient at that.)

I also used that as inspiration for my latest blog design. Not sure if it'll
stick or not, but here's trying. The color scheme is based on
[Andrea Leopardi's Gotham](https://github.com/whatyouhide/vim-gotham) and I
implemented it while listening to a lot of
[Com Truise](http://rd.io/x/QCfeLUjRAA/).

### [CSS*WORDS](/csswords)

I took a listing from `/usr/share/dict/words`, then `grep`ped it to find what
words had `[0-9a-f]` along with some letters I could translate
(`l = 1, z = 2`, etc.) I then took that and built a big page of all the colors.
It made me realize how long it's been since I actually did manual DOM
manipulation without fifteen layers of libraries abstracting things away. I'm,
manually creatig document fragments and stuff. it's crazy. At one point I even
wrote my own `new XMLHttpRequest()` before I moved the `colors` array into the
same file for performance.

Anyway, I hope someone gets a chuckle, or at least exhales sharply out of their
nostrils. It was fun. Now, back to [work](https://github.com/reddit/switcharoo).

