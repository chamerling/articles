---
layout: post
title: CXF & Heroku, one more time...
categories:
- Cloud
- Java
tags:
- Apache CXF
- Apache Maven
- Heroku
- Java
- jaxws
- petalslink
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
  _wpas_mess: ! 'CXF & Heroku: Top chrono!'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _wpas_skip_tumblr: '1'
  _oembed_0f5f7db7708a8dd35a2a5e0a9cbad3dd: ! '{{unknown}}'
  _oembed_7cbad6982ed5012c2004912d0de17eb3: ! '{{unknown}}'
  _oembed_cb974eef7fe307e15b5f68aa11750013: ! '{{unknown}}'
  _oembed_5c1009278a16f74d3109bfc02cfdf55b: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2012-07-05 15:55:54'
---
One more <a class="zem_slink" title="Apache CXF" href="http://cxf.apache.org/" rel="homepage" target="_blank">Apache CXF</a> and <a class="zem_slink" title="Heroku" href="http://www.heroku.com/" rel="homepage" target="_blank">Heroku</a> article to push Web services to the Java PaaS... In some previous articles I explained how to create <a title="Pushing your Web services in the Cloud in 5 minutes…" href="http://chamerling.org/2011/11/21/pushing-your-web-services-in-the-cloud-in-5-minutes/" target="_blank">JAXWS</a> and <a title="Running JAXRS services in the Cloud in 5 minutes… or less!" href="http://chamerling.org/2012/06/28/running-jaxrs-services-in-the-cloud-in-5-minutes-or-less/" target="_blank">JAXRS</a> service by cloning/forking/whatever git repositories. This time it is almost the same but I created a <a class="zem_slink" title="Apache Maven" href="http://maven.apache.org/" rel="homepage" target="_blank">maven</a> archetype to generate tons of maven modules quickly and to integrate them in your maven-based projects (OK cloning a repository is faster, it does not download the entire Internet as Maven does...).

Let's do it with a screen record to check how fast it is. With my poor Internet connection and some typos, I have something running on Heroku in less than 2 min 30...

http://youtu.be/quuk-eIpnO8?hd=1

Here are the commands used in the sample above:

https://gist.github.com/3053446

The archetype source code is located at <a href="https://github.com/petalslink/petalscloud-maven-archetypes" target="_blank">https://github.com/petalslink/petalscloud-maven-archetypes</a> and deployed on <a href="http://repository.ow2.org/nexus/content/repositories/snapshots/org/ow2/petals/cloud/cxf-ws-heroku-archetype/" target="_blank">OW2 repository</a>. Once the project is generated from the archetype, one can add his own JAXWS-annotated services and associated Spring configuration (<em>src/main/webapp/WEB-INF/beans.xml</em>). Generated project is also available on github at <a href="https://github.com/chamerlingdotorg/maven-heroku-jaxws-sample" target="_blank">https://github.com/chamerlingdotorg/maven-heroku-jaxws-sample</a>.

Can it really be more easy?
