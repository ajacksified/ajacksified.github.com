---
layout: post
title: On Modern Web Development
description: In which I make a plea to developers
permalink: /2011/11/on-modern-web-development/index.html
categories:
- html5
---

I shall begin thusly:

    HTML5 and related technologies provide tools with which to enhance
    accessibility through well-structured, sementically-defined content.

And I will asert this:

    The ability to include javascript, webgl, canvas, and other rich media
    is meaningless if you use these tools like you would Flash.

Lately, there have been a lot of blog posts, links on [hacker news](http://news.ycombinator.com),
links on [reddit](http://www.reddit.com), and elsewhere where someone's showing off their latest application or framework. Javacript application
frameworks have never been more popular- like my personal favorite, [backbone](documentcloud.github.com/backbone/).

With these tools, it's easy and simple to build single-page applications. One looks at Twitter, or Facebook, or Grooveshark, and admires the clean
presentation and seamless(ish) transitions between application areas. So, developers go forth with the intent to replicate this kind of experience.
We import a framework like Backbone, and start coding away. Maybe you use something Rails and TDD our way through a nice group of controllers serving
JSON right to our client-side models. We use Jasmine, and TDD our way through the client-side, and use all the "best practices" that we've been tought,
and we deliver a great product on time.

You've built an application that looks pretty, that's fast and performant and efficient, that followed "best practices" for development technique,
but it has one major problem: if there's no non-javascript representation, it isn't machine-readable. Or in other words, if it's a strictly
javascript-based application, that does everything client-side except for the retrievel and storage of data, then you're just as good off as having
built it in Flash; maybe even worse.

## SEO Implications
Googlebot *has* an API for ajax calls, but you have to conform to their specifications. Search engines work by scraping your site's content; and if you
don't have an html representation of you data, it can't scrape anything.

## Accessibility Implications
You're going to be alienating a lot of users who use screen readers. Some can watch for new content, but not all.

## Availability Implications
A javascript-heavy website on a first-gen iPad or a lower-powered tablet does not run well. Also consider set-top boxes like Google TV. 
Modern mobile devices have no problem, but earlier devices won't even be able to use your application.

I can't guess or give you recommendations on how many people you might be ignoring. But I can try to make a case and tell you that 
there's a *right* way to build web applications where you won't run into these problems.

* Start by building your client through HTML. No styles, no javascript. Just HTML. That means using plain, old, boring vanilla links and forms.
  *note: this is a good spot to use html5. section, head, aside, footer, input types, etc. are all there for a reason. This is that reason.*
* Build your server-side:
    * With a RESTful interface. This will let you route HTML request, ajax / JSON requests, and anything else through a common API that you can use
      internally and have the option to externalize. You're building for the future - you can build any client that can produce a RESTful call,
      which is pretty much anything. You'll use the same routes whether requesting JSON or HTML, which means you have to change nothing to switch
      between calls.
    * With logicless templating. I can go on forever about how great [Mustache](http://mustache.github.com/) is. Fill these views with a flattened-down
      version of your model (also known as a viewmodel in some circles.) This will allow you to use the same logic and output whether outputting HTML,
      JSON, XML, or anything. If it's a JSON request, JSONify the viewmodel. If it's HTML, fill a mustache (or erb, or whatever) template with this
      viewmodel and send that. One line of code difference between request type is the goal.
* Layer on your css. This probably sounds like Progressive Enhancement, because it is.
* Now, add your framework on top. If you're using a framework that supports Mustache templates, you can use the same code on your client that you are
  on your server - which means no duplication. Same html, same css, same everything.

You're probably doing most of this already anyway.  The major thing to take away is that now you're building your templates first instead of implementing your
client framework first. With a very small change, if you're already writing well-structured markup (which you can easily check while you're building your
css-less templates), you've built yourself a single-page app that's also accessible. You've built an application that can handle any request type, that provides
an API, that is easily scalable (thanks to RESTfulness, you can easily replicate it across webservers as quick as you can clone them), and that re-uses
as much code as possible, and is therefore easily maintainable.



So my plea: Build in html everything you can. Don't develop Flash in HTML5.



*The only thing I can think of that could probably get away from it is a completely interactive experience such as an online game. But even then, giving 
your players forums, stores, chat rooms, etc. to discuss the game in is just as important to the health and growth of your game as the game itself is.
Having these tools available can make a world of difference.*
