---
id: 173
title: Does DNSSEC work?
date: 2010-05-11T22:25:28+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=173
permalink: /2010/05/11/does-dnssec-work/
original_post_id:
  - "173"
  - "1083"
categories:
  - Tech
---
Does DNSSEC work? My answer is no, at least not yet.

Before going to look up more information about DNSSEC. I can simply give an example: In current setting, the ISP assigns user DNS servers dynamically. If it provides one with fake DNS records, the user&#8217;s computer has no way to find out it&#8217;s authentic or not. Unless all the operationg system or browsers get patched, simply do it in ICCAN can&#8217;t beat GFW.

Then I go to do some study.

When I come back, I still hold my point. Will Microsoft provide a security update which force Wiondows to check the signature of every DNS response? I hope so. But what if there is no signature at all? Wll it be treated as invalid response?