---
title: HTTP The Definitive Guide Nots
author: Raymond
layout: post
date: 2020-07-24T21:13:25-04:00
categories:
  - Tech
---
Read the book [Http: The Definitive Guide: The Definitive Guide](https://www.goodreads.com/book/show/16675506-http) lately. Had spent so many years working on things related to HTTP, I thought it's time to stop, pick up a book and consolidate my knowledge. Here are some notes which I feel interesting and worth internalization.

* Base64: Only 64+1 ASCII characters in the set, 52 letters (upper/lower case), 10 digits, "/" and "+". Plus "=" which is for padding.
* HTTP headers are ASCII only. Body(after the emptyline) can be anything (just bits) depending on the content-type.
* Line separator in HTTP headers is CRLF
* Header categories. Hop-by-Hop (Only for both sides of direct communication. Should not pass it downstream), End-to-End(Must pass it all the way till the final destination)
* Connection header - Important. Must not be forwarded. 'Close'
 means the TCP connection will be closed when HTTP is done.
 * TCP related delays - some TCP features are usually useless for HTTP e.g. Piggyback, Nagle's Algorithm
* TIME_WAIT accumulation caused by 2MSL (Holding the port for a while even TCP connection is closed)
* TCP handshake review: SYN-SYN/ACK-ACK and FIN-ACK-FIN-ACK
* HTTP connection performance improvement: 1. Parrallel connections; 2. Persistent connections (Reuse); 3. Pipeline connections(Concurrent HTTP reqs on TCP connection); 4. Multiplexed connections.
* Determine the end of HTTP data.
* Keepalive is phased out in HTTP 1.1. HTTP/1.1 persistent connections are active by default (unless explicitly set to 'close'.
* Closing the input channel of your connection is riskier, unless you know the other side doesn’t plan to send any more data. If the other side sends data to your closed input channel, the operating system will issue a TCP “connection reset by peer” message back to the other side’s machine.
* Strictly speaking, proxies connect two or more applications that speak the same protocol, while gateways hook up two or more parties that speak different protocols. 
* The Via header field lists information about each intermediate node (proxy or gateway) through which a message passes. Each time a message goes through another node, the intermediate node must be added to the end of the Via list.
* Cache revalidation - Most caches revalidate a copy only when it is requested by a client and when the copy is old enough to warrant a check. Revalidation (success) is a slow hit.
* Conditional headers used for revalidation:  If-Modified-Since, If-None-Match
* Cache-Control values: no-store(never store or cache); no-cache(must revalidate if cached); must-revalidate
* HTTP Tunnel - Web tunnels are established using HTTP’s CONNECT method. The CONNECT protocol is not part of the core HTTP/1.1 specification, but it is a widely implemented extension
* Basic authentication credentials-carried request header format: Authorization: Basic base64-username-and-password
* Web server certificate validation steps: 1. Date check; 2. Signer trust check; 3. Signature check; 4. Site identity check
* The Content-Length header indicates the size of the entity body in the message, in bytes. Clients need Content-Length to detect message truncation. The size includes any content encodings (the Content-Length of a gzip-compressed text file will be the compressed size, not the original size). The Content-Length header is mandatory for messages with entity bodies, unless the message is transported using chunked encoding.
* Chunked encoding breaks messages into chunks of known size. Each chunk is sent one after another, eliminating the need for the size of the full message to be known before it is sent. Note that chunked encoding is a form of transfer encoding and therefore is an attribute of the message, not the body. Multipart encoding, described earlier in this chapter, is an attribute of the body and is completely separate from chunked encoding.
* Trailers in chunked messages - A trailer can be added to a chunked message if the client’s TE header indicates that it accepts trailers. The trailer can contain additional header fields whose values might not have been known at the start of the message.
* Charset Is Poorly Named - Technically, the MIME charset tag (used in the Content-Type charset parameter and the Accept-Charset header) doesn’t specify a character set at all.
* URIs essentially consist of a restricted subset of ASCII characters.
* The HTTP/1.1 specification does not define any mechanisms for transparent negotiation, but it does define the Vary header. Servers send Vary headers in their responses to tell intermediaries what request headers they use for content negotiation.
 
