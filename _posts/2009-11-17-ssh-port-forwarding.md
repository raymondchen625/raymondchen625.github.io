---
id: 10
title: SSH Port Forwarding
date: 2009-11-17T15:51:11+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=10
permalink: /2009/11/17/ssh-port-forwarding/
original_post_id:
  - "10"
  - "1083"
categories:
  - Tech
  - Uncategorized
tags:
  - Linux
---
Just finished a [reading](http://www.ibm.com/developerworks/cn/linux/l-cn-sshforward/index.html?ca=drs-cn-1031) on SSH Port Forwarding.

It&#8217;s pretty clear. Not much left to say. Only one note:  
If you use it in Remote mode and want to bind ip addresses other than localhost, you must enable an option in the /etc/ssh/sshd_config of the ssh server machine:  
GatewayPorts yes

By default, sshd only allows ssh client to initiate service binding to loopback interface.