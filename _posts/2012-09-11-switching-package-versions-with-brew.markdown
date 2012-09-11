---
layout: post
title: Switching Package Versions with Brew
description: In which I switch between lua 5.1.x and 5.2.x
permalink: /2012/09/switching-package-versions-with-brew/index.html
categories:
- lua
- osx
- brew
---

I use [homebrew](mxcl.github.com/homebrew/) to manage installations of things
like Lua, node, mysql, redis, vim, and more on OSX; I find brew essential to 
setting up and maintaining a dev environment. It has a ton of packages
available and makes it super easy to stay up to date with the latest packages.

[busted](http://olivinelabs.com/busted) relies on a package called luafilesystem
for directory traversal; however, lfs relies on Lua 5.1.x. The version in
homebrew is 5.2. Switching is as easy as:

```
brew versions lua
```

which returns

```
5.2.1    git checkout 894f668 /usr/local/Library/Formula/lua.rb
5.1.4    git checkout 7e5bc89 /usr/local/Library/Formula/lua.rb
```

so I can

```
brew switch lua 5.1.4
```

And use 5.1.4 instead of 5.2.1, allowing me to use busted (and test lfs fixes
in 5.2!) This works, in theory, with any brew package.
