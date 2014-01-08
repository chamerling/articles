---
layout: post
title: Open Source Tweet Wall
categories:
- Java
tags:
- Activity stream
- GitHub
- petalslink
- Social network
- twitter
- twitter4j
- wall
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
  _wpas_mess: ! 'Open Source Tweet Wall:'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-11-21
    21:44:25";}
  _oembed_53b853ad34a6581a776b5df4521ef372: ! '{{unknown}}'
  _oembed_e1e71384b42f1086398b2603214dd8d6: ! '{{unknown}}'
  _oembed_7007af38613ed6d78981a2d7cd3e7317: ! '{{unknown}}'
  _oembed_3cfc4e6a5dd11fd360091ebc20143359: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2011-11-21 21:44:25'
---
I am a big fan of <a class="zem_slink" title="Twitter" href="http://twitter.com" rel="homepage">Twitter</a> and last week devoxx live tweet wall (<a href="http://wall.devoxx.com/" target="_blank">http://wall.devoxx.com/</a>) just made me want to create an open source live tweet wall. So last saturday night, I took some time to code it using the now well known <a class="zem_slink" title="Play Framework" href="http://www.playframework.org" rel="homepage">Play! framework</a>, some Web sockets and <a href="http://twitter4j.org/" target="_blank">Twitter4J</a> which supports the Twitter Stream API. This streaming API is really nice and provide a way to be notified almost instantly according to what you decided to listen to. After less than one hour (most of the time was consumed to read to Twitter4J documentation), the result is really fun. In the video below, I configured the wall to catch all the tweets containing 'Apple' and 'Microsoft' terms (I am not a MS supporter, but OMG, there are lot of tweets about it... probably only jokes and bugs...), and tweets are displayed in live on the wall (no I am not moving anything, the page is populated through a Web socket)

[vimeo http://vimeo.com/32418988]

The resulting prototype is quite simple but works, and since I am not a front-end developer at all, it uses Twitter Bootstrap as CSS... The code is available on GitHub at <a href="https://github.com/chamerling/play-twall" target="_blank">https://github.com/chamerling/play-twall</a> and is not perfect at all but just works: Twitter configuration is available in the Play! <em>application.conf</em> file and the Twitter connection is created in the Bootstrap...

Looks like I should start creation sort of "Coding a nice thing in one hour" collection...
