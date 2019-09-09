---
id: 891
title: How to Resize a Out-of-Space VirtualBox Disk
date: 2014-02-07T13:05:27+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=891
permalink: /2014/02/07/how-to-resize-a-out-of-space-virtualbox-disk/
original_post_id:
  - "891"
  - "1083"
categories:
  - Tech
---
I installed an CentOS 6.5 64-bit guest on my VirtualBox host. Later I found the 10-Gigabyte disk space I&#8217;d allocated was used up. I needed to add more space to it. I found the solution after some study. Here is how:

1. Shutdown the guest virtual machine. Find the disk image file(.vdi) on the host server, use this command to resize it (say increasing it to 20G): vboxmanage modifyhd your_guest.vdi &#8211;resize 20000

2. Download the live cd file (.iso) of GParted tool, (like gparted-live-0.17.0-4-i486.iso). Start up the guest vm with booting from that iso file.

3. In GParted tool, resize the partition. It&#8217;s pretty straightforward, just remember to save the change.

4. Reboot the guest vm, starting from disk, of course. Change the LVM(using a LVM to manager your disk is a good idea, especially useful in such occasion. So, don&#8217;t change this default setting when installing a new vm) setting:

> lvextend /dev/vg\_nile/lv\_root /dev/sda2;  
> resize2fs /dev/vg\_nile/lv\_root

// assume the lvm group name is &#8220;vg\_nile&#8221;, logical volume to be extended is &#8220;lv\_root&#8221;, and the new partition shown up is &#8220;/dev/sda2&#8221;.

It&#8217;s all set. Verify that with df command.

It is said I can achieve the same goal by creating a new virtual disk and cloning all current data to it from the old one. Then replace the old vdi with the new one. I haven&#8217;t try it yet.