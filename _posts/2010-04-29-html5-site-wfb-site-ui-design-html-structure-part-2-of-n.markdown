---
layout: post
title: "HTML5 Site WFB Site UI Design / HTML Structure (Part 2 of n)"
permalink: /2010/04/html5-site-wfb-site-ui-design-html-structure-part-2-of-n/index.html
post_id: 234
categories: 
- css
- Development
- Firefox
- Google
- google chrome
- Google Maps
- html
- html 5
- HTML5
- JQuery
- RSS
- User Interface Design
- Web Design
- Web Standards
---
i_
([Back to beginning of series](http://www.thejacklawson.com/index.php/2010/04/html5-site-waterford-family-bowl-part-1-of-n/))

Part 2: Site UI Design / HTML Structure

When I first picked up the project, I had a few things to consider: what 
requirements do I have, and once I had those, how would I display these feature 
in a neat manner, and what technologies would I use to implement them. The 
requirements part came out pretty easily, after a couple of phone conversations:

* Calendar of events
* Photo Gallery
* News section
* A few generic pages
* Email mailing list
* Misc. vital information (times open, address, that sort of thing)

And now I needed to put together some kind of design. I had recently finished 
reading a few usability books, so I was feeling pretty good about putting some 
of these ideas to work. I knew that people coming to the website were probably 
going for directions, a phone number, or hours of operation, so I put high 
priority on these being visible and easy to use (such as a Google Maps link for 
the address). I also wanted to entice people to explore other areas, so I added 
some photos from the photo gallery towards the top of the page. Finally, I 
included elements from all the page- just little teasers- across the front 
page, such as a small calendar of events, news block, and a blurb and picture 
about their restaurant. Now, I had a home page with all the information a user 
needs, quickly and at the top, while encouraging visitors to browse a little 
and check out the gallery and other pages for even more information.

With this sketched out on paper, it was time to actually write out the HTML. I 
came up with a structure similar to [my HTML5 Template](http://www.thejacklawson.com/index.php/2010/04/my-html5-template/)
(and in fact, it's what I based my 
template off of.) I wanted to use the new semantic HTML5 elements and 
attributes, such as example text on textboxes:

    <input type="text" name="Email" id="email" maxlength="60" placeholder="email@youraddress.com" />

I used the help of a [jQuery Watermark plugin](http://jquery-watermark.googlecode.com/) 
to help style the textboxes as 
well, since few browsers actually pay attention to the placeholder attribute 
yet. Further HTML5 inclusions were sections for the individual blocks of text, 
an aside for the side column, header, footer, and nav elements. Besides the 
HTML5, I also implemented a link tag for the news RSS feed:

    <link rel="alternate" type="application/rss+xml" title="RSS" href="http://www.waterfordfamilybowl.com/wordpress/index.php/feed/" />

This allows browsers and whatever happens to crawl your site to say "hey, 
there's an RSS feed here" and allow users to easily pick up the news RSS feed. 

