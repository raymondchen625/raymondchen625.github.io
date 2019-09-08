---
id: 1527
title: Out of Memory on Linux
date: 2018-04-21T22:11:23+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1527
permalink: /2018/04/21/out-of-memory-on-linux/
timeline_notification:
  - "1524363088"
categories:
  - Tech
---
I&#8217;ve been working on optimizing parameters for Kubernetes pods for testing environment VMs lately.

When I deploy a certain number of pods, the system hangs. The root cause is out of memory, or more directly many pods have their JVM heap size set to big or unset.

JVM has strange behaviours on Docker Container: ([Source](https://blogs.oracle.com/java-platform-group/java-se-support-for-docker-cpu-and-memory-limits)) Simply put, if we let it unset, it only sees the RAM amount of the host and tries to initialize with a heap 1/4 of it. Once I turn down the max heap size of each pod explicitly, the VM can run a lot more pods at the same time.

I also found interesting things along the investigation. My test environment has swap turned on, which is not recommended in production. I can observe when I overdeploy the pods, there are a lot of swapping activity. The kswapd0 process takes a large trunk of CPU and the system becomes sluggish.

I tried to turn off the swap and redeployed, which is not a solution by the way, just for experiment. The system hung, not even sluggish this time.

Wondering what had happened, I did it again with swap on first, turn it off with all pods running. The swapoff command got killed. I could see a lot of state=D process showing up in a monitoring window checking the process like image below:

<img class="alignnone size-full wp-image-1528" src="http://localhost/wp-content/uploads/2018/04/swapoff.png" alt="SwapOff" width="1847" height="860" srcset="http://localhost/wp-content/uploads/2018/04/swapoff.png 1847w, http://localhost/wp-content/uploads/2018/04/swapoff-300x140.png 300w, http://localhost/wp-content/uploads/2018/04/swapoff-768x358.png 768w, http://localhost/wp-content/uploads/2018/04/swapoff-1024x477.png 1024w, http://localhost/wp-content/uploads/2018/04/swapoff-1568x730.png 1568w" sizes="(max-width: 1847px) 100vw, 1847px" /> 

So my conclusion is: when swap is off, if the pod can not get the RAM it is allocated when it started up, it enters the D (uninterruptible sleep) state and takes a lot of CPU. Nothing the system can do since there is no swap to turn to. So the system hangs. If we try to turn off swap when the system is already using a lot of swap to handle those pods, it will get a lot of processes to D state like shown in the screen capture. Once the swapoff command is killed due to failure, the system can use swap again, the &#8216;D&#8217;-state process will eventually disappear(get into other states).

A nice post on D state is [here](https://stackoverflow.com/questions/223644/what-is-an-uninterruptable-process).