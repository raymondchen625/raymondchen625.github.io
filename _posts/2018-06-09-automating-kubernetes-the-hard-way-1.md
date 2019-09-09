---
id: 1543
title: 'Automating Kubernetes the Hard Way #1'
date: 2018-06-09T17:05:46+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1543
permalink: /2018/06/09/automating-kubernetes-the-hard-way-1/
timeline_notification:
  - "1528578369"
categories:
  - Tech
---
I have been working on a script to set up Kubernetes in CentOS 7 environment. I had one before. But that one skips setting up secure internal communication so it doesn&#8217;t have to deal with the certificate generation. I wanted it to be closer to production. Comparing to kubeadm, it&#8217;s more transparent. I got to see what&#8217;s under the hood.  
It&#8217;s basically an automation of [Kubernetes the Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way). Tons of thanks to [Kelsey Hightower](https://twitter.com/kelseyhightower).

I&#8217;ll try to put down what I&#8217;ve learnt in this journey, probably in several blog posts.

Here are the  planned features of my script:

  * Run on CentOS 7 and RHEL 7.4+ hosts. Other Linux distribution should work too if proper modifications are made on the package installation part;
  * Main components:

<p style="padding-left:60px;">
  Kubernetes 1.9.8,
</p>

<p style="padding-left:60px;">
  Etcd: 3.2.1
</p>

<p style="padding-left:60px;">
  Flannel: 0.5.5
</p>

<p style="padding-left:60px;">
  Docker: 17.3.2
</p>

  * TLS enabled on all component communications with self-signed certificate
  * Node+RBAC mode for access control
  * All binaries included for installation in environment without access to Internet or a decent CentOS repository
  * Deployer can decide to put etcd, master or worker nodes together or separate them in any combination
  * Ease of installation: config and generate settings for all nodes in one place, copy the whole package to every node and run the same command, everything should be set up.

Source code on GitLab: <https://gitlab.com/raymondca/kubernetes-on-centos>

&nbsp;