---
layout: post
title: Vim "Can't Open Errorfile" Issue
description: In which I figure out a scarily obscure vim error
permalink: /2011/12/vim-cant-open-errofile/index.html
categories:
- vim
---

tl;dr: put `set shell=bash` in your .vimrc

I recently had to reinstall OS X after a catostrophic hard drive failure. I had
most of my configuration [on Github](github.com/ajacksified/Config) (which is
something I *highly* recommend), luckily, so it was a pretty easy move thanks
to that and some well-placed backups.

So, I pulled down my config, loaded the submodules, and threw it all in my
~/.vim directory. Everything went smooth as butter until I tried to run vim-ack.
I had originally thought that it was an error with vim-ack, and tried alternate
versions of vim-ack. But I still got an error. So I tried five different
snapshots of MacVim. I still got the error. Then I tried copying a file with
NERDTree, and I got *another* error. Mystifying.

vim-ack error:

    Searching ...
    Error detected while processing function <SNR>21_Ack:
    line   23:
    E40: Can't open errorfile /var/folders/98/_ymf2wy554n6qrs32ptrwrl40000gn/T/vx96wXC/2
    Press ENTER or type command to continue

NERDTree error:

    Cannot execute shell /usr/local/bin/bash
    Error detected while processing function <SNR>19_showMenu..30..47..NERDTreeCopyNode..54..112:
    line    8:
    E484: Can't open file /var/folders/98/_ymf2wy554n6qrs32ptrwrl40000gn/T/vx96wXC/3
    Error detected while processing function <SNR>19_showMenu..30..47:
    line    6:
    E171: Missing :endif
    Error detected while processing function <SNR>19_showMenu..30:
    line   19:
    E171: Missing :endif

I tried chowning the temp files; I tried unsetting TEMPDIR. Nothing worked.

After browsing page after page of non-helpful Google results, I came back to
a [three-year-old conversation](http://vim.1045645.n5.nabble.com/E40-quot-Can-t-open-errorfile-quot-td1217809.html) 
that I had already been to twice. I noticed one little line in there:

    set shell=bash

And thought "Well, I've tried everything else."

It worked.
