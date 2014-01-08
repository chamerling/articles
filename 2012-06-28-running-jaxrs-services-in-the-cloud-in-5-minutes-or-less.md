---
layout: post
title: Running JAXRS services in the Cloud in 5 minutes... or less!
categories:
- Cloud
- WebService
tags:
- Apache CXF
- Heroku
- Java
- jetty
- JSON
- petalslink
- soap
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
  _wpas_mess: Running JAXRS services in the Cloud in 5 minutes... or less!
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _wpas_skip_tumblr: '1'
  _oembed_4c55de56de0e2030a28b8fc80f716e2a: ! '{{unknown}}'
  _oembed_e25723b98705e02d911489fe558201ba: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2012-06-28 12:53:39'
---
Here is a really simple post about how to push REST services in the Cloud. Nothing really technical nor advanced, just some notes and sample using amazing tools CXF + Heroku...

Last time I was speaking about putting <a title="Pushing your Web services in the Cloud in 5 minutes…" href="http://chamerling.org/2011/11/21/pushing-your-web-services-in-the-cloud-in-5-minutes/">some SOAP Web services in the Cloud with Heroku</a>, this time it is the same with REST services... The approach is exactly the same but it uses the JAXRS implementation provided by <a class="zem_slink" title="Apache CXF" href="http://cxf.apache.org/" rel="homepage" target="_blank">Apache CXF</a>.

The REST service illustrates how to annotate the Java interface to returns JSON-based responses like:

https://gist.github.com/3011121

Once implemented and configured (it uses Spring with the famous WEB-INF/beans.xml file), pushing it to Heroku is as simple as last time, nothing new here. Heroku needs a Procfile to start, and the Maven-based project is configured to generate what the Procfile needs: A shell file which launches a Jetty instance running Apache CXF and all the REST stuff.

The code is located at <a href="https://github.com/chamerling/heroku-cxf-jaxrs">https://github.com/chamerling/heroku-cxf-jaxrs</a>. You can run it locally to test before pushing to heroku:
<ul>
	<li>mvn install</li>
	<li>sh target/bin/webapp</li>
</ul>
