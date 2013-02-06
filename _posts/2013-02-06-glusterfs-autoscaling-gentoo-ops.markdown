---
layout: post
title: Super Awesome Git Deployment Across Autoscaling Groups with GlusterFS
description: In which I discuss ops fun and making deplotment so easy.
permalink: /2012/02/glusterfs-autoscaling-gentoo-ops/index.html
categories:
- ops
- gentoo
- git
- deployment
- workflow
---

The cool thing about having a never-ending side project is that you get to do
all kinds of crazy, fun things that might be outside the normal bounds of your
day-to-day work. In my case, I'm rebuilding a super old text-based RPG, which
leaves a ton of opportunity to learn new things. So recently, tired of FTPing
updates to a handful of micro EC2 instances, and with a new application on the
way, we decided to improve deployment.

## Background

We're working on a browser text-based RPG that we took
over in 2005, originally from late 2001. It's a PHP 3 app with frames, a relic
of the glorious days before Ajax was really a *thing*. We deployed either
through FTP or a series of bash-scripted `scp`s. We moved our hosting to EC2
a couple years ago, but we were still managing servers-- a couple micro
web servers, plus a larger database instance-- manually. To upgrade packages,
we'd have to look up the DNS, ssh in, and update. There was no autoscaling, so
we'd just run between 3 and 6 instances and scale up or down manually. It 
*worked*, but it was super annoying to get anything done.

We're also rebuilding the entire game under a brand-new stack. I'll talk about
that architecture in a later post, but it's all service-based, with super fast
Lua / Nginx servers handling data and exposing an API and some Node servers
serving assets and handling browser web requests. This new stack will require
running a lot of discete services, so time spent in ops has the likelihood of
exponential complexity.

## Requirements

* Easy deployment. Preferrably, git deployment. We would need both file
  transfer and a mechanism for reloading services. Something like Heroku's
  deployment.
* Easy authentication, preferrably with ssh keys. LDAP would be cool.
* Easy ops. Move passwords and config into environment variables, and have a
  way to update these across all the servers.
* Availability / durability. No single point of failure, and the option for
  multi-zone and eventually multi-region scaling.

## Server Setup

We ended up using Gentoo for our servers, because even if it's a little
*weird*, it's __really fast__. Everything is compiled right for the hardware.
Additionally, to make __ops__ easy, we can build our own portage tree and
update and install our applications through our own package manager. We can
also mask releases (mark some as dangerous) until testing passes, giving the
option to run beta / test / non-passing versions of an application if it's
useful, such as in test images.

We built base images for test and prod for each application, and [set up
autoscaling groups](http://www.cardinalpath.com/autoscaling-your-website-with-amazon-web-services-part-2/).

## Deployment and Security

Next, we set up LDAP on a server so that we could quickly SSH into servers.
Gentoo's great about LDAP, so that didn't take terribly long. That allowed us
to use things like `ssh jlawson@files.application.com` (previously, we shared
the EC2 pems on Google Docs. Don't look at me like that, I know, it's
awful.)

We knew we wanted git deployment, but copying an application's files to multiple
servers *feels bad man*, and would be worse with a ton of servers. That decision 
made, we built a file server using
[GlusterFS](http://www.gluster.org/). *Note: we had to unmask and use GlusterFS 3.3
in order to get multiple groups working with LDAP.* We mounted GlusterFS onto the
filesystem on our application AMI, and installed git on the GlusterFS file server.
This allowed us to deploy using our ssh credentials stored in LDAP to the file
server hosting git, and because the application servers had the file server
mounted like a real directory, we would instantly have all application changes
without having to push to each individual application server. It was awesome to
`git remote add test jlawson@files.application.com/files/test` and
`git remote add production jlawson@files.application.com/files/production`. This
meant that deploys to update to *all of the servers* was as easy as
`git push test master` or `git push origin master`. Goodbye, FTP. Goodbye,
complicated bash scripts.

All services for a single application deploy to the same GlusterFS file server. 
This allows any application server to load up whatever app it needs to run.

We can also cluster GlusterFS into multiple zones and, eventually, regions for
durability and performance using a round-robin DNS scheme.

## Server Configuration

We made a git repository for server configs and git hooks for the application.
In it was an app configuration file that set environment variables, like

```
APP_ENV=prod
```

It also contained additional `test` and `prod` folders with appropriate
configurations, so we could then load up the proper environment variables
per environment. This was loaded onto GlusterFS and symlinked into
`/etc/conf.d`. We determined that having each application control its own
config made things a whole lot simpler than trying to load 1-n configs on a
server-by-server basis. The whole point of this system is automating ops, so we
can spend time building stuff; the less we're `ssh`ed, the better.

We also put our post-receive git hooks in this repository, which symlinks the
git deploy for the application to read and notifies our Hubot of deploys in
chat.

## Next Steps

We want to introduce a daemon in each application server that watches for
changes in the files, in case we're running applications that need to be
restarted. We're thinking of introducing a common format, such as requiring
Makefiles in our applications that expose `install`, `restart`, `stop`, and
`start` so that we can, for example, reboot Nginx or restart Node or whatever.
We like the idea of a common makefile so that applications can manage themselves, and
the server does not have to be aware of what's running on it.

We want to open-source our git hooks and autoscaling configuration scripts
at some point soonish.

We also want to set up a constantly-running instance that always builds the
latest Gentoo packages and builds our applications on a custom portage tree so
that we can be up-to-date on both external system packages (like nginx) and our
own applications.

## Conclusion

All of our ops are automated really well now; Amazon automatically scales
groups up and down, deploying changes (or reverting changes) is a git push and
doesn't get in the way of our workflow, and we can submit no-downtime sub-second
application deploys.
