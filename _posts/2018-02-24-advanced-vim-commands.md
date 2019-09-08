---
id: 1514
title: Advanced VIM commands
date: 2018-02-24T18:40:40+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1514
permalink: /2018/02/24/advanced-vim-commands/
timeline_notification:
  - "1519515643"
categories:
  - Tech
---
This only covers the commands which I deem &#8220;advanced&#8221; i.e. I shouldn&#8217;t forget but I usually do

  * ctrl-o(Insert mode): enter command mode but go right back to insert mode upon finish
  * gJ: join lines without adding space in-between
  * gu or gU + action: change to lower/upper case
  * :bd &#8211; only close current file, not the whole vi window
  * macro: record with qw  do something q; and call it with @w (w can be any register name)
  * a/i/t (all/inside/till) operation. e.g. &#8216;ci&#8221;&#8216; change everything inside double quotes (exclusive)
  * :e reload changes from disk
  * Ctrl-G : show file info
  * :r !cmd|filename : run a command or from a file, read output into vi
  * m and &#8216; : set and got to bookmark
  * &#8216;0 : go to last place when vi exited
  * z+{.|-|Enter}: make current line at the screen Top/Bottom/Centre
  * :set list/nolist: show line feed
  * :tabn,:tabp,:tabe {filename} : Use tab for multiple files