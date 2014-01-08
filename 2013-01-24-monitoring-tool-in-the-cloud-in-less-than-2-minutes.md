---
layout: post
title: Monitoring Tool in the Cloud in (less than) 2 minutes...
categories:
- Cloud
- Node
tags:
- cloud
- Heroku
- JavaScript
- Node.js
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
  _wpas_mess: Monitoring Tool in the Cloud in (less than) 2 minutes...
  _wpas_skip_64254: '1'
  _wpas_skip_64253: '1'
  _wpas_skip_21153: '1'
  _elasticsearch_indexed_on: '2013-01-24 14:26:59'
---
This article will show how to deploy a simple monitoring application in the Cloud in a couple of minutes using statusdashboard and <a class="zem_slink" title="Heroku" href="http://www.heroku.com/" target="_blank" rel="homepage">Heroku</a>.

I already introduced <a href="https://github.com/obazoud/statusdashboard" target="_blank">StatusDashboard</a> <a title="Node.js client for Status Dashboard" href="http://chamerling.org/2012/10/23/node-js-client-for-status-dashboard/" target="_blank">several months ago</a> with a small websocket client I wrote allowing to connect to the web application and streaming monitoring data directly in the terminal. The monitoring data is produced by the server which is periodically trying to call a set of defined services (HTTP, HTTPs, FTP, TCP, UDP, ...).

In the last version, you can now embed StatusDashboard in any node application with a really simple to use API. This is especially nice to customize it for your needs, for example, getting settings from a configuration file, from a remote service or whatever:

https://gist.github.com/4621293

Let's deploy it on Heroku...

https://gist.github.com/4621421

There is one problem with the current approach: When running such an application on Heroku with the default Heroku Plan, it will ends when nobody browse it. This will stop the monitoring loop which is not really useful for a monitoring app... In order to avoid such behavior, I added a heartbeat mechanism last night which is configured from the application settings. It also needs to define some environment variable with the heroku client. Let's assume that your application is running on http://YOURAPP.herokuapp.com, you have to define this variable like:

https://gist.github.com/4621482

It will restart your application (if not, restart it manually), will start to ping itself and keep the application alive.

The code of this article is available at https://github.com/chamerling/statusdashboard-server. You get fork/clone/whatever and start monitoring your services in the next minutes...

And the proof that it works in less than 2 minutes (with a poor Internet connection, and taking time to browse source code...)

http://www.youtube.com/watch?v=VGygy6SNpsA

&nbsp;
