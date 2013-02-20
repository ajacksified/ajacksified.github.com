---
layout: post
title: Fixing Bundle '.ccache' Permission Denied Issues on OSX
description: In which I give up and chown -R
permalink: /2012/02/fixing-bundle-ccache-permission-denied-issues-osx/index.html
categories:
- ruby
- bundler
---

I was attempting to install a gem, capybara-webkit, and kept getting errors on
OSX 10.8.2. Viewing the gem output log, it was attempting to create a file at
`/Users/jack/.ccache/a/1/<random letters>` and getting a 'permission
denied' issue. You can't `sudo` bundler, but `sudo gem install capybara-webkit`
executed just fine.

My solution seemed to fix things:

```
sudo chown -R airbnb /Users/jack/.ccache
```
