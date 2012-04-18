---
layout: post
title: Inherited Initialization in Backbone
description: In which I do Backbone magic
permalink: /2012/04/inherited-initialization-in-backbone/index.html
categories:
- coffeescript
- javascript
- backbone
- mustache
- hoganjs
---

One of the niceties that Backbone provides is the ability to perform some level
of inheritance in your Backbone objects. You can, for example, create 'base'
models that provide some kind of functionality; perhaps they override the fetch
method to match your API. Views can share similar functionality or use sub-views
to share functionality across various pages.

Here's an example of an application that has two pages: a *home* page and a member's
*personal gallery* page, might look like this. This uses Twitter's
[Hogan.js](http://twitter.github.com/hogan.js/) (essentially, mustache templates.)
with Backbone code written in Coffeescript. Both render a photo gallery, based on
a model consisting of a collection of photos.

{% highlight coffeescript %}
App.Views.Home ||= {}

class App.Views.Home.IndexView extends Backbone.View
  template: HoganTemplates['photos/index']

  initialize: () ->
    @model.bind('reset', @render)
    @render()

  render: =>
    html = @template.render({ photos: @model.toJSON() }, { 
      photoGallery: HoganTemplates['shared/_photoGallery'],
    })

    @$el.find(".photoGallery").html(html)

    @$el.find('.photoGallery').flexslider();

App.Views.Members ||= {}

class App.Views.Members.GalleryView extends Backbone.View
  template: HoganTemplates['members/personalGallery']

  initialize: () ->
    @model.bind('reset', @render)

  render: =>
    html = @template.render({ photos: @model.toJSON() }, { 
      photoGallery: HoganTemplates['shared/_photoGallery'],
    })

    @$el.find(".photoGallery").html(html)

    @$el.find('.photoGallery').flexslider();
{% endhighlight %}

As you can see, *they're almost the same thing*. We can make a base class
to inherit from, such as a GalleryView that sets up the initialize and render
functions; but, that limits us from the ability to add custom initialize and
render functions on our individual page views.

*or does it?*

With a little trick, you can have your initialize and eat it, too. You can
call your ancestor's initialize function, and call your own function, at the
same time with a code sippet that looks like:

(javascript)

{% highlight javascript %}
this.constructor.__super__.initialize.apply(this, [options]);
{% endhighlight %}

(coffeescript)

{% highlight coffeescript %}
super
{% endhighlight %}

And turn that initial code into this:

{% highlight coffeescript %}
App.Views.Base ||= {}
class App.Views.Base.GalleryView extends Backbone.View
  initialize: () ->
    @model.bind('reset', @render)
    @render()

  render: =>
    @galleryView = new App.Views.Gallery.GalleryView({ model: @model, el: @$el.find(".photoGallery") })

App.Views.Gallery ||= {}
class App.Views.Gallery.GalleryView extends Backbone.View
  template: HoganTemplates['shared/_photoGallery']

  initialize: () ->
    @model.bind('reset', @render)

  render: () ->
    html = @template.render({ photos: @model.toJSON() }, { 
      photoGallery: HoganTemplates['shared/_photoGallery'],
    })

    @$el.html(html)
    @$el.flexSlider()

App.Views.Home ||= {}

class App.Views.Home.IndexView extends App.Views.Base.GalleryView
  template: HoganTemplates['photos/index']

  initialize: () ->
    @render()
    super

  render: =>
    html = @template.render({ })
    @$el.html(html)
    super

    @$el.find('.photoGallery').flexslider();


App.Views.Members ||= {}

class App.Views.Members.GalleryView extends Backbone.View
  template: HoganTemplates['members/personalGallery']

  initialize: () ->
    @render()
    super

  render: =>
    html = @template.render({ })
    @$el.html(html)
    super

    @$el.find('.photoGallery').flexslider();
{% endhighlight %}

Super calls it's ancestor's functions. Once the base template is rendered,
it calls the base view's, which builds a gallery and sets it up as a slider.

Neat!
