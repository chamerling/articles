---
layout: post
title: Service Bus Live Monitoring
categories:
- Java
- SOA
tags:
- bus
- dsb
- eda
- esb
- event
- Message
- monitoring
- PEtALS
- petalslink
- petlas
- Play Framework
- servicebus
- SOA
- SOAPUI
- Web service
- WebSocket
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  tagazine-media: a:7:{s:7:"primary";s:72:"http://f.cl.ly/items/2y353o3l2T062n2b1q3V/servicebus-live-monitoring.jpg";s:6:"images";a:1:{s:72:"http://f.cl.ly/items/2y353o3l2T062n2b1q3V/servicebus-live-monitoring.jpg";a:6:{s:8:"file_url";s:72:"http://f.cl.ly/items/2y353o3l2T062n2b1q3V/servicebus-live-monitoring.jpg";s:5:"width";s:3:"800";s:6:"height";s:3:"483";s:4:"type";s:5:"image";s:4:"area";s:6:"386400";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-09-30
    16:03:03";}
  _wpas_mess: ! 'Service Bus Live Monitoring:'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _elasticsearch_indexed_on: '2011-09-30 16:03:03'
  twitter_cards_summary_img_size: a:7:{i:0;i:800;i:1;i:483;i:2;i:2;i:3;s:24:"width="800"
    height="483"";s:4:"bits";i:8;s:8:"channels";i:3;s:4:"mime";s:10:"image/jpeg";}
---
I explained in the last articles how I tested the Play Framework, Web sockets and how I integrated all this nice stuff with a real example based on a Service Bus, Web services Notifications, etc…

This time, let’s go one step further. We have a Service Bus which is Web service notification enabled like last time. We can bind services to the bus, expose service endpoints as Web services, blahblahblah… But, this time, I am interested on having some real time monitoring of service invocations. It means that each time a message goes through the service bus (a service invocation in fact), I want to know (almmost) immediatly the service response time.
Hopefuly, the PetalsLink Distributed Service Bus I develop and use provides many extension points. One is the capability to add modules to the routing engine ie the software module each message must be able to go through on service request and response. So adding some router module which catch all the messages, timestamp them and then send this monitoring data to someone is quite easy. At the implementation level, this monitoring router module publishes monitoring reports to the service bus notification engine topic dedicated to monitoring stuff.

So, a client interested in monitoring data just has to register itself as subscriber to the monitoring notification topic. Every time a message is published in the topic, it will be delivered to all the subscribers. Up to the subscriber to display data as soon as it can. This is where Play, Web sockets and some cool javascript library came in. Since I never developed javascript stuff, I tried to find an easy to integrate solution to create some moving plots, asking twitter. I finally found the <a title="Smoothie Charts" href="http://smoothiecharts.org/">Smoothie Chart</a> library which is really easy to use and updates graph in real time.

The high level architecture of the system can be defined as

<img src="http://f.cl.ly/items/2y353o3l2T062n2b1q3V/servicebus-live-monitoring.jpg" alt="" />

The following video shows the result of the complete stack: Each time a message a service is invoked with SOAPUI, a Web service notification is sent to a Play application which subscribed to the monitoring topic, the Play application then pushes the data to the client by using a Web socket. Finally, the javascript code on the client side feeds the Smoothie chart which updates automatically. At the end, it is quite simple and efficient.

[vimeo http://www.vimeo.com/29803280 w=600&h=375]

Oh, I forgot to say something: This took me 2 or 3 hours to create all this stuff… The code has been published on github in the <a href="https://github.com/chamerling/dsbmanager-webapp">dsbmanager-webapp</a> project.
