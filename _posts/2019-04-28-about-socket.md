---
id: 1627
title: About socket
date: 2019-04-28T10:19:42+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1627
permalink: /2019/04/28/about-socket/
timeline_notification:
  - "1556461183"
categories:
  - Tech
---
Most if not all of my applications I have worked on were about HTTP, which is a stateless protocol. Until lately we started to work on a feature with websocket.

I ran some tests to help me understand how the connection is opened and closed. I deploy several websocket services in containers. I use a wscat tool to access them.

Some observations:

  1. The connection will keep open even I don&#8217;t send any data in hours. That depends on the implementation of the nodes between them. Say if we have routers between them, some routers might decide to drop the session in 1 idle hour. That&#8217;s why we usually need keepalive to keep it going.
  2. When I kill the client process or the server process, the connection will be closed shortly. That&#8217;s due to the mechanism when a Linux process receives a terminate signal (KILL), it tries to close the file descriptors and TCP connections as cleanup.
  3. If I terminate some service between them, like the &#8216;flanneld&#8217; in my Kubernetes service, which also brings down the docker service, the connection is in fact closed. But the client won&#8217;t notice it until it tries to send data again. That&#8217;s due to when I do that, it&#8217;s like powering off some routers, the Linux process termination mechanism has no chance to kick in.

Conclusions:

  1. Sockets try their best to clean up on many occasions. But keep alive is still needed
  2. The network nodes between the two parties have impacts on the behaviors. Some might keep a shorter session. Some might not be able to clean up when they go out of service.