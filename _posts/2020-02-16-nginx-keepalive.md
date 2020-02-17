---
title: Nginx Keepalive
author: Raymond
layout: post
date: 2020-02-16T19:40:25-05:00
categories:
  - Tech
---
In Kubernetes environment we put an Ngxin-Ingress-Controller before our Java services, and we found in customer production some long running requests will get a '000' response code. The processing on Java side is completed, we wonder why Nginx is not able to return the data and generates this weird code. 

Googling indicates '000' is a generic network error. We switched to an old Nginx-Ingress-Controller version and it worked. The difference is in the old version, the 'keepalive(KA)' feature doesn't work. Our theory was in the customer's production environment, there were some devices which were killing the kept-alive TCP connection. But if Nginx sends out KA packet, the network device shouldn't see it as a stale connection and kill it. Maybe it's a misconfiguration? To find out the reason, I used tcpdump to capture packets inside a Java container. I sent a bunch of requests to a JSP page which sleeps for a period of time specified in the request parameter. The keepalive timeout setting was set to 15 seconds.
<img class="alignnone size-full wp-image-1653" src="/wp-content/uploads/2020/02/nginx-tcpdump.png" alt="gke" width="700" height="262" srcset="/wp-content/uploads/2020/02/nginx-tcpdump.png 700w, /wp-content/uploads/2020/02/nginx-tcpdump.png 300w" sizes="(max-width: 700px) 100vw, 700px" />

From the screen capture we can see, when JSP is processing the request, there is no KA packet. Neither there is any after the JSP response until it reaches the timeout setting and closes the TCP connection with a FIN. So Nginx doesn't send KA packet. 

Since Nginx doesn't send KA packet but keeps the TCP connection, some network components along the route deem the connection stale and close it. That's the reason of the network error. To disable keepalive feature, we set the 'upstream-keepalive-connections' to 0, it will close the TCP connection immediately after serving the request. We captured the packets again and this time we could see the TCP connection closed right after returning the reponse from the upstream JSP page.

We haven't got to the root cause of what is acutally causing the problem. The customer had other priorities. But up to this point, we have more understanding of how Nginx works. Nginx as a proxy terminates the TCP connection from its client, and it establishes another TCP connection with its upstream server. It holds the mapping relationship during the request-proxying process. It doesn't send out any KA packet.

