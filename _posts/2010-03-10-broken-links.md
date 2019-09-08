---
id: 104
title: Broken Links
date: 2010-03-10T20:25:44+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=104
permalink: /2010/03/10/broken-links/
original_post_id:
  - "104"
  - "1083"
categories:
  - Tech
---
In a huge website, like portal, there are millions of web pages. They could reference each other by links, the <a href> tags.If somehow, some web pages need to be deleted, which happens now and then, those referencing pages will have broken links.

Whether this is an issue depends on how often it happens and company&#8217;s judgement. I see there are many cases that this is just ignored. After all, people only read updated news. The problem will go away as time goes by. But for some website like encyclopedia, e.g. Wikipedia,  broken links are unacceptable.

How shall we deal with it? Basically, there are two solutions. 1. Detect broken links periodically. 2. Keep the references so that you know who reference the page when you are to delete it.

Solution one looks bad, since it will be nightmare for huge website with millions of pages. It may take hours, even days, to finish a cycle. But it isn&#8217;t that bad in practical use as long as the website is reasonably partitioned. If you know exactly which small sections could be affected, and others definitely not, when you delete a page, you don&#8217;t need to scan all the webpages. It requires deliberate consideration while partitioning the system.

Solution two is easy to design, you just add a table or column in the database or something. Eachtime you add a &#8220;<a>&#8221; tag, you keep that reference, or you scan the page content and normalize all the links for saving each time the user submits a webpage. So when a page deletion is about to submit, user can be prompted which pages are to be affected and what actions are available.

Method two sounds good, only with an minor issue: User does not always know what to do while being asked. And he does not always have enough authorization to perform all the actions. Maybe the page is on different business unit, which the only thing he can do is to make a phone call, provide the information and wait. So this keeping the references feature is not as good as the desinger originally thinks. Of course, even in solution one, human intervention is also inevitable. There is the authorization problem, too.

With all the imperfection, I prefer the second solution, which is closer to a good one.  Adding some feature like asychronicity, notification, it can be really good.