---
id: 404
title: Dabr Fixed
date: 2012-10-15T01:50:21+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=404
permalink: /2012/10/15/dabr-fixed/
original_post_id:
  - "404"
  - "1083"
categories:
  - Internet
  - Tech
---
It took me almost 5 days to fix the dabr problem on my vps.

At first, I thought it broke because I changed the permission setting the day before to enable the direct message function. So I spent quite some time fiddling with the app setting on Twitter side, in vain.

Then I suspected there was something wrong with the Dabr code. By downloading and installing newest code, it still didn&#8217;t work.

I did many Google search, of course. But I didn&#8217;t find any result until I found this: http://code.google.com/p/dabr/issues/detail?id=348

The reason is due to the deprecation of an old version api service. After I applied the new user.php file, it worked again!