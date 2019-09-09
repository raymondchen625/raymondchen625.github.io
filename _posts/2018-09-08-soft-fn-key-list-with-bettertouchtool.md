---
id: 1580
title: Soft Fn key list with BetterTouchTool
date: 2018-09-08T10:48:16+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1580
permalink: /2018/09/08/soft-fn-key-list-with-bettertouchtool/
timeline_notification:
  - "1536418100"
categories:
  - Gadget
  - Tech
  - Uncategorized
---
Finally found a way to define a soft Fn key list in [BetterTouchTool](https://folivora.ai/).

Define a Group &#8216;Fn&#8217; and add touch buttons like these:

<img class="alignnone size-full wp-image-1577" src="http://localhost/wp-content/uploads/2018/09/fn-group.png" alt="Fn-group" width="1622" height="794" srcset="http://localhost/wp-content/uploads/2018/09/fn-group.png 1622w, http://localhost/wp-content/uploads/2018/09/fn-group-300x147.png 300w, http://localhost/wp-content/uploads/2018/09/fn-group-768x376.png 768w, http://localhost/wp-content/uploads/2018/09/fn-group-1024x501.png 1024w, http://localhost/wp-content/uploads/2018/09/fn-group-1568x768.png 1568w" sizes="(max-width: 1622px) 100vw, 1622px" /> 

For each button, assign it to Apple Script like this:

> <p class="p1">
>   <span class="s1"><b>tell </b></span><span class="s2"><i>application</i></span><span class="s1"> &#8220;System Events&#8221;</span>
> </p>
> 
> <p class="p2">
>   <span class="s1"><b>  key code</b></span><span class="s3"> 120 </span><span class="s4">&#8212; F2</span>
> </p>
> 
> <p class="p1">
>   <span class="s1"><b>end </b><b>tell</b></span>
> </p>

It looks like this when collapsed:

<img class="alignnone size-full wp-image-1582" src="http://localhost/wp-content/uploads/2018/09/fn-touch-bar.png" alt="Fn-touch-bar" width="2170" height="60" srcset="http://localhost/wp-content/uploads/2018/09/fn-touch-bar.png 2170w, http://localhost/wp-content/uploads/2018/09/fn-touch-bar-300x8.png 300w, http://localhost/wp-content/uploads/2018/09/fn-touch-bar-768x21.png 768w, http://localhost/wp-content/uploads/2018/09/fn-touch-bar-1024x28.png 1024w, http://localhost/wp-content/uploads/2018/09/fn-touch-bar-1568x43.png 1568w" sizes="(max-width: 2170px) 100vw, 2170px" /> 

Expanded:

<img class="alignnone size-full wp-image-1579" src="http://localhost/wp-content/uploads/2018/09/fn-buttons.png" alt="Fn-buttons" width="2170" height="60" srcset="http://localhost/wp-content/uploads/2018/09/fn-buttons.png 2170w, http://localhost/wp-content/uploads/2018/09/fn-buttons-300x8.png 300w, http://localhost/wp-content/uploads/2018/09/fn-buttons-768x21.png 768w, http://localhost/wp-content/uploads/2018/09/fn-buttons-1024x28.png 1024w, http://localhost/wp-content/uploads/2018/09/fn-buttons-1568x43.png 1568w" sizes="(max-width: 2170px) 100vw, 2170px" /> 

&nbsp;

&nbsp;