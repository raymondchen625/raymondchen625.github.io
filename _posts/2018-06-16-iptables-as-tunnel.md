---
id: 1551
title: Iptables as Tunnel
date: 2018-06-16T19:09:30+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1551
permalink: /2018/06/16/iptables-as-tunnel/
timeline_notification:
  - "1529190574"
categories:
  - Tech
---
My requirement is: I have a service listening to 127.0.0.1:4040 only, I want to make it accessible on port 4040 of any IP address, not just 127.0.0.1, so other host in the LAN can access it.

I usually use SSH Tunnel (with **-L**) to achieve this. It&#8217;s easy and works on most scenarios, as long as I have access to a SSH shell, even when iptables is not installed. But I decided to practice my knowledge by using iptables.

My thought was to DNAT . It&#8217;s basically correct, only I got stuck:

<pre>iptables -t nat -A OUTPUT -p tcp -d 192.168.1.0/24 --dport 4040 -j DNAT --to-destination 127.0.0.1:4040
</pre>

It doesn&#8217;t work. Googling shows I need to set the route_local to 1:

<pre>sysctl -w net.ipv4.conf.eth0.route_localnet=1
sysctl -p</pre>

It&#8217;s due to security reason. Like ip_forward, we have to explicitly change the kernel setting to allow traffic forwarded to loopback interface.

Plus this is required for accessing from the host itself:

<pre>iptables -t nat -A OUTPUT -p tcp --dport $port -j DNAT --to 127.0.0.1:4040</pre>

For the scenario of forwarding to another host, I also put notes here (Listening to 80 on 192.168.2.16 (host with the iptables rules) and forwarding the traffic to 192.168.2.10:8080) :

<pre>iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.2.10:8080
iptables -t nat -A POSTROUTING -p tcp -d 192.168.2.10 --dport 8080 -j SNAT --to-source 192.168.2.16
</pre>

&nbsp;

&nbsp;