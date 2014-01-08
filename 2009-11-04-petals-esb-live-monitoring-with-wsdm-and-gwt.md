---
layout: post
title: PEtALS ESB Live Monitoring with WSDM and GWT
categories:
- PEtALS
- SOA
- WebService
tags:
- comet
- gwt
- Java
- management
- monitoring
- ow2
- PEtALS
- SOA
- WebService
- wsdm
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _ideation_attached_images: '237'
  _elasticsearch_indexed_on: '2009-11-04 16:29:28'
---
In one of my previous posts (<a href="http://chamerling.wordpress.com/2009/11/02/adding-registry-listener-in-petals/" target="_blank">Adding Registry Listener in PEtALS</a>), I spoke about adding a registry listener in <a href="http://petals.ow2.org" target="_blank">PEtALS ESB</a>. In the current article, I want to introduce how I used this feature to implement a live monitoring Web application.

Here are the different modules which are used in this Live Monitoring Tool :
<ol>
	<li>PEtALS ESB. The standard behaviour has been customized by adding a registry listener and a routing module (to be detailled below).</li>
	<li>Monitoring layer. This layer is an independant process which embeds a WS-Notification engine.</li>
	<li>A WS-notification subscriber. This is the module which will receive the notifications from the monitoring layer.</li>
	<li>GWT based Web application used to display monitoring data.</li>
</ol>
<strong>PEtALS ESB Extensions</strong>

<em>Registry Listener</em>
The role of the registry listener is to register a new monitoring endpoint into the monitoring layer when a new endpoint is available within PEtALS.

<em>Routing Module</em>
Since modules can be added dynamically inside the PEtALS message router, we have created a module which timestamp the messages. Once the message exchange is complete, a message exchange report is sent to the monitoring layer.

<strong>Monitoring Layer</strong>

The monitoring is in charge of creating monitoring endpoints through a management API. Once a monitoring endpoint is created, it is also exposed as a Web service. This newly created Web service exposes a WS-notification subscribe operation.
Another role of the monitoring layer is to receive raw reports from the PEtALS ESB, to process the report in order to generate a WSDM payload which will be send to subscribers.

<strong>WS-Notification subscriber</strong>

The subscriber subscribes, receives and stores notifications from the Monitoring layer. That's all for that module ;o)

<strong>GWT Based Web application</strong>

The GWT Web application uses comet in order to display live service response time. The data used to display response time is the one received and stored into the database and of course the server part of the Web application have access to this database.

As a result, we have a really nice live Web application (live means that the chart gives real time result and is updated automatically when messages are exchanged within PEtALS Service Bus). Here are some screenshots :

[caption id="attachment_235" align="aligncenter" width="480" caption="Live Response Time"]<a href="http://chamerling.files.wordpress.com/2009/11/response_time.jpg"><img class="size-full wp-image-235" title="response_time" src="http://chamerling.files.wordpress.com/2009/11/response_time.jpg" alt="Live Response Time" width="480" height="255" /></a>[/caption]

[caption id="attachment_237" align="aligncenter" width="480" caption="SLA Violation"]<a href="http://chamerling.files.wordpress.com/2009/11/sla.jpg"><img class="size-full wp-image-237" title="SLA" src="http://chamerling.files.wordpress.com/2009/11/sla.jpg" alt="SLA" width="480" height="254" /></a>[/caption]
