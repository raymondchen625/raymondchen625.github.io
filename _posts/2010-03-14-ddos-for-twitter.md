---
id: 110
title: DDoS for Twitter
date: 2010-03-14T11:55:38+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=110
permalink: /2010/03/14/ddos-for-twitter/
original_post_id:
  - "110"
  - "1083"
categories:
  - Tech
tags:
  - Twitter
---
We were discussing about some design problems in an IM group, I was suddentlly hit by a thought of GFW attacking Twitter. GKZhong later named it DDoS for Twitter.

Here is the way: Gather 100K people whom GFW can control, which is pretty easy for Chinese government. Let them follow each other. That will be a huge dense network. All of them are popular people. Either one post a new tweet, it will be written to all other 99999 people&#8217;s timelines. To attack: let them tweet simultaneouslly.

I don&#8217;t have the exact architecture diagram of Twitter. So I&#8217;ve to guess. The section which will take the heat will be the section of updating the timelines. Though the website doesn&#8217;t have to go down, but if the updating section is down, people will see no update any more.

How Twitter do the timeline update job is important. I&#8217;ve read some articles telling that they do distinguish popular people from others, meaning people having many followers are processed differently. Anyway, the point is: there is always a limit. Twitter may have quite some popular people now. But they don&#8217;t construct a dense network by following each other. So the architecture or hardware might be sufficient now, but not necessarily enough to handle such deliberate attack.

Twitter might need some test to see how severe the result will be if this attack really happens. Changing the rule may be necessary: you can have unlimited followers, but you can only follow up to 5,000 or 10,000 people. Nobody needs to follow unlimited people, which is not different to checking the public timeline.