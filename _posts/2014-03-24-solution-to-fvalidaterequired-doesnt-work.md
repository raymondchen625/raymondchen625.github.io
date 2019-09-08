---
id: 944
title: 'Solution to &#8220;f:validateRequired doesn&#8217;t work&#8221;'
date: 2014-03-24T16:13:05+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=944
permalink: /2014/03/24/solution-to-fvalidaterequired-doesnt-work/
original_post_id:
  - "944"
  - "1083"
categories:
  - Tech
---
I found this problem on my JSF application running on Tomcat: the <f:validateRequired /> didn&#8217;t work: When I let the input field empty and submit, no validation message shown and the field value became zero automatically.

Solution:

1) Tomcat convert null value to Zero automatically, so add this JVM option to catalina.sh: -Dorg.apache.el.parser.COERCE\_TO\_ZERO=false

2) JSF doesn&#8217;t validate empty field by default! Set this context parameter, javax.faces.VALIDATE\_EMPTY\_FIELDS, to true in web.xml

&nbsp;