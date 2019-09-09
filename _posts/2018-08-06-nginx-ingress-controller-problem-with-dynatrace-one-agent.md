---
id: 1567
title: Nginx-ingress-controller problem with Dynatrace One Agent
date: 2018-08-06T17:32:13+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1567
permalink: /2018/08/06/nginx-ingress-controller-problem-with-dynatrace-one-agent/
timeline_notification:
  - "1533591137"
categories:
  - Tech
---
A problem has been bothering me for days. Our performance team has 3 VMs to test our application running on Kubernetes. Everything works fine except the nginx-ingress-controller. I can destroy the k8s cluster and reinstall it many times. The weird thing is, sometimes it works on one node, but never all of them. I don&#8217;t understand the randomness.

We created a new VM in the same environment, which has no problem running anything. Since the 3 VMs were upgraded from RHEL 7.3 to 7.4, we spent so much time looking into that aspect, which turns out to be a red herring.

I found in those problematic containers, any nginx command will hang, even `nginx -v` It&#8217;s not likely a bug of nginx. I tried strace, which I was not familiar and got nothing. Finally I change the yaml of the nginx-ingress-controller to pure sleep, and run a `nginx -v` inside the container. When I compared with the good container the output of  `lsof -c nginx`, I found the Dynatrace One Agent in it. And I tried stopping it, bingo, all nginx came back to normal now. I was told there were nothing special running on those VMs. I shouldn&#8217;t have just believed it&#8230;

I think for some reason the tracing tool is holding the process when Nginx initializes itself. Got to be its settings. I&#8217;ll know soon.