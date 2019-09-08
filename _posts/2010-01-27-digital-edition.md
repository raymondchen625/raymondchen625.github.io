---
id: 87
title: Digital Edition
date: 2010-01-27T10:30:38+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=87
permalink: /2010/01/27/digital-edition/
original_post_id:
  - "87"
  - "1083"
categories:
  - Book
  - Tech
tags:
  - ebook
---
After fooling around with the DRM-protected material of Adobe Digital Edition for some days, I guess I&#8217;ve figured out the mechanism. Here is my guess, without deep googling:

There is an Adobe ID first. It represents a user. When a user buys a ebook, e.g. on BooksOnBoard.com, a small *.acsm file is downloaded. The book is not within that .acsm file. It contains the information to verify the purchase and where to download the book. When that .acsm file is opened by the appropriate software, like Adobe Digital Edition or Sony Reader Library(which in turn calls Adobe Digital Edition), the download process is kicked started.

Before the downloaded process, the software will have to authorize some hardware first. Those hadware include computer and digital reader. I guess the Adobe ID is bound with the hardware IDs. Then when it starts to download, it send those information to the download website.  When the server receive these information, it verify with the purchase and download history data, if it&#8217;s ok, it encrypt the original content, of the book, with the Adobe ID as well as the authorized hardware information and created an DRM-protected file. This file is sent back to the user. Then the user can read this book on his authorized devices only.

There are times the book is allowed to be read on multiple, like six, devices. And it&#8217;s usually the case. Can&#8217;t imagine it can only be read on digital reader but not on the PC which is used to transfer the book. So the download server must record the authorized device ID so that it won&#8217;t be unlimitedly used.

Guess that&#8217;s it. Don&#8217;t know how close it is to the truth.