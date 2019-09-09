---
id: 1480
title: 'My Docker shortcuts: d-'
date: 2017-11-27T18:52:51+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1480
permalink: /2017/11/27/my-docker-shortcuts-d/
categories:
  - Tech
---
I have created some shortcuts on Linux/MacOSX to save me some keystrokes when working with Docker:

  * alias d-rmdi=&#8217;docker images -f dangling=true -q | xargs docker rmi&#8217;
  * alias d-stopall=&#8217;docker ps -q | xargs docker stop&#8217;
  * alias d-rmall=&#8217;docker ps -qa | xargs docker rm&#8217;
  * alias d-names=&#8217;docker ps &#8211;format &#8220;{% raw %}{{.Names}}{% endraw %}&#8221;&#8216;
  * alias d-images=&#8217;docker images &#8211;format &#8220;{% raw %}{{.Repository}}{% endraw %}&#8221;&#8216;
  * function d-bash() { docker ps -a | grep $1 | awk &#8216;{print &#8220;docker exec -it &#8221; $1 &#8221; bash&#8221;;exit;}&#8217; ; }

<img style="max-width:100%;" src="http://localhost/wp-content/uploads/2017/11/1485a-1txashznqpjg_9inpar9a7w.png" /> 

The last one is a function, need to put it in a pair of grave accents to really run it, like: \``` d-bash myapp` ``
