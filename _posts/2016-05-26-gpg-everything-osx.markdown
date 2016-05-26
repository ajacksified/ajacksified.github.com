---
layout: post
title: GPG
description: In which I finally get around to using a public key
permalink: /2016/05/gpg/index.html
categories:
- security
- osx
---

I finally got around to creating a public key, after Github
[announced signature support](https://github.com/blog/2144-gpg-signature-verification).
It's something I've been meaning to set up for a while (as a curiosity more than
as a threat mitigation strategy).

![A screenshot of GitHub saying the commit is verified.](/img/gpg-verified.png)

What I wanted was to automatically sign git commits (without too much hassle),
and an easy way to send encrypted messages, such as shared passwords, with
coworkers. It wasn't hard at all, although the information wasn't all in one
place; here's what I did:

1. `brew install gpg`. (Step 0, install [homebrew](http://brew.sh/), if you
  don't have it. If you don't, how are you even using OSX?
2. [Generate a gpg key](https://help.github.com/articles/generating-a-new-gpg-key/)
  and [add the key to your GitHub account](https://help.github.com/articles/adding-a-new-gpg-key-to-your-github-account/).
3. `brew install gpg-agent`. gpg-agent allows you
  cache your password entry for a configurable amount of time (so you're not,
  for example, re-entering your very strong private key password 12 times during
  a rebase).
4. Create a file at `~/.gnupg/gpg-agent.conf`, and add `use-standard-socket` and
  `default-cache-ttl 3600` on separate lines. The cache TTL is in seconds; an
  hour seems to be a good line between getting annoyed at entering too much and
  not opening myself up too much. Adjust as necessary.
5. Edit `~/.gnupg/gpg.conf` to uncomment the line `use-agent`.
6. Update your `~/.bashrc` or `~/.bash_profile` or whatever to add the
  following, which sets up the GPG daemon:
```
[ -f ~/.gpg-agent-info ] && source ~/.gpg-agent-info
if [ -S "${GPG_AGENT_INFO%%:*}" ]; then
  export GPG_AGENT_INFO
  export GPG_TTY=$(tty)
else
  eval $( gpg-agent --daemon --write-env-file ~/.gpg-agent-info )
fi
```

Finally, and optionally, you can update your git config to add signing by
default. First, get your uid from `gpg --list-keys | grep uid` which should
return something like:

```
uid                  Jack Lawson (jack) <myemail@gmail.com>
```

You can put this uid in your gitconfig:

```
[user]
  name = Jack Lawson <myemail@gmail.com>
  email = myemail@gmail.com
  signingkey = Jack Lawson (jack) <myemail@gmail.com>
```

Along with a line that tells git to automatically sign commits:

```
[commit]
  gpgsign = true
```

And, henceforth, all of your commits shall be signed, and you too can have that
beautiful, green [verified] badge.

Sidenote: I've started using [keybase.io](https://keybase.io/ajacksified) for
utilities like easily encrypting and verifying data with other users, and to
generate messages to verify my signature on Twitter, GitHub, this blog, and the
like.

[gpg signature](gpg/2016-05-26-everything-osx.txt)
