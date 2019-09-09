---
id: 249
title: 'RT: Problems with Tracking a Rename'
date: 2010-07-29T13:40:57+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=249
permalink: /2010/07/29/rt-problems-with-tracking-a-rename/
original_post_id:
  - "249"
  - "1083"
categories:
  - Tech
---
[extract from book <a href="http://oreilly.com/catalog/9780596520137" target="_blank" rel="noopener noreferrer">Version Control with Git</a>]  
Tracking the renaming of a file engenders a perennial debate among developers of  
version control systems.  
A simple rename is fodder enough for dissension. The argument becomes even more  
heated when the file’s name changes and then its content changes. Then the scenarios  
turn the parley from practical to philosophical: Is that “new” file really a rename, or is  
it merely similar to the old one? How similar should the new file be before it’s considered  
the same file? If you apply someone’s patch that deletes a file and recreates a similar  
one elsewhere, how is that managed? What happens if a file is renamed in two different  
ways on two different branches? Is it less error-prone to automatically detect renames  
in such a situation, as Git does, or to require the user to explicitly identify renames, as  
Subversion does?  
In real-life use, it seems that Git’s system for handling file renames is superior, because  
there are just too many ways for a file to be renamed, and humans are simply not smart  
enough to make sure Subversion knows about them all. But there is no perfect system  
for handling renames…yet.