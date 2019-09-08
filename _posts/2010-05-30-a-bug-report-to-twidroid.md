---
id: 186
title: A Bug Report to Twidroid
date: 2010-05-30T10:58:04+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=186
permalink: /2010/05/30/a-bug-report-to-twidroid/
original_post_id:
  - "186"
  - "1083"
categories:
  - Tech
  - Web
---
Hi,

I&#8217;m located in China, where Twitter is blocked by the government. So when I use Twidroid, I&#8217;ve to specify an custom API, a twip API on my own server in US, rather than using the default official one. Everything is fine except for the List part. In the home screen, open the menu and choose &#8220;view list&#8221;, nothing will show. I tried many times and finally figure out how to work around it. I connect my phone to an openvpn, which enables me to connect to twitter.com. Then I add another account, set it to the default type. I could load my lists then. And I delete the official site account, disconnect my vpn and switch back to my custom api account. The lists are still shown, which are cached I guess. From now on I can access the timelines of those lists smoothly. What I should not do is trying to refresh the list of my lists, which could empty everything.  
My custom API is ok, many other software are using it, the list function works fine. I guess the bug is from the api call of loading the list of my twitter lists. Maybe that call is not through the custom API, but still be directed to the official twitter.com. Then it&#8217;s blocked by the firewall of Chinese government. Please look into this. Thank you very much.

Best regards,

Raymond