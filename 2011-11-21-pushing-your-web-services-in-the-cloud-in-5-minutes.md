---
layout: post
title: Pushing your Web services in the Cloud in 5 minutes...
categories:
- Cloud
- Java
tags:
- Apache CXF
- cloud
- cxf
- Git
- Heroku
- jaxws
- jetty
- petalslink
- Programming
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
  _wpas_mess: ! 'Pushing your Web services in the Cloud in 5 minutes...:'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-11-21
    22:31:45";}
  _oembed_ecd37b102bcf2f5fac4f8daf53bd85ca: ! '{{unknown}}'
  _oembed_10dc28a884cf31d6112754c243cb89b2: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2011-11-21 22:31:45'
---
... or less! <a class="zem_slink" title="Heroku" href="http://www.heroku.com/" rel="homepage">Heroku</a> is defined as a "Cloud application platform". I just want to redefine it to "Awesome Cloud application Platform". So, this awesome platform provides a way to host and scale your application in the Cloud really easily with 3 or 4 commands...

Since I am currently working on my talk at #<a href="http://www.ow2.org/view/OW2Con-2011/" target="_blank">OW2Con</a> 2011 (coming later this week) dealing with BPM, Services and the Cloud, I wanted to host some Web services on several places. I never had time to test Heroku but I just took this precious time today. After looking some examples, I created a Maven project template (no I do not have time to create an archetype, maybe there is one somewhere) which uses Jetty and Apache CXF to expose JAXWS annotated classes as Web services. So now, using heroku to freely expose your services is easy as:
<ol>
	<li> Sign up to heroku</li>
	<li>Download the heroku client for your platform</li>
	<li>Clone/Fork the repository at <a href="https://github.com/chamerling/heroku-cxf-jaxws" target="_blank">https://github.com/chamerling/heroku-cxf-jaxws</a></li>
	<li>Add your own services</li>
	<li>Login to heroku '<em>heroku auth:login</em>'</li>
	<li>Create the app on heroku '<em>heroku create -s cedar</em>'</li>
	<li>Push your services to heroku '<em>git push heroku master</em>'. There is a git hook somewhere which just automatically compile and start your application after you pushed it.</li>
	<li>Open your CXF services summary page 'heroku open'</li>
</ol>
<div>The default application name is some random one, you can rename it by using the '<em>heroku rename yournewname</em>' but in the current case I had an issue on the generated Web service endpoint name. So I suggest restarting your app after renaming (have a look to the '<em>heroku ps</em>' command).</div>
<div></div>
<div>That's all, that's quick!</div>
