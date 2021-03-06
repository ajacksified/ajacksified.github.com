---
layout: post
title: Mediator.js 0.3 and 0.4
permalink: /2011/09/mediator-js-0-3-and-0-4/index.html
post_id: 410
categories: 
- Development
- github
- Javascript
- jsFiddle
- Mediator.js
- Programming
- Source code
---

(<a title="jsfiddle showing example of using Mediator.js 0.4" 
href="http://jsfiddle.net/ajacksified/vazAV/">Skip to the jsfiddle</a> if you'd 
like to see a live example)

I made some changes to Mediator.js which were originally going to be in 0.3, 
but just as I was about to push an update (the update being "assigning 
predicates to channels"), I decided to to go ahead and implement namespacing. 
You can now subscribe to things like "application:chat:receiveMessage" 
directly, or subscribe higher up on "application:chat" to receive everything 
under the namespace. Callbacks are applied recursively. Subscribing, 
publishing, and removing all work recursively with namespaces.

It was an interesting engineering challenge to turn "x:y:z" into 
Channel[x][y][z]; the transformation was pretty simple, but there's still 
something in the back of my mind shouting that "there's a better way to do 
this". Right now, it loops through and generates new Channel objects until it 
runs out of the split string's index, as such:

    var namespaceHeirarchy = namespace.split(':');

    if(namespaceHeirarchy.length > 0){

      for(var i = 0, j = namespaceHeirarchy.length; i < j; i++){
        if(!channel.HasChannel(namespaceHeirarchy[i])){
          channel.AddChannel(namespaceHeirarchy[i]);
        }

        channel = channel.ReturnChannel(namespaceHeirarchy[i]);
      }
    }

Some of the method signatures have been changed so that things make a little 
more sense; Subscribe now uses (channel, callback, _options_, _context_) where 
it used to use (channel, callback, _context_, _options_). It didn't make sense 
having callback come before options (especially as options are more likely to 
be used than context.) Options now holds the predicate (if there is one), 
rather than using the predicate as a channel. And as always, we have no 
external library dependencies. The minified/gzipped library is up to 580 bytes 
now, over my goal by 80... version 0.4.1 may be tweaked to get us back under 
that limit.

An internal change is that _Channel_ is now its own object with AddChannel, 
AddSubscriber , Remove, Publish, HasChannel commands, which makes it easier to 
nest channels in channels and tack on callbacks without losing track of 
context. It's all only used internally within Mediator (and, of course, all 
tested.)

The latest is [up on Github](https://github.com/ajacksified/Mediator.js) with 
an updated readme file that explains how to use everything. 
I've also updated [my example on 
jsfiddle](http://jsfiddle.net/ajacksified/vazAV/) with 
the latest method signature changes.
