---
id: 1645
title: About Ansible
date: 2019-07-08T21:07:31+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1645
permalink: /2019/07/08/about-ansible/
timeline_notification:
  - "1562634453"
categories:
  - Tech
---
The first time I used Ansible was 10 months ago when I did a research on OpenShift. The installation of OpenShift was through Ansible. But I only configured the Ansible script to install OpenShift, not really developed it. But I read a book on OpenShift.

Here came an opportunity when got a task to deploy our application on GCP. I spent some extra time to make tasks automated so I didn&#8217;t need to manually do them again when I had to repeat. It&#8217;s great to revisit Ansible. Here are some thoughts I have collected during the journey:

  * TerraForm and/or Ansible

I couldn&#8217;t determine how to use them together at first. There were different opinions. I eventually chose to only use Ansible because in my case my focus is application deployment. Provisioning is its first step, not at the centre of everything. But there could be other scenarios provisioning is at the centre, which justifies using TerraForm, or even have TerraForm drive Ansible script.

  * Staging VM

I chose a staging VM(GCE Instance). That instance is also created with Ansible script. The purpose of the staging VM is to run the remaining Ansible scripts to continue provisioning application VMs and deploying applications. The reason I don&#8217;t do this in my local is hostname to IP resolving(DNS). With the staging VM residing on the same subnet on GCE, it can simply talk to other hosts by their hostnames, which saves my work to probe and collect the IP addresses dynamically.

The main tasks the staging ansible script does:

  1. Create a small instance
  2. Install Ansible software
  3. Prepare SSH key so it can access other instances on the staging VM
  4. Copy the Ansible script onto the staging VM

  * Windows VM

Ansible has a full set of win_* module for manipulating Windows VM. To connect to the Windows VM, I need to manually enable the WinRM service first. To simplify the process I created a Windows image with WinRM already enabled and RDP password marked down.  So for application-specific instances, I create them from my custom image so I don&#8217;t have to do extra things to have my Ansible script access them.

  * Image usage

Apart from WinRM, I create some other images for the scenarios which I can&#8217;t automat or am not ready to automate yet. Like Oracle initialization, I manually install and configure it to a ready-to-use state, and create a disk image for it. Then next time I can create a new instance from that custom image. All I need to do is to populate data.

  * Tricks and tips 
      * Don&#8217;t forget the quotes when using {{variables}}
      * Facts and hostvars are your friends
      * Use -vvv to debug when needed
      * lineinfile/win_lininfile can be used to modify text files. But use them in descretion or it will be quite messy.
      * Some optimization methods can be applied if better performance is required, like fact caching, ssh multiplexing or simply run playbooks in parallel if that doesn&#8217;t affect the correctness