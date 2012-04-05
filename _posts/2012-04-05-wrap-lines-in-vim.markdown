---
layout: post
title: Wrap Lines in Vim
description: In which I share '!fold -80 -s'
permalink: /2012/04/wrap-lines-in-vim/index.html
categories:
- vim
---

If you search for things like 'how to wrap lines in vim', 'line wrapping vim', 
'80 characters vim', etc, you will often find people saying ':set textwidth=XX'.

But if you want to break, say, selected lines - or if this just plain doens't
work for you, try this: `!fold -w##`. Add -s on the end to break at words. To,
for example, select text and wrap it at 80-line sentences, breaking at words,
you would select the text and use `!fold -w80 -s`.

Magic!
