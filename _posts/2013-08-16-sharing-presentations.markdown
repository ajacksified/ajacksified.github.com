---
layout: post
title: Sharing Presentations
description: In which I discuss a super easy way to share your presentations
permalink: /2013/08/sharing-presentations/index.html
categories:
- speaking
---

I've been using the supremely excellent
[reveal.js](http://lab.hakim.se/reveal-js/) library for all of my presentations
in the last year. It's super easy to use, it's very lightweight, and I can
build a sharp presentation without having to download or install some 400mb
software package on my computer *and* the computer I'm using to present, if,
God forbid, it's not the same as I used to write it.

Also, by virtue of being html, I can write my own theme files in CSS and
plugins in Javascript. Some plugins supported include secondary-monitor speaker
notes, remote control over mobile phone, markdown, and code syntax
highlighting. It can also print all the slides to PDF in individual pages. And,
perhaps best of all, you can use gifs as massive background images.

Oh, and if this wasn't already magical enough, I can store all of my
presentations on GitHub. That allows for sharing, pull requests, and quick
updates through either Vim or the web ui.

That's when it struck me today - not only can I put my slides on GitHub for
keeping track of edits and all that Git goodness, but I can create a `gh-pages`
branch, and it will *automatically host my slides under my domain name*.

But wait, there's more. You don't even have to hop into the terminal to do it.
In GitHub, click on the 'branches' button, type in `gh-pages`,
and a minute later you're up and running.

Check out these examples:

* [thejacklawson.com/how-to-hubot](/how-to-hubot)
* [thejacklawson.com/dismantling-the-monorail](/dismantling-the-monorail)
* [thejacklawson.com/javascript-testing](/javascript-testing)


