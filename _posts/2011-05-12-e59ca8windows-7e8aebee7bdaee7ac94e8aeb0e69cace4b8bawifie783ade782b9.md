---
id: 478
title: 在Windows 7设置笔记本为WIFI热点
date: 2011-05-12T22:28:20+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.info/?p=387
permalink: '/2011/05/12/%e5%9c%a8windows-7%e8%ae%be%e7%bd%ae%e7%ac%94%e8%ae%b0%e6%9c%ac%e4%b8%bawifi%e7%83%ad%e7%82%b9/'
original_post_id:
  - "478"
  - "1083"
categories:
  - Gadget
  - Internet
  - 生活
---
有的酒店不提供无线只给有线上网，其实可以将笔记本连上去这样设的，这次出差北京仔细搞了一下，终于搞通了。网上教程很多，只是在最后一步共享里面写得不是很清楚。

步骤：

1、 先确定网卡是否支持。去看看网络连接的适配器里面有没有一个叫做“Microsoft Virtual WIFI Miniport Adapter”的虚拟无线网卡，如果没有，根据网卡类型去下载个新的驱动。如果是Intel AGN 5100，可以[下载这个](http://driver.zol.com.cn/detail/39/384068.shtml)。如果不是，可以参考[这篇教程](http://imjoyo.com/could-not-start-carrying-the-network-group-or-state-resources-to-perform-the-requested-operation-is-not-the-correct-state-solution.html)。

2、 通过三条命令启动共享任务：

> （1）netsh wlan set hostednetwork mode=allow
> 
> （2）netsh wlan set hostednetwork ssid=RaymondWifi key=1234567890
> 
> （3）netsh wlan start hostednetwork

3、如果上述命令都正确返回，则已经启动一个名称为RaymondWifi，密码是1234567890的路由，还有再设置一下共享：在连接了Internet那个网络适配器（我这里是固网那个“本地连接”）那里按右键->属性->共享，勾上第一个勾“允许其他网络用户通过此计算机的Internet连接来连接”，在“家庭网络连接”里面选择Miniport Adapter的那个虚拟无线网卡。确定之后，看到创建的AP在网络列表里并且显示“Internet 访问”，那就成功了。