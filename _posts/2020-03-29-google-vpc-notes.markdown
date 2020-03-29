---
title: Google VPC Notes
author: Raymond
layout: post
date: 2020-03-29T11:20:25-04:00
categories:
  - Tech
---
I took quite a few notes in learning how the VPC of Google Cloud works. Here are some highlights:

* Region, Zone and Subnet. When a VPC network is created in auto mode, it automatically creates a subnet in each region, all named 'default'. When an instance's created and a zone is specified, the subnet is hence determined.
* VPC subnet Mode: auto and custom. Auto has default routes (internet gateway and subnet routes) automatcially created. Custom needs to add them manually based on needs.
* Shared VPC can span across different projects in the same organizaiton.
* One VM can only attach to one VPC network with one NIC at most.
* Route and Firewall: A route is needed for communication to possibly happen. But firewall must also allow it.
* A next-hop of a route can be VM instance name, IP address or VPN tunnel name.
* An auto VPC subnet always comes with two routes: 1. default route whose next-hop is default-internet-gateway, for Intenet and Private Google Access); 2. subnet route, for communication between subnets.
* How an instance chooses route: 'applicable routes' are applied to each instance as a set. There is an algorithm to determine which one is chosen(priority, type etc.)
* Load balancer return paths: these routes are not in the routing table, but they are part of the routing game. 
* Cloud NAT can do STUN and TURN.
* Cloud Armor works with Global Load Balander to provide anti-DDoS and other security services.