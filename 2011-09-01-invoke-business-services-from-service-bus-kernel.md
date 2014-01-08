---
layout: post
title: Invoke business services from service bus kernel
categories:
- Java
- PEtALS
tags:
- Client (computing)
- Java
- Petals ESB
- petalslink
- Programming
- Service-oriented architecture
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
  tagazine-media: a:7:{s:7:"primary";s:60:"http://f.cl.ly/items/3Z1q2T301W0m0c1i0S0V/invokebusiness.png";s:6:"images";a:1:{s:60:"http://f.cl.ly/items/3Z1q2T301W0m0c1i0S0V/invokebusiness.png";a:6:{s:8:"file_url";s:60:"http://f.cl.ly/items/3Z1q2T301W0m0c1i0S0V/invokebusiness.png";s:5:"width";s:3:"659";s:6:"height";s:3:"476";s:4:"type";s:5:"image";s:4:"area";s:6:"313684";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-09-01
    14:19:32";}
  _wpas_mess: ! 'Invoke business services from service bus kernel:'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_done_linkedin: '1'
  _elasticsearch_indexed_on: '2011-09-01 14:19:32'
  twitter_cards_summary_img_size: a:6:{i:0;i:659;i:1;i:476;i:2;i:3;i:3;s:24:"width="659"
    height="476"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}
---
<h1>Let's introduce what it means...</h1>
While Petals ESB does not provide any solution to invoke services directly from the kernel/component Java code, the DSB now provides a solution for that... By using this DSB feature, we can really use the power of service oriented architecture directly in our code. No more direct calls to services, you just need to call the client and say which service you want to invoke. The service bus will resolve the endpoint and route the message to the right place by using the right way/path. By using this way, you do not have to care if you need to speak with a SOAP service, a JMS queue or a REST service: Just call the service, the DSB will do the rest.
<h1>Now some code...</h1>
So, let's have a look on the client code and first, let's imagine that you have a web service which is bound to the service bus. Once bound, a DSB endpoint is available inside the DSB, and all messages which are sent to this endpoint will be forwarded to the final web service. One basic approach is to create a web service hosted by the service bus which 'consumes' the DSB service. As a result, a SOAP message sent to the DSB web service will be forwarded to the DSB endpoint which will forward it again to the final web service. Allright, but this is just for external SOAP clients and it is impossible to invoke anything firectly from the service bus kernel.
Now, let's call the DSB endpoint from Java code... To put ourselves in a **real** case, let say that you are DSB kernel developer and that you want to say hello to the world (oh s*** really?) by using the *HelloWorldService* which is bound to the bus. Let's code this client:

<a href="https://gist.github.com/1186183" target="_blank">https://gist.github.com/1186183</a>

So, just get a *org.petalslink.dsb.service.client.Client* from the *org.petalslink.dsb.service.client.ClientFactory* factory, create your *org.petalslink.dsb.service.client.Message* and send it by using the *sendReceive* method. That's all... Let's try to clarify what it means with a simple schema:

<img src="http://f.cl.ly/items/3Z1q2T301W0m0c1i0S0V/invokebusiness.png" alt="" />
<h1>Hacker inside!</h1>
It looks so simple... I just <strong>deeply</strong> hacked the petals service bus kernel to provide such a thing. Why? Because the service bus is also deeply based on the JBI specification: Creating a client means that the client need to be able to receive responses and acts itself as a service on the service bus point of view. It is also built using all the component context stuff. No component context means no way to send/receive messages (component contexts are just created when JBI components are installed).
So it is really important to release the client when you do not use it anymore: There are tons of listeners and threads in the different layers which are just used to receive messages. If you do not release the client, there will be unused resources for a long time.
