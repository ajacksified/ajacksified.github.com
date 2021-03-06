---
layout: post
title: jQuery 1.4.2 updates, and the release of jQuery UI 1.8rc2
permalink: /2010/02/jquery-1-4-2-jquery-ui-1-8rc2/index.html
post_id: 207
categories: 
- Development
- Javascript
- JQuery
- jquery 1.4
- jquery 1.4.1
- jquery 1.4.2
- jQuery-UI
---

<a class="zem_slink" title="JQuery" rel="homepage" 
href="http://jquery.com/">jQuery</a> 1.4.2 is out- not officially on their 
website, but you can be sneaky and get it here:

<a href="http://code.jquery.com/jquery-1.4.2.js">http://code.jquery.com/jquery-1.4.2.js</a>

<a 
href="http://code.jquery.com/jquery-1.4.2.min.js">http://code.jquery.com/jquery-1.4.2.min.js</a> (not really "minified" as much as "closure compiled"... can we 
get a verb for that?) The changelog isn't up yet, but that's generally the last 
thing to go (and hey, we snuck in the back door to grab the update anyway, 
right?) I imagine it'll eventually find it's way to <a 
href="http://api.jquery.com/category/version/1.4.2/">their API page for 1.4.2</a>.  I ran a quick diff; here's some changes that I found notable from 
jQuery 1.4.1 to 1.4.2:

* Optimizations for the body selector
* A few changes to how arrays of elements selected are built, I'll go with my 
gut and say that they're optimizations too
* Trimming whitespace from JSON data responses, for good ol' IE
* A more few various bugfixy-looking things, mostly targeting IE
* It looks like there was a lot done to the event-adding code; I think mostly 
internal changes. Probably more optimizations? I'll leave it to Resig or 
someone to explain whenever the changelog comes out. A few (generally pretty 
deep) methods had some parameters changed, or were renamed (things like 
"jQuery.event.special.submit.remove" becoming 
"jQuery.event.special.submit.teardown")
* A smattering of changes within "live" and "die" mostly centering around the 
usage of namespaces
* Blackberry browser bugfix concerning converting NodeLists to arrays
* jQuery.getText changed to jQuery.text
* Changes to caching fragments to help out WebKit and IE 6

I haven't really dug too deeply in the jQuery core before, so I'm not totally 
qualified to give a whole lot more detail than that. I used SourceGear 
Diffmerge between 1.4.1 and 1.4.2 to find out what I found out.

Also, in related (and perhaps more _official _news),  the jQuery UI team <a 
href="http://blog.jqueryui.com/2010/02/jquery-ui-18rc2/" 
target="_blank">released jQuery UI 1.8rc2</a> - which includes bugfixes in the 
already pretty stable 1.8rc1 (<a 
href="http://jqueryui.com/docs/Changelog/1.8rc2" 
target="_blank">changelog</a>). You can grab it:

<a href="http://jquery-ui.googlecode.com/files/jquery-ui-1.8rc2.zip" 
target="_blank">http://jquery-ui.googlecode.com/files/jquery-ui-1.8rc2.zip</a> 
(development bundle)

As of right now, I'm testing it in our application at MeM. Let's see how things 
go!
