---
id: 1476
title: Hyper-V, VirtualBox and Vagrant
date: 2017-11-22T21:44:53+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1476
permalink: /2017/11/22/hyper-v-virtualbox-and-vagrant/
categories:
  - Tech
---
I have been working on Docker environment lately. And I got a new desktop which &#8220;upgraded&#8221; my operating system from Windows 7 to Windows 10. So I got the chance to try Hyper-V.

It didn&#8217;t work very well at first. I messed up the environment so much that  I couldn&#8217;t even reinstall Docker for Windows. It always gave me an error when I started it. But later I figured out that actually Hyper-V,, which is a hypervisor virtualization, is not like VirtualBox. When Hyper-V is enabled, virtual device and physical device are all at the same level. My problem was the docker machine somehow created many virtual NIC devices, most of them are in invalid states, with exclamation mark in device manager. Once I removed them, things started to work again.

But the folder sharing between Docker Machine is awkward, which requires admin privilege. I later switched to a pure Ubuntu VM in Hyper-V. I shared the folder by using Samba share on the VM. I didn&#8217;t use most of the Docker for Windows features except for the docker client. I connect it to the Docker engine in the VM listening to a TCP port.

I had been with Hyper-V VM for a while until I decided to switch to VirtualBox. The main reason is cross-platform. I did write a Vagrant file for setting up the Hyper-V box. But it&#8217;s Windows 10 only. Working with Docker on Windows is already bad. Writing a Vagrant file for Hyper-V is too much.

Then I started to write Vagrant file for Docker running on a VirtualBox VM of Ubuntu 16.04 64-bit. Everything is fine. One interesting thing is the box is very slow when 7 containers are running on it. I realized that actually is because of VirtualBox is a full virtualization (Type 2 Hypbervisor), which means everything is simulated. I only assigned 2G memory to it. 7 JVM&#8217;s are too much for it. Hyper-V is a Type 1 hypervisor, which is supposed to have better performance. That means I need to assign more RAMs to my VirtualBox VM. As long as it&#8217;s running, I can not use them for other works.

Vagrant is cool. I can&#8217;t imagine how I can reset my environment dozens of times without it when I learn to set up Kubernetes cluster .