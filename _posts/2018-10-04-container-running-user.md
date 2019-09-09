---
id: 1593
title: Container Running User
date: 2018-10-04T19:58:45+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1593
permalink: /2018/10/04/container-running-user/
timeline_notification:
  - "1538697526"
categories:
  - Tech
---
Running a Docker container as root could raise a lot of red flags in security scanning. So we have been trying to make our Docker containers support running as non-root user lately.

The summary:

  1. Need to create a default user when we build the image in Dockerfile. use &#8216;USER username&#8217; to indicate by default it&#8217;s running as that user. Some scanning tools will check this
  2. In Kubernetes deployment YML file, add the securityContext section like below:

> securityContext:  
> &nbsp;&nbsp;runAsUser: 2000  
> &nbsp;&nbsp;fsGroup: 2001

This will override the &#8216;USER&#8217; in Docker image