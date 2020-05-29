---
title: Mixed TLS Types
author: Raymond
layout: post
date: 2020-05-29T19:07:00-04:00
categories:
  - Tech
---
Behind my Nginx-Ingress-Controller, I have two services. Service A is configured with one-way TLS; while Service B is configured with two-way TLS.
When I curl Service B, I found it is actually a one-way TLS because it doesn't need me to specify a client certificate and a key.
After investigation, I find the direct reason is A and B are sharing the same hostname, only their context paths are different. So when TLS handshake is initiated and client is happy with the server's certificate. Nginx needs to decide whether it should ask fo the client to present a certificate i.e. one-way or two-way. But all Nginx knows is the domain name from the SNI(Service Name Indictor), it can't get the context path because the HTTP connection is not established yet. The context path is in the HTTP header. So Nginx has two matching configuration with that hostname, one says one-way the other says two-way. For some reason Nginx falls back to one-way TLS.
Once we change them to use different domain names, both are working well.