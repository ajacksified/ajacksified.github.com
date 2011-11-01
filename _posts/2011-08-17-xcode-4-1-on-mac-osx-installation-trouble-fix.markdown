---
layout: post
title: XCode 4.1 on Mac OSX Installation Trouble (fix)
permalink: /2011/08/xcode-4-1-on-mac-osx-installation-trouble-fix/index.html
post_id: 363
categories: 
- OSX and Apple Technologies
---

I upgraded to OSX Lion, and decided one day to use [homebrew](http://mxcl.github.com/homebrew/) 
to give a package called [git-flow](https://github.com/nvie/gitflow) a shot. The installation failed with a somewhat 
cryptic message, but suggested running "brew doctor" to find out what was 
wrong. And, indeed, XCode was gone from Lion - it was on my previous Snow 
Leopard installation, but failed to carry over. "No big deal," says I, "a 
simple reinstall."

_Wrong._

The trouble started when I found out that you have to download XCode from the 
App Store. "Ah," says I, "I guess this makes sense, they want to push everyone 
through the app store. Annoying, but whatever." And so I download the 3gb-ish 
file, and the App Store cheerily let me know that it was, indeed "Installed." I 
tried brew again, and no dice. Apparently all the app store downloads is an 
installer. So, by "installed", it means "downloaded installer, but nothing's 
actually installed." And so, with great joy, I clicked "Install XCode" from 
within the applications folder.

Nope.

After running for a few minutes, it failed and suggested looking at the log, 
which, after several thousand status messages, let me know that one of the 
packages failed to extract properly. After a quick Googling, I found that many 
other people had the same problem, and suggested re-downloading it; so, I did. 
This time a different package failed. The fourth and fifth installs failed, 
too. I tried copypasting it in other directories, tried opening the package and 
running the xcode package directly, and nothing worked. I hunted down and ran 
the uninstall script for the old version of XCode that was still hanging round. 
At last, I finally saw someone who [brilliantly suggested](https://discussions.apple.com/message/15931741#15931741):

    sudo mv /Applications/Install\ Xcode.app/ /Applications/Install_Xcode.app
    sudo open /Applications/Install_XCode.app

And suddenly, XCode worked!
