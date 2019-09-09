---
id: 1625
title: Manifest and Lock
date: 2019-04-14T09:36:48+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1625
permalink: /2019/04/14/manifest-and-lock/
timeline_notification:
  - "1555249008"
categories:
  - Tech
---
I  saw the usage of manifest and lock before, like Yarn for a node.js project. But rarely gave much thought on how it worked.

[Sam Boyer](https://twitter.com/sdboyer) provides [a brief summary](https://changelog.com/gotime/36) on this which sounds great to me:

> &#8230; manifests essentially describe constraints, and manifests only describe constraints on your direct dependencies, whereas locks are a transitively-complete picture of the entire dependency graph; there aren’t constraints in there, there are specific revisions, ideally immutable revisions.