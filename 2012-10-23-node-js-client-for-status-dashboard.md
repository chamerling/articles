---
layout: post
title: Node.js client for Status Dashboard
categories:
- Node
tags:
- dashboard
- JavaScript
- monitoring
- Node.js
- petalslink
- socket.io
- terminal
- Web application
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _publicize_pending: '1'
  _wpas_mess: Node.js client for Status Dashboard
  _wpas_skip_64254: '1'
  _wpas_skip_21153: '1'
  _wpas_done_64253: '1'
  _publicize_done_external: a:1:{s:8:"linkedin";a:1:{s:10:"fGqzF5BxEd";b:1;}}
  _wpas_skip_64253: '1'
  twitter_cards_summary_img_size: a:6:{i:0;i:1106;i:1;i:459;i:2;i:3;i:3;s:25:"width="1106"
    height="459"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}
  _elasticsearch_indexed_on: '2012-10-23 21:59:29'
---
<a href="https://github.com/obazoud/statusdashboard" target="_blank">Status Dashboard</a> is an awesome <a class="zem_slink" title="Node.js" href="http://nodejs.org/" target="_blank" rel="homepage">node.js</a> monitoring application developed by <a href="https://twitter.com/obazoud" target="_blank">@obazoud</a>. I recently sent some pull requests to Olivier to improve the IRC plugin and then I though that even if I am always connected to IRC, jabber or whatever, I also have a Terminal opened most of the time. So the question was: How can I get my services status pushed to my laptop in realtime?
<a href="http://socket.io" target="_blank">Socket.IO</a> is the candidate: It does not provide only server and browser modules, there is also the <a href="https://github.com/LearnBoost/socket.io-client" target="_blank">socket.io-client</a> module which can be used in your node runtime, on the client side in exactly the same way you use Socket.IO in your HTML pages. Since Socket.IO is already used in Status Dashboard to push status to the browser, we have the right solution.

I created a simple node.js client application called <a href="https://github.com/chamerling/statusdashboard-client" target="_blank">statusdashboard-client</a> which connect to a status dashboard instance using Socket.IO. Once data is pushed by the server to the client, it is displayed with some basic code colors on the terminal:

<img class="aligncenter" title="Monitoring in your terminal" alt="" src="http://f.cl.ly/items/2v1F233Q0s0t2o0l1p2s/statusdashboard-client.png" height="459" width="1106" />

It is totally fun to see what we can do without any node.js expertise. I just start looking at it but I already have many ideas, especially for platform monitoring.

[youtube=http://youtu.be/xGcHElsUWFk]
