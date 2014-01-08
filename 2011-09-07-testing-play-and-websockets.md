---
layout: post
title: Testing Play! and WebSockets
categories:
- Java
- WebService
tags:
- Java
- Open source
- petalslink
- play!
- playframework
- poc
- Web service
- WebService
- websockets
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_done_linkedin: '1'
  tagazine-media: a:7:{s:7:"primary";s:60:"http://f.cl.ly/items/0G3J2Q2X3k0t3t0c0O2q/soap-websocket.JPG";s:6:"images";a:2:{s:60:"http://f.cl.ly/items/0G3J2Q2X3k0t3t0c0O2q/soap-websocket.JPG";a:6:{s:8:"file_url";s:60:"http://f.cl.ly/items/0G3J2Q2X3k0t3t0c0O2q/soap-websocket.JPG";s:5:"width";s:3:"800";s:6:"height";s:3:"598";s:4:"type";s:5:"image";s:4:"area";s:6:"478400";s:9:"file_path";s:0:"";}s:58:"http://f.cl.ly/items/0v3z2h042t1e303K3i3N/play-soap-ws.png";a:6:{s:8:"file_url";s:58:"http://f.cl.ly/items/0v3z2h042t1e303K3i3N/play-soap-ws.png";s:5:"width";s:3:"582";s:6:"height";s:3:"211";s:4:"type";s:5:"image";s:4:"area";s:6:"122802";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"2";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-09-07
    07:57:34";}
  _wpas_mess: ! 'Testing Play! and WebSockets:'
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-09-07 07:57:34'
  twitter_cards_summary_img_size: a:7:{i:0;i:800;i:1;i:598;i:2;i:2;i:3;s:24:"width="800"
    height="598"";s:4:"bits";i:8;s:8:"channels";i:3;s:4:"mime";s:10:"image/jpeg";}
---
I spent one hour playing with the [Play Framework](http://playframework.org) and WebSockets in order to push some (SOAP) messages received on some Web services hosted by the Play application to the clients browser.

<img src="http://f.cl.ly/items/0G3J2Q2X3k0t3t0c0O2q/soap-websocket.JPG" alt="" />

The result is really amazing: We can simply push these SOAP messages to clients in less than 100 lines of code. There are some problems with some messages lost due to some conception problems but which are not Play ones. In fact, the current prototype just send the messages to all the clients but what should be done is creating streams per client with some ID to identify them...

The source code is available on GitHub at [https://github.com/chamerling/play-soap-websockets](https://github.com/chamerling/play-soap-websockets)

<img src="http://f.cl.ly/items/0v3z2h042t1e303K3i3N/play-soap-ws.png" alt="" />

A quick screen capture with SOAPUI sending messages to the Play! SOAP service. The Play application pushes the SOAP message to the clients (Two browsers).
[vimeo 28669875 w=600 h=375]
