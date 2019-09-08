---
id: 1507
title: Trying Eclipse Che
date: 2018-01-07T14:18:35+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1507
permalink: /2018/01/07/trying-eclipse-che/
geo_public:
  - "0"
categories:
  - Tech
---
Eclipse Che has been out for a long time. Its current version is 5.0.

I think it&#8217;s a good idea to move more stuffs to the cloud to achieve less friction in dev and qa. I tried Che a couple times, always got horrible user experience due to my slow machine, plus the IDE in browser causing loss of familiar shortcut keys and a whole new UI to get familiar with&#8230; But I believe those are actually non-issues. It&#8217;s not supposed to run in local, and the IDE issue can be improved, probably not by Che itself. The idea of Language Server Protocol seems amazing to me. So I decided to give it another shot in the weekend.

<img class="alignnone size-full wp-image-1511" src="http://localhost/wp-content/uploads/2018/01/eclipse-che.png" alt="Eclipse Che" width="1600" height="1035" srcset="http://localhost/wp-content/uploads/2018/01/eclipse-che.png 1600w, http://localhost/wp-content/uploads/2018/01/eclipse-che-300x194.png 300w, http://localhost/wp-content/uploads/2018/01/eclipse-che-768x497.png 768w, http://localhost/wp-content/uploads/2018/01/eclipse-che-1024x662.png 1024w, http://localhost/wp-content/uploads/2018/01/eclipse-che-1568x1014.png 1568w" sizes="(max-width: 1600px) 100vw, 1600px" /> What I&#8217;ve done and notes:

✔️ Run an Eclipse Che instance on my GCE instance

<p style="padding-left:30px;">
  Got stuck at the beginning when I deployed it on GCE. What worked in my local didn&#8217;t work on GCE. As soon as I opened the Chrome Developer Tools window, things became more obvious. Make sure:
</p>

<p style="padding-left:30px;">
  1) Ports should not be blocked on the VM. It requires a lot of other ports to work;
</p>

<p style="padding-left:30px;">
  2) Host name of the Che server, which will be used on the web page to find the correct agent to talk to, need to be configured correctly when firing up the Che docker container. Basically provide the right hostname or IP address to CHE_DOCKER_IP_EXTERNAL environment variable. Che can not always figure out which IP to bind to correctly so there is a chance it binds to an internal IP on the cloud. When we access from browser, we will see strange errors, but we can find out what&#8217;s going on if we look at the network requests.
</p>

✅ Run official &#8216;petclinic&#8217; tutorial

<p style="padding-left:30px;">
  Running it is easy if environment is set up correctly. What interested me was how the 2 containers discover/talk to each other and how the data is populated.
</p>

<p style="padding-left:30px;">
  Data population is simply a Spring thing so once we locate the config file, it&#8217;s just there. I think the dev machine find the db machine by hostname, which is &#8216;db&#8217; in this case. It should be similar to docker link, but I didn&#8217;t see the entry in hosts file so I figured it was a DNS service. Didn&#8217;t look under the hood.
</p>

✔️ Connect with one of my own Java/MySQL project on BitBucket

<p style="padding-left:30px;">
  I need to add the Che workspace&#8217;s generated public key to the key list in BitBucket(or Github). Maybe we can do it the other way but I have to upload my private key and input or save password when it accesses my Git repo. Better add the generated pub key to BitBucket.
</p>

✅ Run unit tests and SpringBoot app in the IDE

<p style="padding-left:30px;">
  As mentioned above, the dev machine finds the db by hostname. I needed to set the db machine name, and the db credentials, to the one I defined in the project. Once this is done, they can talk. I didn&#8217;t add the data population, which was out of scope. But unit tests and app ran well after I populated the data manually.
</p>

In the feature list it seems to be able to mount the project through SSH so I can have a mirrored local workspace. That sounds cool. I might try it when I have time to play with it again.

GCE is really cool btw. I upgraded my minor instance to &#8220;n1-standard-2 (2 vCPUs, 7.5 GB memory)&#8221;, it worked pretty well. Once I&#8217;m done with this resource-demanding task, I can switch back to the minor.