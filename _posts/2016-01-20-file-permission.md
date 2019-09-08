---
id: 1251
title: File Permission
date: 2016-01-20T14:06:08+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1251
permalink: /2016/01/20/file-permission/
categories:
  - Tech
---
I tried to run a MySQL instance with Docker, while mounting its data folder to an folder under my Dropbox so I can reuse the data when I am in different locations. Make sense?

I refered to the [doc](https://hub.docker.com/_/mysql/). It failed to read create the MySQL data folders and files when starting up. But if I run it without -v, it started ok. I navigate to the folder of my host where the volume were mapped to, and found the owner was &#8220;vboxadd:daemon&#8221;.

So the solution: don&#8217;t map this vbox share to my normal user, add something like this in the rc.local: mount -t vboxsf -o umask=0022,uid=999,gid=1 Data /home/raymond/Data

&nbsp;