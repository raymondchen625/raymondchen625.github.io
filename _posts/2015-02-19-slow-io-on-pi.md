---
id: 1136
title: Slow I/O on Pi
date: 2015-02-19T19:01:00+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1136
permalink: /2015/02/19/slow-io-on-pi/
sharing_disabled:
  - "1"
categories:
  - Uncategorized
---
When transmission is working, the I/O on my Pi is horribly slow. I checked it with hdparm, both external disks look fine:

/dev/sda:  
Timing cached reads: 256 MB in 2.01 seconds = 127.30 MB/sec  
Timing buffered disk reads: 68 MB in 3.09 seconds = 22.01 MB/sec

/dev/sdb:  
Timing cached reads: 252 MB in 2.01 seconds = 125.50 MB/sec  
Timing buffered disk reads: 68 MB in 3.08 seconds = 22.06 MB/sec

Need some more study on this…