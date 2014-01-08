---
layout: post
title: PEtALS ESB over the internet
categories:
- SOA4All
tags:
- distributed
- PEtALS
- SOA
- SOA4All
- WebService
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-10-05 13:46:33'
---
I am finally back from the SOA4All project review @ Brussels where I showed a Distributed Service Bus based on PEtALS ESB deployed over the internet.

The first prototype of the SOA4All Distributed Service Bus has been deployed on 4 nodes (three in France at Toulouse, Antibes, Paris and one in Austria at Innsbruck). Each node was providing SOAP access to all the platform services bound to the bus (most of them were SOAP based Web services). It is good to note that response time was quite good between service consumers and providers (some optimizations to be performed to avoid remote checking before sending messages).
<blockquote>Note 1 : It is true that to allow the nodes communication, we opened all the required ports (actually 4 for SOAP/HTTP, JMX and raw TCP communication) but next step will be probably to just use the default HTTP port and this is a real challenge...</blockquote>
<blockquote>Note 2 : The new distributed technical registry (self developed) used to store service endpoints was used in the prototype. Good job ;)</blockquote>
