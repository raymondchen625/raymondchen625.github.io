---
id: 443
title: VPS Blocked by GFW
date: 2013-01-17T18:13:20+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=443
permalink: /2013/01/17/vps-blocked-by-gfw/
original_post_id:
  - "443"
  - "1083"
categories:
  - Internet
  - Tech
---
My VPS, located in San Jose, could not be reached in last Friday. Website, SSH as well as ping didn&#8217;t work. And all of them worked fine if connected to an VPN. It looked like having been blocked by GFW.

But I&#8217;d done more digging. I did a traceroute on my Mac to it, which got me a strange result: the time out occured on the 16th hop, against usually on the first one or two hops. And the timeout occured on a router with an ip starting with &#8216;172&#8217;. That&#8217;s an IP range for LAN! I wondered if there was a misconfiguration on that router. I asked an friend, who is a SA. He suggested it was that router&#8217;s problem.

Only until the day after was I told by another SA that private IP address range showing up in tracert wasn&#8217;t necessary a problem. There could be some private network connections between routers, though not very often seen. Then I tried to trace the next IP address to mine one. That IP worked, with the 172.x.x.x IP address in the path.

Actually it was GFW&#8217;s work. [This article explains everything.](http://www.williamlong.info/archives/3342.html) The timeout happening in American router node was caused by the one-way block algorithm of GFW.