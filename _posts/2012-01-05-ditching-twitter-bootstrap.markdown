---
layout: post
title: Ditching Twitter Bootstrap
description: In which I update my blog's design
permalink: /2012/01/ditching-twitter-bootstrap/index.html
categories:
- css
- meta
---

Until recently, my blog was using a fairly vanilla version of the CSS framework put
together by the developers at Twitter, [Bootstrap](http://twitter.github.com/bootstrap).
It's a really full-featured framework that gives grids, headings, links, buttons,
and all manner of form elements a clean and consistent design. You can see another
example at the [Alchemy Websockets](http://alchemywebsockets.net) site.
I used it- as I think many people do- as a way to get a decent site deisgn up
in a matter of minutes; one can download or clone the repository, and use as-is
or tweak a few LESS variables and compile a new version complete with new colors
and gradients.

I put a bit more work into the Alchemy site than I did on my blog; I originally
was more concerned about setting up jekyll correctly and getting everything
online than learning *another* layer of abstraction between me and my code.
In fact, that's probably Bootstrap's greatest contribution to me - I learned more
about LESS while tweaking link colors and gradient styles for Alchemy. I'm still
not sold on the concept of CSS preprocessors on a wide-scale team use, but I'm
experimenting with it on the new Neflaria site and hope to have a much more
substantial recommendation once that project is complete (fingers crossed,
June.)

As all bootstrapping frameworks must (or, should), the monotony eventually got to me.
I only used about 10% of the features, and the design just didn't feel right if it
wasn't either heavily customized or built from the ground-up. It is my blog,
after all - and even if it'll take a little work to bring it back up to speed,
to homogenize the components and get all the elements lined up correctly, it feels
a little more like home now.

Related: [subtlepatterns.com](http://subtlepatterns.com) is amazing. Also, I'm
a happy user of the [1140 grid](http://cssgrid.net/) which provides a fluid-ish
responsive grid system.
