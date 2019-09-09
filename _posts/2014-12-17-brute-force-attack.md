---
id: 1079
title: Brute Force Attack
date: 2014-12-17T00:45:31+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=1079
permalink: /2014/12/17/brute-force-attack/
original_post_id:
  - "1079"
  - "1083"
categories:
  - Uncategorized
---
裝了WordPress運行著，無論多麼少人訪問，總會被黑客的肉雞盯上，不知疲倦地重複發送登錄請求，試圖找到弱密碼，從此俘獲一個新肉雞。

偶然查看log，發現大量來自[<img class="alignleft size-full wp-image-1080" src="http://www.raymondchen.com/wp-content/uploads/2014/12/capture1.jpg" alt="Access Log" width="835" height="505" />](http://www.raymondchen.com/wp-content/uploads/2014/12/capture1.jpg)一個Amsterdam的IP，不斷訪問/wp-login.php，顯然就是在不斷嘗試login。。。

儘管我的密碼不弱，但這樣任由它不斷嘗試也是件令人不安的事。WP的插件BruteProtect實際也沒有發生什麼功效，至少它不能在網絡的層次將它封殺。

於是寫了個script，定時運行一下，如果最近的access log裡面有異常多的同一IP嘗試login，就封它24小時。

> #!/bin/bash  
> cd /home/pi/firewall  
> zombieList=\`tail -1000 /var/log/apache2/access.log | grep wp-login.php | grep &#8221; 500 &#8221; | awk &#8216;{print $1}&#8217; | sort | uniq -c | awk &#8216;$1>100{print $2}&#8217;\`  
> for ip in $zombieList  
> do  
> grep $ip blocked\_ip\_list  
> if [ &#8220;$?&#8221; = &#8220;1&#8221; ]  
> then  
> echo &#8220;$ip&#8221; >> blocked\_ip\_list  
> fi  
> done

Firewall設置改變的時候，還順帶發個通知到我手機吧。

[<img class="alignleft size-full wp-image-1083" src="http://www.raymondchen.com/wp-content/uploads/2014/12/Pushover.jpg" alt="Pushover" width="441" height="312" />](http://www.raymondchen.com/wp-content/uploads/2014/12/Pushover.jpg)