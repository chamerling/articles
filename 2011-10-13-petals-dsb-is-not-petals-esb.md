---
layout: post
title: Petals DSB is not Petals ESB
categories:
- PEtALS
tags:
- dsb
- Enterprise service bus
- esb
- Java
- Petals ESB
- petalslink
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
  _wpas_mess: ! 'Petals DSB is not Petals ESB:'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _elasticsearch_indexed_on: '2011-10-13 10:33:59'
---
Almost true... In fact Petals DSB uses and extends <a href="http://petals.ow2.org" target="_blank">Petals ESB</a> in several ways. When I started to think about extending the Enterprise Service Bus, it was just to avoid all the JBI stuff at the management level i.e. use a real, simple and efficient API. So I added many management stuff exposed as Web services : Bind external services, expose internal services (oh yes bind + expose = proxy), get endpoints, activate things, etc...
One other goal was to avoid to use closed protocols for inter node communication: The ESB uses at least three ports to create inter node communications for JMX, NIO and SOAP. So why not, just using an open protocol like SOAP and just one port? This is what I did, I changed some implementations to use the same port and the same protocol for all services.

All this stuff has been developed focusing on extensibility and easier development. This is mainly because the ESB can be hard to extend for newbies, there are so many things inside... Today the DSB is not only a Distributed Service Bus, it is also a framework so that developers can easily extend the DSB without the need to know how it works inside. Some examples? Want to expose a kernel service : Add the JAXWS @WebService annotation. Want to subscribe to Web service notifications : Annotate you Java method with @Notify. Want to be notified about new endpoints : Add the @RegistryListener annotation. That's all, you do not have to search for the Web service server to expose your service, nor read all the WSN documentation to receive notifications, ... Simple, efficient.

There are other things you can do, mostly all is detailed in the <a href="http://research.petalslink.org/display/petalsdsb/How-tos" target="_blank">DSB page</a>.
