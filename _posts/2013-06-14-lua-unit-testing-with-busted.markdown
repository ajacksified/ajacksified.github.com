---
layout: post
title: Lua Unit Testing With Busted
description: In which I discuss how to write the best Lua code of your life.
permalink: /2013/06/lua-unit-testing-with-busted/index.html
categories:
- lua
- tdd
- busted
---

### History of Busted

You may have been hearing a lot about Lua; it's used in an awful lot of
[neat](http://redis.io/commands/eval)
[places](https://blog.wikimedia.org/2013/03/11/lua-templates-faster-more-flexible-pages/)
[nowadays](http://leafo.net/lapis/). Lua's actually been used in lots of neat
places for a while, but it seems that it's gotten a bit of spotlight recently,
with things like Redis integration, Wikipedia templates, mobile development,
and gaming of all sorts.

Lua's *really* fast, especially with Luajit (it's been removed from the
[benchmarks game](http://benchmarksgame.alioth.debian.org/), but you can look
at the Lua benchmarks and extrapolate data from the official
[Luajit benchmarks](http://luajit.org/performance_x86.html)). It's also fairly
widely used in the gaming world, and it's a fun and easy language to use.
So, for some fun projects,
[my friends and I](https://www.github.com/Olivine-Labs) are building
games using Lua to run RESTful game servers, with Node.js to build the
web game interfaces.

The only issue we've had so far with Lua is that we've had to build a few tools
ourselves to get the same environment that we enjoy in other languages, like
Node.js, so we can be just as effective. One of the tools we built is
[busted](https://olivinelabs.com/busted), a testing framework written in Lua
for Lua. It started off as a few assertions, and has grown into a full library
including everything from testing deep-equality of tables (Lua's version of
objects), to spies and mocks, to asynchonous, time-based tests.

Our first goal was to make it simple and easy to use, taking cues from such
frameworks as Jasmine, Mocha, RSpec, JUnit, and others. We also wanted to have
really nice terminal output and integration with testing frameworks, so you
could run your Lua tests from systems like Travis CI, Jenkins, and anything
else. We integrated i18n support through
[a simple i18n library](https://olivinelabs.com/say), and we're now translated
into 9 different langauges!

Busted uses our [luassert](https://github.com/Olivine-Labs/luassert) library,
which is separated from busted itself so that any testing library can use them.

Busted also supports [Moonscript](http://moonscript.org/), a Coffeescript
analogue, and [Terra](terralang.org), a low-level counterpart to Lua.

With that background, let's dig a little into how to use busted for your own
projects.

### Installation

The first thing you'll need to do is install Luarocks. Depending on your
environment, you can `apt-get luarocks`, `brew install luarocks`, or otherwise
get it from [luarocks.org](http://www.luarocks.org). That done, you can run
`luarocks install busted` to install the busted runtime and executable script,
which will run in most variations of Linuxes, OSX, and Windows.

### Writing Tests

The homepage has an example that looks something like:

```lua
describe("Busted unit testing framework", function()
  describe("should be awesome", function()
    it("should be easy to use", function()
      assert.truthy("Yup.")
    end)

    it("should provide some shortcuts to common functions", function()
      assert.are.unique({{ thing = 1 }, { thing = 2 }, { thing = 3 }})
    end)

    it("should have mocks and spies for functional tests", function()
      local thing = require("thing_module")
      spy.on(thing, "greet")
      thing.greet("Hi!")

      assert.spy(thing.greet).was.called()
      assert.spy(thing.greet).was.called_with("Hi!")
    end)
  end)
end)
```

If you save this to a file like `spec/busted_spec.lua`, you can run `busted`,
which will automatically look for files matching `spec/*_spec.lua`,
recursively. You can also use command-line options to change the pattern.

At the top of the spec file, after doing any `require`ing of code or
initialization of globals, you can start writing a `describe` block. That block
will take a name and a function. Inside the block, you can write `it` blocks,
which are the tests themselves and will contain any `assert`s; you can also
write `pending` blocks for tests you'll come back to later.

Busted's super easy to get started with; read up in the docs to find out more
about spies, async tests, assertions, internationalization, and more!

