---
layout: post
title: Simple HeartBeat Manager with Play
categories:
- Java
tags:
- GitHub
- heartbeat
- HTTP
- Java
- petalslink
- playframework
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_mess: ! 'Simple HeartBeat Manager with Play:'
  _wpas_skip_twitter: '1'
  _wpas_done_linkedin: '1'
  _ideation_attached_images: '1434'
  _oembed_911067f5d0527e21bb2a591223444de8: ! '{{unknown}}'
  _oembed_b88e6f6562ecfa267a7b3c9b83a41d80: ! '{{unknown}}'
  _oembed_748622058fd076b3dbe59810f27d7006: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2011-12-16 09:52:57'
  twitter_cards_summary_img_size: a:6:{i:0;i:987;i:1;i:411;i:2;i:3;i:3;s:24:"width="987"
    height="411"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}
---
One more time, one more tiny (and maybe useful in some cases...) application with Play!.
<blockquote>WTF, my server is dead again!?</blockquote>
This app is a simple heartbeat manager looking at remote HTTP services and notifying you by email when something becomes unreachable. It uses the background Job feature of the Play framework and just does HTTP GETs on the specified list. That's all, that's simple, but that's can be useful sometimes...

<img class="aligncenter size-full wp-image-1434" title="hb-manager" src="http://chamerling.files.wordpress.com/2011/12/hb-manager.png" alt="" width="584" height="243" />

The code is located at <a href="https://github.com/chamerling/heartbeat-manager" target="_blank">https://github.com/chamerling/heartbeat-manager</a>
