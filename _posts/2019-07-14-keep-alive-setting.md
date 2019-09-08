---
id: 1647
title: Keep-alive Setting
date: 2019-07-14T18:26:44+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1647
permalink: /2019/07/14/keep-alive-setting/
timeline_notification:
  - "1563143204"
categories:
  - Tech
---
We started to see intermittent 502 errors since we upgraded Nginx-ingress-controller to a certain version. After investigation, we found the root cause is related to keep-alive.

Kepp-alive started to take effect since Nginx Ingress Controller 0.20.0. There is keep-alive timeout setting on both the Nginx(default=60s) and Tomcat(default=20s) sides. With both set to default, it&#8217;s always the Tomcat who closes the TCP connection, since 20<60. Depending on the timing, if Nginx sees the connection is open when it checks, but Tomcat closes it before Nginx can actually send data, Nginx will run into an error, returning a 502 Bad Gateway response to its client.

The solution is straightforward, always setting the Nginx timeout shorter than Tomcat&#8217;s.