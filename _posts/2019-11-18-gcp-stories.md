---
author: Raymond
layout: post
date: 2009-11-19T21:21:09-05:00
title: GCP Stories
categories:
  - Tech
---
I've been working on GCP lately. Having a lot of fun, learning stuffs and running into problems.
<img class="alignnone size-full wp-image-1653" src="/wp-content/uploads/2019/09//11/google-cloud-logo-bigger.png" alt="gke" width="700" height="262" srcset="/wp-content/uploads/2019/09//11/google-cloud-logo-bigger.png 700w, /wp-content/uploads/2019/09//11/google-cloud-logo-bigger.png 300w" sizes="(max-width: 700px) 100vw, 700px" /> 
There are several bugs of GCP I have run into:
  1. I couldn't start several stopped VM instances one morning. It gave me an error code and asked me to contact Google support. But after a few minutes, it worked again.
  2. Extra files showed up in my Filestore share. They ended with ":zoneIdentifier" with the same name of existing files as the former part.
  3. Starting and stopping VM instances became much slower than usual.

Some things related to Load Balancer need to remember:
  1. HTTP has better performance than TCP in general. (Keep-alive helps)
  2. The timeout setting sometimes needs to be tuned so it can work with long requrests.

About Terraform:
  1. Separate files into different logical units.
  2. Can not use underscore in many naming places. Try to stay with hyphen.

Others:
  1. When a request is failing, check firewall.
  2. On Windows VM, the Windows Firewal could be blocking.
  3. Startup script is helpful in preparint base image or running startup work.
