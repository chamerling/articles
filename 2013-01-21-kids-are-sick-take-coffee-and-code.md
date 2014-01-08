---
layout: post
title: Kids are sick? Take coffee and code
categories:
- Node
tags:
- api
- Application programming interface
- expressjs
- Heroku
- Hypertext Transfer Protocol
- JavaScript
- JSON
- Node.js
- nodejs
- ow2
- Representational State Transfer
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
  _wpas_mess: Kids are sick. Take coffee and code during nights.
  _wpas_skip_64254: '1'
  _wpas_skip_64253: '1'
  _wpas_skip_21153: '1'
  _oembed_4878b051792c1b34d05c8460c2696229: ! '{{unknown}}'
  _oembed_486c0d7dde58ba0af604cc50dea325dc: ! '{{unknown}}'
  _oembed_54891114be15692ee8c6af0e4754ed81: ! '{{unknown}}'
  _oembed_3e20db0df289bc5047fe643e5719e69f: ! '{{unknown}}'
  _oembed_4b2406eb7f0f7276285018fe8c1bcda8: ! '{{unknown}}'
  _oembed_98c3233c76c794995e8e3febeec15efa: ! '{{unknown}}'
  _oembed_60d517915c20641bd5391e408fbe8b72: ! '{{unknown}}'
  _oembed_db5698b11f0481bb87f6810f863ae93c: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2013-01-21 08:53:50'
---
I had some time since several nights (thanks kids to be sick...) to start watching how to implement a REST API using <a href="http://nodejs.org" target="_blank">node.js</a> (my new friend).

I plan to create an <a href="http://ow2.org" target="_blank">OW2</a> API this year; but before I started to wrap some existing services as proof of concept. The first feature focuses on how to get projects data from the gitorious instance we run at OW2. There is no clear API for gitorious but after some googling and source code search, it looks that there is a read-only XML API. Since I am targeting node.js, let's translate this API into a JSON one: <a href="https://npmjs.org/package/xml2js" target="_blank">xml2js</a> will do the job.

This results in a first library (<a href="https://npmjs.org/package/gitoriou.js" target="_blank">gitoriou.js</a>) which just call the gitorious instance and translates the XML data into JSON without any other data mapping. The following gist is showing how to get information from a project:

https://gist.github.com/4584620

I talked about having a REST API, let's use <a href="http://expressjs.com" target="_blank">express.js</a> for that and let's run it on <a class="zem_slink" title="Heroku" href="http://www.heroku.com/" target="_blank" rel="homepage">Heroku</a> (this platform rules, just took 2 minutes to create, deploy and run the service and it is up at <a href="http://ow2apisample.herokuapp.com/" target="_blank">http://ow2apisample.herokuapp.com/</a>).

https://gist.github.com/4555805

To get information (as JSON) about the Jasmine project, just HTTP GET <a href="http://ow2apisample.herokuapp.com/project/ow2-jasmine" target="_blank">http://ow2apisample.herokuapp.com/project/ow2-jasmine</a>

If you master any scripting language, you will be able to get all the repositories URLs from the JSON response, if not you can use the <a href="https://npmjs.org/package/ow2git" target="_blank">ow2git</a> project to clone them all. Assuming node.js and npm are available on your system (you already have java, ruby and every other stuff installed, why not adding node?), you can install the binary:
<blockquote>npm install -g ow2git</blockquote>
and then clone all the repositories from any gitorious project
<blockquote>ow2git --clone ow2-jasmine</blockquote>
Will clone all the ow2-jasmine repositories into your current folder.

I am really impressed by node.js runtime, tools and community. Even if all of this is just a proof of concept, not really well designed, you can have something running quickly without many effort. Time to get something real. Soon...
