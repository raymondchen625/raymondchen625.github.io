---
title: SSH Blocked
date: 2019-12-22T09:05:25-05:00
author: Raymond
layout: post
categories:
  - Tech
---
A couple months ago I found I could no longer connect through SSH from my office to my Raspberry Pi at home on port 443 of my router. I was busy and kind of brushed it aside until today when I digged a little bit and found the reason. Here is what I did:

1. I got a 'connection reset' error when I connectted from office wifi. Then I switched to my iPhone hotspot, it worked. That means it has something to do with my company network environment;
2. I use the '-v' parameter to print debug message when I ssh, it was able to talk to the SSH service at the beginning but stopped at 'SSH2_MSG_KEXINIT sent' message. I googled it and found this didn't actually mean anything. I should check the server side logs;
3. I connect to the Raspberry Pi, and read the log file '/var/log/auth.log', and found this: 
```
Dec 22 08:51:12 raspberrypi sshd[4362]: Connection reset by a.b.c.d port 61304 [preauth]
```
I checked the IP address of 'a.b.c.d' and found it was actually the ISP's IP of my office network. Got it!

Conclusion: The ISP has a security policy to stop SSH connection to some endpoints. Obviously there is a whitelist since I can SSH to Google Cloud VMs without any problem. The security policy monitors the TCP traffic and resets it when the destination is not in the whitelist and it detects an SSH connection establishment keyword.

