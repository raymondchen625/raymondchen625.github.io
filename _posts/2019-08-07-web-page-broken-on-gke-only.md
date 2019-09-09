---
id: 1651
title: Web Page Broken on GKE only?
date: 2019-08-07T08:37:26+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1651
permalink: /2019/08/07/web-page-broken-on-gke-only/
timeline_notification:
  - "1565181449"
categories:
  - Tech
---
Everybody was confused when we found a webpage running in Tomcat in a Docker container was broken only on GKE(Google Kubernetes Engine). It worked well in all other environments, including Kubernetes built on GCE(Google Compute Engine) VMs(OS: CentOS 7)or local Oracle VirtualBox VMs, Docker engine on macOS or standalone Tomcat outside of docker container. Shouldn&#8217;t container be OS-agnostic? How could it affect a web page, which is the least likely to be affected?

There are a few things going on:

  1. The web page is rendered by [MyFaces](https://myfaces.apache.org/). There are multiple elements on the JSF page with &#8216;rendered&#8217; attribute. When it works correctly, only one of the element will render. Those conditions can&#8217;t be true at the same time;
  2. We have a myfaces-shaded jar file on our classpath, which is a duplicate jar of other &#8216;real&#8217; MyFaces implementation. If myfaces-shaded is loaded, it will throw some errors and enters an incorrect state. In this state, the &#8216;rendered&#8217; attribute will be ignored on #1 page, which causes all elements to render. This is the direct cause of the broken web page;
  3. In our &#8216;working&#8217; environments other than GKE, the myfaces-shaded never gets picked up by the classloader, which is why we never discovered this problem before. But we did have developers find this from time to time. It disappeared after restarting Tomcat. So nobody has actually followed this through;
  4. In GKE, the myfaces-shaded gets picked up by the classloader for some reason. Once we removed the myfaces-shaded in the classpath, it started to work.

<img class="alignnone size-full wp-image-1653" src="/wp-content/uploads/2019/08/gke.png" alt="gke" width="700" height="262" srcset="/wp-content/uploads/2019/08/gke.png 700w, /wp-content/uploads/2019/08/gke-300x112.png 300w" sizes="(max-width: 700px) 100vw, 700px" /> 

I suspect the GKE&#8217;s host OS, Container-Optimized OS from Google, has some optimization on the file system behaviour, which causes the order change when JVM classloader loads a list of JAR files. Imagine the JVM asks for a list of inodes with a file path list, the OS decides to optimize the response time, trying to minimize the mechanical movement. Hopefully, all data can be retrieved in one disk magnetic head trip. But the file distribution on the disk could be widely spreaded. If the head doesn&#8217;t want to go back and forth, the file order it picks up couldn&#8217;t be the same as in the API call parameter. I think this might not be an issue if the API clearly states the order is not guaranteed. And if there is an asynchronous mechanism, the JVM process might have already started to get callbacks and load jar files as the head moves across the disc radius.

Nevertheless, an application should never have 2 same classes with different versions on the classpath. The loading order, which shouldn&#8217;t be relied on in the first place, in other environments hides this bug by accident.
