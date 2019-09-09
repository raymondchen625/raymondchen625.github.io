---
id: 1289
title: 'Range 2 Concatenated String [js]'
date: 2016-03-22T15:07:42+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1289
permalink: /2016/03/22/range-2-concatenated-string-js/
categories:
  - Uncategorized
---
[source]var k="1000000-1000008".split("-");  
console.log(Array(k[1]-k[0]+1).fill(0).map(function(v,i) {return i+parseInt(k[0]);}).join("_")); [/source]