---
title: IP Protocol Notes
author: Raymond
layout: post
date: 2020-07-26T18:55:25-04:00
categories:
  - Tech
---

Some notes from the book [TCP/IP Illustrated](https://www.goodreads.com/book/show/13094531-tcp-ip-illustrated-volume-1)
* Types of IP address: Unicast, Multicast & Broadcast
* When checksum of an IP packet detects a failure, IP implementation simply discards the packet without any error message. It's called packet loss. It's up to the upper layer to decide whether to retransmit or not.
* What differentiates a host from a router to IP is how IP datagrams are handled: a host never forwards datagrams it does not originate, whereas routers do.
* Upon receiving a datagram: 1. Check if destination IP mathches one of interface's IP; 2. If yes, based on the Protocol field in the header, packet is sent to the protocol module for proessing; 3. If no, discard if not configured as router; or foward the packet.
* Typical fields in routing or forwarding table: destination, mask, next-hop and interface.
* IP forwarding is performed on a hop-by-hop basis. As we can see from this forwarding table information, the routers and hosts do not contain the complete forwarding path to any destination.
* Forwarding decisions at the IP layer are based on the destination address. So destination IP is usually not changed. But source IP might change, e.g. NAT.
* When a host sends an IP datagram, it must decide which of its IP addresses to place in the Source IP Address field of the outgoing datagram. In modern IP implementations, the IP addresses used in the Source IP Address and Destination IP Address fields of the datagram are selected using a set of procedures called source address selection and destination address selection.
* For IPv4, the class D space (224.0.0.0–239.255.255.255) has been reserved for supporting multicast. Special use in different multicast ranges: Local network control, Internetwork control, Source-specific multicast (SSM), GLOP.
* Using CIDR, any address range is not predefined as being part of a class but instead requires a mask similar to a subnet mask, sometimes called a CIDR mask.