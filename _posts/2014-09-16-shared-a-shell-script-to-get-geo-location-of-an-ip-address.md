---
id: 1045
title: '[Shared] A shell script to get geo location of an IP address'
date: 2014-09-16T14:48:09+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=1045
permalink: /2014/09/16/shared-a-shell-script-to-get-geo-location-of-an-ip-address/
original_post_id:
  - "1045"
  - "1083"
categories:
  - Tech
---
> #!/bin/bash  
> locmessage=\`curl -s ipinfo.io/$1\`  
> city=\`echo &#8220;$locmessage&#8221; | grep city | awk &#8216;{print $2}&#8217; | sed &#8216;s/[s&#8221;,]//g&#8217;\`  
> region=\`echo &#8220;$locmessage&#8221; | grep region | awk &#8216;{print $2}&#8217; | sed &#8216;s/[s&#8221;,]//g&#8217;\`  
> country=\`echo &#8220;$locmessage&#8221; | grep country | awk &#8216;{print $2}&#8217; | sed &#8216;s/[s&#8221;,]//g&#8217;\`  
> org=\`echo &#8220;$locmessage&#8221; | grep org | awk &#8216;{$1=&#8221;&#8221;; print $0}&#8217; | sed &#8216;s/[s&#8221;,]//g&#8217;\`  
> ip=\`echo &#8220;$locmessage&#8221; | grep ip | awk &#8216;{print $2}&#8217; | sed &#8216;s/[s&#8221;,]//g&#8217;\`  
> echo &#8220;$ip($org[$city/$region/$country])&#8221;

&nbsp;