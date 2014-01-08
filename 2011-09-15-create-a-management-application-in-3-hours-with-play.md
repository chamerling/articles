---
layout: post
title: Create a management application in 3 hours with Play
categories:
- Java
tags:
- Application programming interface
- dsb
- esb
- GitHub
- Google Web Toolkit
- Java
- PEtALS
- petalslink
- Play Framework
- Service Bus
- SOA
- soap
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_mess: ! 'Create a management application in 3 hours with Play: http://wp.me/pcSx0-kP'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  tagazine-media: a:7:{s:7:"primary";s:68:"http://f.cl.ly/items/3T1p1z0r1W3F132X1D3V/dsbmanager-webapp-play.png";s:6:"images";a:1:{s:68:"http://f.cl.ly/items/3T1p1z0r1W3F132X1D3V/dsbmanager-webapp-play.png";a:6:{s:8:"file_url";s:68:"http://f.cl.ly/items/3T1p1z0r1W3F132X1D3V/dsbmanager-webapp-play.png";s:5:"width";s:4:"1086";s:6:"height";s:3:"541";s:4:"type";s:5:"image";s:4:"area";s:6:"587526";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-09-15
    09:28:53";}
  twitter_cards_summary_img_size: a:6:{i:0;i:1086;i:1;i:541;i:2;i:3;i:3;s:25:"width="1086"
    height="541"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}
  _elasticsearch_indexed_on: '2011-09-15 09:28:53'
---
<h1>Story, code, compare</h1>
Yet another ‘nightly project’ (thanks to current house build project and the lack of sleep it brings). This time I needed to be able to manage the so-famous Service Bus from some Web enabled tooling. I already developed such tool in a research project but the fact is that the licence of some libraries are not compatible with the petalslink open source approach. The second thing is that it is GWT based (which bores me, has almost 100 libraries dependencies and takes 10 minutes to compile). So this application is a good candidate to compare development productivity between GWT and the Play Framework. Here is a summary of the application creation:
<ul>
	<li>I bootstraped the application yesterday during lunch between 1PM and 2PM. I was already able to invoke most of the interesting service bus actions with the help of service bus SOAP API.</li>
	<li>I added some pages and actions last night, let say that since it was between 11PM and 1AM I was not very productive…</li>
	<li>I fixed some bugs this morning</li>
</ul>
As a result, I think I worked around 3 hours on this application. I think I spent one hour to resolve a dependency conflict between Play and a CXF dependency but as a result I have a good result which is almost equivalent in functionality to the GWT based application. I still miss some operations but I do not need them for now… I feel ashamed to say how long it tooks me to create the GWT version…
<h1>Deploy</h1>
Let’s talk about deployment… I did not have time to play with <a title="Heroku | Cloud Application Platform" href="http://www.heroku.com/">heroku</a> to push this application in the cloud. Tis will be a future step but since I use Play and git, I am able to push the code to github and then pull it on an OVH server I rent. I am able to provide this instance for some project partners if they want to manage the service bus. The time it took? 5 minutes (wget play, git clone and play start…).
<h1>Code</h1>
The Play enabled application is available on github at <a href="https://github.com/chamerling/dsbmanager-webapp">chamerling/dsbmanager-webapp</a>.

<img src="http://f.cl.ly/items/3T1p1z0r1W3F132X1D3V/dsbmanager-webapp-play.png" alt="" width="800" />
