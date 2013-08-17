---
layout: post
title: Dismantling the Monorail
description: In which I give a talk at Heroku Waza 2013 about monolithic architectures
permalink: /2012/03/dismantling-the-monorail/index.html
categories:
- architecture
- rails
- mvc
---

I gave a talk at Heroku's Waza conference this year, discussing moving from
monolithic applications to service oriented architecture. My goal was to
dispell the myth that SOAs are particularly complex or that they slow down
development time for both scrambling startups and mature businesses.
Neither are true, and starting with a simple appliation structure- simply
starting with two applications, one serving a RESTful data API, and one to use the
API and serve your front-end (be it web, mobile, or otherwise)- will, in
most cases, increase your efficiency, increase the longevity of your code,
and will allow you to scale in application complexity without sacrificing
code quality or agility.

<div class="media-container">
  <iframe src="http://player.vimeo.com/video/61043049" width="400" height="300" frameborder="0" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe>
</div>

