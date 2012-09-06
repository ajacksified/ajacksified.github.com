---
layout: post
title: Lua Testing with Busted and Travis-CI
description: In which I automate Lua testing #feelsgoodman
permalink: /2012/09/lua-testing-with-busted-and-travis-ci/index.html
categories:
- lua
- testing
- travis-ci
---

Lua's an awesome language. If you already know what Lua is, skip ahead to
the Busted section. If not, here's a brief overview.

Lua is a lightweight (< 50MB), super fast (fastest scripting language in
almost all benchmarks when using the Luajit interpreter), simple language. It's
primarily used in building game scripts, such as World of Warcraft plugins,
but is also used in applications like Adobe Lightroom (which is, or was, largely
written in Lua) and Redis's new scripting capabilities. It isn't very opinionated,
and I find it to be similar to Javascript in its flexibility; it can be used
functionally, OO, or procedurally which provides a wide range of applicability.

It's also, in my opinion, totally underutilized.

## Busted

[Busted](http://olivinelabs.com/busted) is a simple unit testing framework that
styles itself after such test frameworks as Mocha, Jasmine, RSpec, and \*Unit.
An example might look like this, taken from the official documentation:

{% highlight lua %}
require("busted")

describe("Busted unit testing framework", function()
  describe("should be awesome", function()
    it("should be easy to use", function()
      assert.truthy("Yup.")
    end)

    it("should have lots of features", function()
      -- deep check comparisons!
      assert.are.same({ table = "great"}, { table = "great" })

      -- or check by reference!
      assert.are_not.equal({ table = "great"}, { table = "great"})

      assert.true(1 == 1)
      assert.falsy(nil)
      assert.has.error(function() error("Wat") end, "Wat")
    end)

    it("should provide some shortcuts to common functions", function()
      assert.are.unique({ { thing = 1 }, { thing = 2 }, { thing = 3 } })
    end)

    it("should have mocks and spies for functional tests", function()
      local thing = require("thing_module")
      spy.spy_on(thing, "greet")
      thing.greet("Hi!")

      assert.spy(thing.greet).was.called()
      assert.spy(thing.greet).was.called_with("Hi!")
    end)
  end)
end)
{% endhighlight %}

Our goal was to make it super easy to read and write; to not have to look up
documentation every time you want a test, but at the same time, build in
flexibility to cover enough use cases to make it usable by the entire Lua
community. So, we provide things like internationalization support (with 6
languages built in at time of writing), definable output types (for human or
machine consumption), and composable assertions.

Testing using busted is really easy. The convention I usually follow is:

* Create a `spec` folder in my code repository
* Write *module*\_spec.lua files to hold unit tests, broken up by classes or
  some other granular unit of functionality
* Run `busted spec` from project root.

### Installing

Installing busted is easy:

**OSX**

* `brew install luajit`
* `brew install luarocks` (luarocks is the "ruby gems" of Lua)
* `luarocks install busted`

**Linux**

* `sudo apt-get install luajit`
* `sudo apt-get install luarocks`
* `sudo luarocks install busted`

**Windows**

(Forthcoming when I bug my Windows-using friend about it. Install lua, then
Luarocks, then `luarocks install busted`.)

### Further Discussion on Busted

The [busted docs](http://olivinelabs.com/busted) are fairly solid; I reocmmend
you check them out. There's also plenty of information on the internet about
testing and TDD and the like. So let's move on to integrating with Travis!

## Travis

[Travis](http://travis-ci.org) is a *really cool* tool that watches changes to
Github repositories and automatically runs tests. It's particularly useful
because you can check, every time you commit code, if you derped and broke
something that you shouldn't have. It has built-in support for many popular
languages, such as Ruby, Javascript, and others, but nothing for Lua yet.
However, it's pretty easy to abuse a worker meant for another language to get
it running busted specs.

Create an account on [Travis](http://travis-ci.org), then enable Travis to watch
your repository. Next, you'll create a `.travis.yml` file in the root of your project.

In this example `.travis.yml` file, we'll fake out an erlang worker and run our
busted specs using both lua 5.1 and luajit. We define a series of installation
steps that will get everything installed and run the script, then post the
results to a web hook and send an email to me.

{% highlight yaml %}
language: erlang

env:
  - LUA=""
  - LUA="luajit"

branches:
  only:
    - master

install:
  - sudo apt-get install luajit
  - sudo apt-get install luarocks
  - sudo luarocks install luafilesystem
  - sudo luarocks install busted

script: "busted spec"

notifications:
  webhooks:
    - http://my-url/travis-results
  recipients:
    - engineering@company.com
  email:
    on_success: change
    on_failure: always

{% endhighlight %}

At Olivine Labs, our webhook is actually a hubot endpoint that uses this
[travis-ci hubot script](https://gist.github.com/3660666).  This allows our 
chatbot to display the status in chat every time someone makes a commit or
pull request.

    -----------------
    PASSED on Lua <luassert> [06aad23b79686b6b1293eafcac687658823fd18b]
    Compare: https://github.com/Olivine-Labs/luassert/compare/066101917a96...06aad23b7968
    Committed by Jack Lawson at 2012-09-06T20:04:34Z
    -----------------

Check it out, and feel free to leave feedback and questions!
