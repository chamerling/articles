---
layout: post
title: Playing in Brussels
categories:
- Play
tags:
- Complex Event Processing
- dsb
- eda
- esb
- European Commission
- Pachube
- PEtALS
- petalslink
- play
- Service Level Agreement
- SOA
- twitter
- Web service
- wsn
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-11-03
    11:05:26";}
  _wpas_mess: ! 'Playing in Brussels: http://wp.me/pcSx0-lP'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _elasticsearch_indexed_on: '2011-11-03 11:05:26'
---
Last week was the first annual review meeting of the <a href="http://www.play-project.eu/" target="_blank">Play project</a> I work on since one year. I am involved at several levels in this project: from the architecture point of view, to the software integration and quality ones. On my side, my goal is to provide the efficient software infrastructure for events actors, or how to build an Event Driven Architecture based on Petals Distributed Service Bus. There are others points which have to be developed, especially all the platform governance stuff and Service Level Agreement for events, what we call Event Level Agreement.

We showed several things to the European Commission reviewers and we were also able to show an early prototype (this one was originally planned to be show at mid project ie in 6 months...). I made a video capture of the demo, which really needs to be explained...

[vimeo http://vimeo.com/31491959]
<ul>
	<li>There is the idea of a market place for events : The event marketplace. From there users are able to subscribe to things they are interested in. For now it is just a simple Web application which subscribe on behalf of the user to events through topics. This subscription is sent to the Play paltform using Web standards. Here we use OASIS Web service Notification to create this communication link.</li>
	<li>Events are collected from several sources by events adapters. In the video above, we can see the user setting this FB status which is collected by the Play system and transformed into a notification which is published to the platform. There are also some <a class="zem_slink" title="Twitter" href="http://twitter.com" rel="homepage">Twitter</a> adapters and <a class="zem_slink" title="Pachube" href="http://www.pachube.com/" rel="homepage">Pachube</a> ones which collects data in real time and publish them to the platform. This time again, we use Web standards for adapters communication.</li>
	<li>Now, what happens in the video? The user logs in to event marketplace and subscribes to FB events. The user then publishes something to its FB wall (note that it is not mandatory to have the same user in the event marketplace and in FB. A user can subscribe to FB events and receive events from all the FB statuses collected from the FB application users). After the event propagation delay, the event marketplace display the event to the user.</li>
</ul>
So do we need such machinery to do things like that? No, if you want to do some other simple mashup portal. There are several components which are under active development: Storage and processing. Yes we store all events in the Play platform. This storage will be huge, but it is one of the project goal: Providing efficient and elastic storage in some P2P way. The need for this storage comes with the other important aspect of the project: Complex Event Processing. We will soon be able to create complex rules on top of events and be able to generate notifications based on past and real time notifications because we have efficient storage and real time stuff inside the platform. I am not an expert of this domain, so I can not give more details about that point but capabilities are huge! For example, we can express something like "Hey Play, can you send me a SMS me when there is my favorite punk rock band playing just around and I am not on a business trip and X and !Y or Z". All of this intelligence coming from the processing of various sources I push since months in the platform coming from Twitter, FB, last.fm and other data providers.

Now let's take some time to work on my OW2Con talk. The session name is pretty cool : <a href="http://www.ow2.org/view/OW2Con-2011/Sessions_Cloud_Summit" target="_blank">Open Cloud Summit Session</a>.
