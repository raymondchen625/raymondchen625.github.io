---
id: 254
title: '设置DD-WRT及Chnroutes(201009#4)'
date: 2010-09-22T03:12:10+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.info/?p=254
permalink: '/2010/09/22/%e8%ae%be%e7%bd%aedd-wrt%e5%8f%8achnroutes2010094/'
original_post_id:
  - "254"
  - "1083"
categories:
  - Internet
  - 技术
---
换了个支持DD-WRT的LinkSys二手路由。一来家里的旧无线路由只支持WEP加密，看完<a href="http://blog.codingnow.com/2010/09/nz.html" target="_blank" rel="noopener noreferrer">云风的一个博文</a>后大受刺激，决定换掉。其实我早知道WEP是能破的，但懒惰作怪，又总是愿意假设邻居里面没有懂技术的人，或者懂的也不屑于干这种事。但你看，名人到了国外还不照样蹭网？还写成博客登出来。

路由拿到手，设置比较折腾。说好要支持OpenVPN的ROM，淘宝卖家不太懂这些，还是给了个不支持的STD给我。本来不想学刷ROM，结果还是逼着学会了（不难，在web界面就可以做，记住给自己电脑设个静态IP就是了），因为第一个没支持JFFS，还刷了两遍。

设置<a href="http://code.google.com/p/autoddvpn/" target="_blank" rel="noopener noreferrer">autoddvpn</a>没有成功，反而在周六把<a href="http://code.google.com/p/chnroutes/" target="_self" rel="noopener noreferrer">chnroutes</a>设好了。原理其实挺简单的（不知官网为何写得看起来那么复杂，汗。。），但因为我是Windows 7，它提供的脚本有个vbs上的bug，加路由会出错，结果还是折腾了很久我才从log和文档中解决了问题。目前是写死网关地址，反正家里网关是固定的（公司基本也是固定的），每次手工跑一下即可。

原理也不妨解释一下：在连上VPN之后，默认路由一般都会走VPN的网关，这个不要改。你要做的事就是把访问中国（墙内）服务器的IP地址的路由修改到不要走VPN，走你原来上网的那个网关。在挂断VPN的时候，把这些添加的路由给清理删除一下。就这么简单。我改了一个<a href="http://www.raymondchen.com/download/UpdateRoute_Home.bat" target="_blank" rel="noopener noreferrer">自己用的脚本</a>，把网关写死为192.168.1.1，其它人也可以用，把网关改成自己路由的网关IP即可。清理脚本为<a href="http://www.raymondchen.com/download/vpndown.bat" target="_self" rel="noopener noreferrer">vpndown.bat</a>，挂断之后运行一下即可。如果不是chnroutes提供的脚本在Windows 7下有bug，这两步是全自动的，不用每次跑那么麻烦。

有了这个成果，设好DD-WRT上的autoddvpn应该也是指日可待的事，等有空再弄弄。

这个方案和客户端自己上vpn比，有优势也有劣势。好处当然是客户端什么都不要做了，基本上实现自动透明翻墙。弱点是不够精确，并非所有“境外”IP都被墙的，在这个方案之下，访问非被墙的外国网站，速度会慢一些，对VPN主机也带来了流量的压力。这个可以不断优化路由表来解决，但这就走到autoproxy的路子去了。在vpn的速度比较理想、流量也没经济压力的情况下，这个方案我觉得是更佳选择。

对手机和ipad等设备，每次都要连一下vpn是很折腾的一件事。