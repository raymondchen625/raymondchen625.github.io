---
id: 1622
title: Goroutine and channel
date: 2019-03-20T16:22:52+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1622
permalink: /2019/03/20/goroutine-and-channel/
timeline_notification:
  - "1553113375"
geo_public:
  - "0"
categories:
  - Tech
---
This is a mini retrospect of the fundamentals of goroutine and channel in Go after getting stuck in [an exercism task](https://exercism.io/my/solutions/e068e2c9b931409781e26e36c43382d7). The full official details can always be found in the [language spec](https://golang.org/ref/spec).

  * Channel is supposed to be shared between different goroutines (or your main and sub goroutines)
  * Usually at least one goroutine puts things in the channel, and at least one other consume things in it
  * When \`c:=range channelXxx\` is used, the loop will loop indefinitely until somebody closes the channel. That doesn&#8217;t mean you can not get out of the loop by other means, like &#8216;break&#8217;, &#8216;return&#8217;&#8230;
  * The main goroutine will exit without waiting for the completion of other goroutines, unless we use waitgroup, conditional loop or range-channel-loop in the main method to block its exit. And exit when some condition is changed.

Many of my difficulties were caused by my misunderstanding of these fundamentals, or my doubt on my understanding which was correct.