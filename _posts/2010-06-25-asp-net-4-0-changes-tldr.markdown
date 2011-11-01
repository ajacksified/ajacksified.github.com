---
layout: post
title: ASP.NET 4.0 Changes tl;dr
permalink: /2010/06/asp-net-4-0-changes-tldr/index.html
post_id: 252
categories: 
- ASP.NET
- Development
---

*ASP.NET 4.0 Updates* - 
[http://www.asp.net/learn/whitepapers/aspnet4](http://www.asp.net/learn/whitepapers/aspnet4)

tl;dr:

* Stuff moved from web.config to machine.config
* More robust XSS checking and form post validation
* More streamlined inclusion of Microsoft AJAX framework (UpdatePanels), and 
jQuery (which Microsoft officially includes now)
* More control over viewstate
* Built-in page routing for webforms (how MVC urls work, but now works with 
webforms. [www.mem.com/contentdisplay/5123431](http://www.mem.com/contentdisplay/5123431) for example)
* Setting client IDs on controls instead of ASP.NET building it for you
* New Chart control
* Project templates slimmed down
* Better CSS styling on controls, option to not render tables around some 
controls (formview, login, stuff like that)
* ASP.NET MVC2
* Intellisense improvements

Tl;dr: tl;dr: Lots of little updates that will make asp.net better.

*C# 4.0 Updates* - [MSDN C# 2010 Samples](http://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=cs2010samples&DownloadId=10177)

tl;dr:

* Dynamic binding; create objects and use methods and properties that may not 
exist yet on a “dynamic” typed object. This gets resolved at runtime. Upside: 
use non-type-safe languages and crazy voodoo magic where you don’t know the 
type. Downside: you only catch bugs at runtime. Use sparingly, but for awesome 
things. “dynamic” objects can literally do anything, as long as whatever it 
ends up as supports it. No lambdas on dynamic.

* Tl;dr: magic, type safety is so 2009
* Named and optional arguments; you don’t have to enter in your parameters in 
order, or even enter in all of them.
* “string”- that’s lower-case, kids- is an object now.



Tl;dr: tl;dr: Dynamic programming is the key word. Also VB updates, but if a 
tree falls in the forest and nobody’s around to hear it, does anyone care?

*ASP.NET 4.0 Breaking Changes* - [.NET 4 Breaking changes](http://www.asp.net/learn/whitepapers/aspnet4/breaking-changes)

tl;dr:

* A bunch of stuff you can do to revert changes, so I dunno why they’re in the 
“Breaking changes list”. Read for details, sorry.
* Stronger .aspx parsing, controls with random characters breaking stuff will 
break stuff. Our code should be clean, but needs to be looked at anyway.
* Stronger request validation, so errors may occur on posts that didn’t 
previously. To revert, we can add a line in the web.config if it gets crazy. 
Definitely check pages with rich text controls.
* Update your projects through visual studio to .net 4.0 so your web config 
doesn’t get borked. They support it, but it’ll make life easier.
* A bunch of troubleshooting options

tl;dr: tl;dr: More stuff, almost definitely won’t break existing code we have. 
Double-check pages submitting HTML (rich text areas.)

*tl;dr: Better standards, MVC 2, Dynamic typing, stronger parsing and 
validation*
