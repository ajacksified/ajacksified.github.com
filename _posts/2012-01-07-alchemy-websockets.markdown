---
layout: post
title: Alchemy Websockets
description: In which I talk about our C# websocket library
permalink: /2012/01/alchemy-websockets/index.html
categories:
- css
- meta
---

[Alchemy Websockets](http://www.alchemywebsockets.net) is a C# .NET library
that allows a web browser that supports the WebSockets protocol to connect
to a server application using a peristant TCP connection. This means that we
can employ real-time, stateful communication between web applications and server
applications. WebSockets are supported by current versions of Chrome, FireFox,
Safari, and Opera and Opera Mobile with special configuration settings set.
IE10 will also have WebSockets. Alchemy can run on Mono (which means EC2 is a go)
and supports most versions of the protocol, stretching from hybi-00 to the
latest (and final) RFC spec.

We (Drew and I and [Olivine Labs](http://www.olivinelabs.com)) had started
considering how to use WebSockets for some of the games we were building and
planning. At the time, we were thinking about using them for chat for the
now-defunct [Chrysellia](https://github.com/Olivine-Labs/Chrysellia); and we
have plans to build a game that uses WebSockets fully as its transport. Our
search took us first to [socket.io](http://socket.io), a Node.js implementation
that gave not only WebSockets, but also fallbacks with Comet and Ajax
long-polling and other hackish methods of doing socket-like communication.

However, while I, a javascript developer, originally lobbied for Node.js and Socket.io,
we decided to move forward with C#. It isn't the new, sexy language, but it is
a language that we both know well; and better, we could take advantage of C#'s
amazing microthreading. We built the library around this core threading model so that
it could *very* efficiently handle hundreds of thousands of simultaneous connects,
disconnects, and messages on a single server. Drew was able to get around 200k
live connections on his dev box before he ran out of memory for the *clients*.

Alchemy also includes a WebSocket client for server intercommunication that uses the
latest protocol version [rfc6455](http://tools.ietf.org/html/rfc6455) to talk
to anything else running WebSockets (whether Alchemy or otherwise.)

We'll be putting it up to a big test with a server infrastructure management API
that we've called Tiamat and an API command router called Vocale that will be
built over the coming months (we plan to MIT / LGPL these as well).
