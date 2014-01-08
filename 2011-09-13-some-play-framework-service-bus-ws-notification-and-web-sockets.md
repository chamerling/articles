---
layout: post
title: Some Play Framework, Service Bus, WS Notification and Web Sockets...
categories:
- Java
- SOA
tags:
- bus
- dsb
- esb
- GitHub
- Java
- PEtALS
- petalslink
- Play Framework
- service
- Service Bus
- Service-oriented architecture
- soap
- Web service
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
  _wpas_mess: ! 'Some Play Framework, Service Bus, WS Notification and Web Sockets...:'
  _wpas_skip_twitter: '1'
  _wpas_done_linkedin: '1'
  tagazine-media: a:7:{s:7:"primary";s:63:"http://f.cl.ly/items/182y1Q0e1g1D1u1L1d2C/playwsndsbsockets.jpg";s:6:"images";a:1:{s:63:"http://f.cl.ly/items/182y1Q0e1g1D1u1L1d2C/playwsndsbsockets.jpg";a:6:{s:8:"file_url";s:63:"http://f.cl.ly/items/182y1Q0e1g1D1u1L1d2C/playwsndsbsockets.jpg";s:5:"width";s:4:"1000";s:6:"height";s:3:"747";s:4:"type";s:5:"image";s:4:"area";s:6:"747000";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-09-13
    16:34:32";}
  twitter_cards_summary_img_size: a:7:{i:0;i:1000;i:1;i:747;i:2;i:2;i:3;s:25:"width="1000"
    height="747"";s:4:"bits";i:8;s:8:"channels";i:3;s:4:"mime";s:10:"image/jpeg";}
  _elasticsearch_indexed_on: '2011-09-13 16:34:32'
---
<h1>The Context</h1>
In the previous post I was introducing some tests I did with <a title="Play framework - Home" href="http://www.playframework.org/">Play Framework</a> and Web sockets. To summarize, it was just ‘about’ receiving messages on the Play! application and pushing them to the browser. This time, let’s go one step forward: Let’s add some infrastructure stuff to do something more real…

In the current case, I want to introduce a service bus which allows to create a real integration between service consumers and producers (I need to write another article in response to this nice videocast about integration from <a title="Zenexity - web oriented architecture" href="http://www.zenexity.com/">Zenexity</a> guys ‘<a title="EAI &amp; ESB n’apportent rien si les applications ne sont pas intégrables et interopérables - Zengularity" href="http://www.zengularity.com/item/1650674910/eai-esb-napportent-rien-si-les-applications-ne-sont">EAI &amp; ESB n'apportent rien si les applications ne sont pas intégrables et interopérables</a>’, but I really need more time to explain my thoughs…).
Using a service bus in the current case (and not for this case in fact…) must bring some added value. Here I choose to show that a service bus, even if it is not so lightweight at all, can provide some real cool features that you can have out of the box. Now, let’s add some event stuff to switch to an event-based world where we can have tons of event producers and let’s say thousands of event consumers. Since I can not setup such hude amount of actors, let’s say that we have one event producer and two event consumers:
<ul>
	<li>The event producer wants to publish some stuff somewhere. To illustrate, let’s say that we have a weather sensor connected to our platform.</li>
	<li>The platform provides a list of topics which producers can use to publish data. One is the weather topic which will be used by the producer above.</li>
	<li>The event consumers want to be notified on new weather data i.e. as soon as the weather sensor publish new data. To keep it simple, they need to subscribe to the weather topic provided by the platform.</li>
</ul>
To recap, in the event context, the event producer only knows the service he has to push weather data to, the event consumers just have to subscribe to a topic they are interested in. All the knowledge stuff about producers, consumers, topics and all the mapping is delgated at the service bus level. Yes, true it is exactly like in some topic-based messaging stuff because at the end of the day, it is topic-based messaging stuff…
<h1>The Stuff</h1>
Now we can speak about the stuff we are going to use in the software point of view for notificaiton actors…

<img src="http://f.cl.ly/items/182y1Q0e1g1D1u1L1d2C/playwsndsbsockets.jpg" alt="" />
<ul>
	<li>The event producer will not be a sensor but a Play! application. The application sends Web service notifications message to the service bus on a given topic.</li>
	<li>The service bus is (of course) the <a title="PetalsDSB Overview - PetalsDSB - Petalslink Research" href="http://research.petalslink.org/display/petalsdsb">Petals Distributed Service Bus</a> with some Web service notification modules inside.</li>
	<li>The event consumers are 1/A Play! application exposing a Web service to receive notification it subscribes to and 2/ A local java application displaying OS X notifications using Growl (let’s use <a href="https://github.com/chamerling/JavaGrowl">JavaGrowl</a> I published some days ago…). Note that the Play! application which have subscribed to notifications pushes them to their clients (browsers) using the funky Websocket stuff.</li>
</ul>
Let’s look at what really happens in a short video:

[vimeo 28992086 w=600 h=375]
<ul>
	<li>I use the Play! powered application <a href="https://github.com/chamerling/play-soap-wsnclient">play-soap-wsnclient</a> to subscribe to notification on behalf of the <a href="https://github.com/chamerling/play-soap-websockets">play-soap-websocket</a> Web application.</li>
	<li>In Eclipse, I start a Web service notification powered Web service which subscribes to the same notification topic. Its listener is configured to use JavaGrowl to display incoming notifications.</li>
	<li>I use <a href="https://github.com/chamerling/play-soap-wsnclient">play-soap-wsnclient</a> to send notification messages to the notification service hosted on the service bus.</li>
	<li>Once received, the service bus forwards the notification to all the subscribers using internal routing and WSN stuff.</li>
	<li>The <a href="https://github.com/chamerling/play-soap-websockets">play-soap-websocket</a> Web application receives a notification and push it to the client browser using Websocket.</li>
	<li>At the same time, the Java application also receives a notification and display it using Growl.</li>
</ul>
<h1>One (or more) step(s) further…</h1>
And what if we have something which is not a Web service which subscribes to notifications? With the help of a service bus like Petals ESB/DSB, we just have to add a component which knows how to speak with the subscriber. For example, let’s say that SOAP is bad and that REST is the best thing ever. Can we have REST services receiving notifications? Yes, we can! Let’s just add the REST connector to the service bus. Another protocol/transport/format? Develop and add the new one. This is where the service bus can also help you. Hopefully, there are also other things which are possible with a service bus, we will see it later in other posts if I have time (as usual): Let’s think about business processes, service orchestration, transformation…
Next time we will have a look on what we can do if we use the distributed feature of the service bus, for example, receiving some notifications on one node and be able to notify subscribers which are bound to other nodes…
<h1>Source Code</h1>
<ul>
	<li>The event producer Play! application is on GitHub at <a href="https://github.com/chamerling/play-soap-wsnclient">play-soap-wsnclient</a></li>
	<li>The event consumer Play! application is also on GitHub at <a href="https://github.com/chamerling/play-soap-websockets">play-soap-websockets</a></li>
	<li>The Distributed Service Bus is not on GitHub but is open sourced on <a title="PetalsDSB Overview - PetalsDSB - Petalslink Research" href="http://research.petalslink.org/display/petalsdsb">PetalsLinkLabs</a></li>
</ul>
