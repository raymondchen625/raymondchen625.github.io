---
id: 99
title: Social-networking inside GFW
date: 2010-05-08T02:21:33+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.info/?p=99
permalink: /2010/05/08/mobile-social-networking-inside-gfw/
original_post_id:
  - "99"
  - "1083"
categories:
  - Internet
  - 技术
tags:
  - GFW
  - Social-networking
---
都知道Facebook和Twitter在中国大陆境内是被封锁了的。如果要做一个手机应用软件，怎样绕过GFW？

对懂技术的，VPN和SSH Tunnel肯定会搞。但对普通用户，怎样让过程变得简单一些。下面是一个很粗糙的Idea：

1、 安装之后让用户填写邮箱帐号和密码，可以是hotmail、gmail、yahoo等等；邮箱就是一个获取翻墙信息的桥梁。当然用户如果毋须翻墙就不必填了；

2、 软件通过用户的邮箱，用email向特定自动应答的邮箱发一个邮件，对方将翻墙的信息加密之后返回，譬如是一个ssh的帐号，或者一个代理设置信息；

3、 软件将内容解密，设置到自己的链接方式中，那就可以上了。遇到代理不能用的情况，软件还可以通过邮件发送服务情况反馈。同时可以再拿一个新的。

用邮件作为获取翻墙信息的媒介，主要考虑GFW不太可能封锁所有邮箱。又比较能保证对普通用户的易用性。

需要考虑的一些因素：

1、  自动发送和应答的邮件可能会被邮件服务商或者用户自己当成了SPAM，可能要考虑一下一些绕过的处理机制；

2、 用户可能会不愿意提供邮箱密码，但其实这只需要一个可用的邮箱即可以，不一定要私人邮箱；

3、 GFW可能会进行攻击。例如大量获取配置信息进行封锁，或者控制软件大量发送错误的服务不可用信息。