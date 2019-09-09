---
id: 859
title: Powerline Ethernet
date: 2014-01-21T21:57:43+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=859
permalink: /2014/01/21/powerline-ethernet/
original_post_id:
  - "859"
  - "1083"
categories:
  - Tech
---
Powerline ethernet is not as stable/mature as I expected. I bought the [LEA](http://www.amazon.ca/gp/product/B003K12EZ0/ref=ox_ya_os_product) because it is cheaper than other brands, which might be the source of the problem.

[<img class="alignleft size-full wp-image-861" alt="powerline ethernet adapters" src="http://www.raymondchen.com/wp-content/uploads/2014/01/powerline-ethernet-adapters.jpg" width="300" height="300" />](http://www.raymondchen.com/wp-content/uploads/2014/01/powerline-ethernet-adapters.jpg)

When I got it, I used it to connect the used server in my basement to my router in 2nd-floor living room. When I tested with my MacBook the first time, the whole installation is a breeze, just plug and play.

But I bumped into some weird problem of my server later. When everything looked fine, but I just couldn&#8217;t ping other device in the same LAN. I thought it could be the Linux driver problem of the NIC, or the powerline adapter was loose from the receptacle. It kept switching between connection and disconnection for sometime until I finally figured out why it was so: I noticed when I switch on my monitor, the network went down. It went up again after I turned off the monitor again!

When the receptacle being used, or near, by the powerline adapter has too many deviced connected, the network goes down. It might have something to do with the electronic interference. Once I find the reason, the solution is simple, put the adapter in separate receptacle and use a longer network cable.