---
layout: post
title: BPEL support in Petals DSB
categories:
- PEtALS
tags:
- Application programming interface
- Business process
- Business Process Execution Language
- dsb
- Java Business Integration
- Petals BPM
- Web application
- Web service
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_mess: ! 'BPEL support in Petals DSB: http://wp.me/pcSx0-ml'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_done_linkedin: '1'
  _elasticsearch_indexed_on: '2011-12-01 15:12:28'
---
As promised in the last article about my talk at OW2Con 2011 last week, here is a video on something I was not able to show due to some low resolution problems. The video is a bit long but shows several things (in the right order):
<ol>
	<li>The DSB Manager Web application is used to manage the Distributed Service Bus. It uses the DSB Web service API to interact with node instances running somewhere...</li>
	<li>The DSB Manager is used to bind business services to the DSB (let's forget JBI, the user does not care about it...). DSB services are also exposed. Every DSB node provides the same business API with the help of the distributed endpoint registry it uses.</li>
	<li>The DSB Manager uses the DSB BPEL API to deploy BPEL processes to the DSB. Up to the DSB to use the right internal endpoint when the process is executed. Services can be hosted on any node, it is the role of the DSB to route messages to the right endpoint on the right node. The BPEL process is exposed as Web service and can be invoked by any Web service client. Here I just use SOAPUI client.</li>
	<li>We can monitor what happens when invoking a service! For now the DSB Manager uses Web service notification to subscribe to some monitoring topic hosted on the DSB node. When a message is exchanged between the client and the services involved in the process execution, notification are automatically published to the DSB Manager which has just subscribed. The monitoring uses Web sockets for live display in the browser...</li>
	<li>Last thing is just a test to show more monitoring data when many calls are exchanged between consumers and providers.</li>
</ol>
<div>[vimeo http://vimeo.com/32915151]</div>
<div></div>
<div>Let's go one step further... The BPEL engine we use in the DSB is our own (PetalsLink) BPEL engine we developed from scratch. This allows us to have a complete control on it and to be able to extend it and embed it as we want without any constraint. In the current case, the BPEL Engine is hosted on a dedicated DSB component. It means that we do not have an external thing which talk with services through some exposed services. This is really important to notice that by doing such thing we can really base process execution on a Service Oriented Architecture. When developing the BPEL process with the Petals Studio, or when creating a BPM process (more details in a future post), you do not have to care about service endpoints. You just have to say to the process that you want to call operation X of service Y or interface Z. It is up to the DSB hosting the BPEL engine to resolve endpoints at runtime. By using this approach we can really do interesting things, just because the DSB is Distributed: services can be hosted on any nodes, can be replicated, can move, can be updated without any impact on the process itself: Oh wait this is SOA!</div>
<div></div>
Related articles
<ul class="zemanta-article-ul">
	<li class="zemanta-article-ul-li"><a href="http://chamerling.org/2011/11/28/back-from-ow2con-2011/">Back From OW2Con 2011</a> (chamerling.org)</li>
	<li class="zemanta-article-ul-li"><a href="http://chamerling.org/2011/09/13/some-play-framework-service-bus-ws-notification-and-web-sockets/">Some Play Framework, Service Bus, WS Notification and Web Sockets...</a> (chamerling.org)</li>
	<li class="zemanta-article-ul-li"><a href="http://chamerling.org/2011/10/11/lets-talk-to-ow2con-2011/">Let's talk at OW2Con 2011</a> (chamerling.org)</li>
</ul>
