---
layout: post
title: Adding Routing modules in Petals ESB
categories:
- PEtALS
tags:
- fractal
- Java
- module
- ow2
- PEtALS
- routing
- SOA
- tip
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _wpas_done_yup: '1'
  _oembed_6d44413d683364e60fc22386e4636400: ! '{{unknown}}'
  _oembed_b2a72a76700d889797dcfb78c4e41d32: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2010-01-21 14:01:46'
---
<a href="http://petals.ow2.org">Petals ESB</a> is based on a highly modular architecture built using software components based on the <a href="http://fractal.ow2.org">Fractal</a> component model.

In a previous article about WSDM monitoring (<a href="http://chamerling.wordpress.com/2009/11/04/petals-esb-live-monitoring-with-wsdm-and-gwt/">http://chamerling.wordpress.com/2009/11/04/petals-esb-live-monitoring-with-wsdm-and-gwt/</a>), I was speaking about adding routing modules to the ESB router to intercept messages and to notify the monitoring system that messages are transmitted and received between service consumers and providers.

In the current article, I am going to show you how you can add modules to the routing layer of the service bus. In short, the routing layer is the component which select the right endpoint(s) to send the current message to. The routing layer has been decomposed into routing modules which are all called when a message is sent or received.

There are two types of routing modules the SenderModule (org.ow2.petals.jbi.messaging.routing.module.SenderModule) and the ReceiverModule (org.ow2.petals.jbi.messaging.routing.module.ReceiverModule) which both have right names ;). All the sender modules will be called on message emission, the receiver ones will be called at message reception... Here are the Java APIs :

<strong><em>org.ow2.petals.jbi.messaging.routing.module.SenderModule</em></strong>

[sourcecode language="java"]

package org.ow2.petals.jbi.messaging.routing.module;

import java.util.Map;
import org.ow2.petals.jbi.component.context.ComponentContext;
import org.ow2.petals.jbi.messaging.endpoint.ServiceEndpoint;
import org.ow2.petals.jbi.messaging.exchange.MessageExchange;
import org.ow2.petals.jbi.messaging.routing.RoutingException;
import org.ow2.petals.transport.util.TransportSendContext;

public interface SenderModule {

void electEndpoints(final Map&lt;ServiceEndpoint, TransportSendContext&gt; electedEndpoints,

final ComponentContext sourceComponentContext, final MessageExchange exchange)

throws RoutingException;

}

[/sourcecode]

<strong><em>org.ow2.petals.jbi.messaging.routing.module.ReceieverModule</em></strong>

[sourcecode language="java"]

package org.ow2.petals.jbi.messaging.routing.module;

import org.ow2.petals.jbi.component.context.ComponentContext;
import org.ow2.petals.jbi.messaging.exchange.MessageExchange;
import org.ow2.petals.jbi.messaging.routing.RoutingException;

public interface ReceiverModule {

boolean receiveExchange(final MessageExchange exchange,

final ComponentContext sourceComponentContext) throws RoutingException;

}

[/sourcecode]

Implementing and adding modules to the Petals ESB router steps

1. Step one : Create your routing module by implementing the Sender and/or Receiver module interface(s) like, for example, just logging exchange information and date :

[sourcecode language="java"]

package com.googlecode.chamerling.blog.petals.router;

import java.util.Date;

import java.util.Map;

import org.objectweb.fractal.fraclet.annotation.annotations.FractalComponent;

import org.objectweb.fractal.fraclet.annotation.annotations.Interface;

import org.objectweb.fractal.fraclet.annotation.annotations.LifeCycle;

import org.objectweb.fractal.fraclet.annotation.annotations.Monolog;

import org.objectweb.fractal.fraclet.annotation.annotations.Provides;

import org.objectweb.fractal.fraclet.annotation.annotations.type.LifeCycleType;

import org.objectweb.util.monolog.api.Logger;

import org.ow2.petals.jbi.component.context.ComponentContext;

import org.ow2.petals.jbi.messaging.endpoint.ServiceEndpoint;

import org.ow2.petals.jbi.messaging.exchange.MessageExchange;

import org.ow2.petals.jbi.messaging.routing.RoutingException;

import org.ow2.petals.jbi.messaging.routing.module.ReceiverModule;

import org.ow2.petals.jbi.messaging.routing.module.SenderModule;

import org.ow2.petals.transport.util.TransportSendContext;

import org.ow2.petals.util.LoggingUtil;

@FractalComponent

@Provides(interfaces = {

@Interface(name = &quot;logSender&quot;, signature = SenderModule.class),

@Interface(name = &quot;logReceiver&quot;, signature = ReceiverModule.class) })

public class LogModule implements SenderModule, ReceiverModule {

@Monolog(name = &quot;logger&quot;)

private Logger logger;

private LoggingUtil log;

/**

* {@inheritDoc}

*/

public void electEndpoints(

Map&lt;ServiceEndpoint, TransportSendContext&gt; electedEndpoints,

ComponentContext sourceComponentContext, MessageExchange exchange)

throws RoutingException {

this.log.info(LogModule.class.getCanonicalName()

+ &quot; router module is sending a message exchange with ID = &quot;

+ exchange.getExchangeId() + &quot; at &quot; + new Date());

for (ServiceEndpoint endpoint : electedEndpoints.keySet()) {

this.log.info(&quot;Selected endpoint : &quot; + endpoint.getEndpointName());

}

// Do what you want...

}

/**

* {@inheritDoc}

*/

public boolean receiveExchange(MessageExchange exchange,

ComponentContext sourceComponentContext) throws RoutingException {

this.log.info(LogModule.class.getCanonicalName()

+ &quot; router module is receiving a message exchange with ID = &quot;

+ exchange.getExchangeId() + &quot; at &quot; + new Date());

// Do what you want...

return false;

}

@LifeCycle(on = LifeCycleType.START)

protected void start() {

this.log = new LoggingUtil(this.logger);

this.log.debug(&quot;Starting...&quot;);

}

@LifeCycle(on = LifeCycleType.STOP)

protected void stop() {

this.log.debug(&quot;Stopping...&quot;);

}

}

[/sourcecode]

Note that the Fractal annotations are mandatory!

2. Add the modules to the Fractal descriptor files. The modules MUST be added to the JBI-Messaging.fractal configuration file (located in the src/main/resources of your favorite Petals ESB distribution) :

[sourcecode language="xml"]&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;

&lt;!DOCTYPE definition PUBLIC &quot;-//ow2.objectweb//DTD Fractal ADL 2.0//EN&quot; &quot;classpath://org/objectweb/fractal/adl/xml/standard.dtd&quot;&gt;

&lt;definition extends=&quot;JBI-MessagingType&quot; name=&quot;JBI-Messaging&quot;&gt;

&lt;component definition=&quot;org.ow2.petals.jbi.messaging.registry.DistributedEndpointRegistryImpl&quot; name=&quot;EndpointRegistryImpl&quot;/&gt;

&lt;component definition=&quot;org.ow2.petals.jbi.messaging.routing.RouterServiceImpl&quot; name=&quot;RouterServiceImpl&quot;/&gt;

&lt;component definition=&quot;org.ow2.petals.jbi.messaging.routing.module.TransportResolverModule&quot; name=&quot;TransportResolverModule&quot;/&gt;

&lt;component definition=&quot;org.ow2.petals.jbi.messaging.routing.module.EndpointResolverModule&quot; name=&quot;EndpointResolverModule&quot;/&gt;

&lt;component definition=&quot;org.ow2.petals.jbi.messaging.control.JMXExchangeCheckerClientImpl&quot; name=&quot;ExchangeCheckerClientImpl&quot;/&gt;

&lt;!-- My logging module --&gt;
&lt;component definition=&quot;com.googlecode.chamerling.blog.petals.router.LogModule&quot; name=&quot;LogModuleImpl&quot;/&gt;

&lt;!-- Expose --&gt;

&lt;binding client=&quot;this.router&quot; server=&quot;MonitoringModuleImpl.service&quot;/&gt;

&lt;binding client=&quot;this.transportlistener&quot; server=&quot;RouterServiceImpl.transportlistener&quot;/&gt;

&lt;binding client=&quot;this.endpoint&quot; server=&quot;EndpointRegistryImpl.service&quot;/&gt;

&lt;binding client=&quot;this.exchangechecker&quot; server=&quot;ExchangeCheckerClientImpl.service&quot;/&gt;

&lt;binding client=&quot;this.storage&quot; server=&quot;MonitoringStorageServiceImpl.service&quot;/&gt;

&lt;binding client=&quot;MonitoringModuleImpl.router&quot; server=&quot;RouterServiceImpl.service&quot;/&gt;

&lt;binding client=&quot;MonitoringModuleImpl.storageService&quot; server=&quot;MonitoringStorageServiceImpl.service&quot;/&gt;

&lt;binding client=&quot;EndpointRegistryImpl.configuration&quot; server=&quot;this.configuration&quot;/&gt;

&lt;binding client=&quot;EndpointRegistryImpl.topology&quot; server=&quot;this.topology&quot;/&gt;

&lt;!-- Add logging module to router --&gt;

&lt;binding client=&quot;RouterServiceImpl.sendermodule-3&quot; server=&quot;LogModuleImpl.logSender&quot;/&gt;

&lt;binding client=&quot;RouterServiceImpl.receivermodule-1&quot; server=&quot;LogModuleImpl.logReceiver&quot;/&gt;

&lt;!-- router --&gt;

&lt;binding client=&quot;RouterServiceImpl.transporter-local&quot; server=&quot;this.transporter-local&quot;/&gt;

&lt;binding client=&quot;RouterServiceImpl.transporter-tcp&quot; server=&quot;this.transporter-tcp&quot;/&gt;

&lt;!--the order of collection of bindings is alphabetically inversed --&gt;

&lt;binding client=&quot;RouterServiceImpl.sendermodule-2&quot; server=&quot;EndpointResolverModule.service&quot;/&gt;

&lt;binding client=&quot;RouterServiceImpl.sendermodule-1&quot; server=&quot;TransportResolverModule.service&quot;/&gt;

&lt;binding client=&quot;EndpointResolverModule.configuration&quot; server=&quot;this.configuration&quot;/&gt;

&lt;binding client=&quot;EndpointResolverModule.topology&quot; server=&quot;this.topology&quot;/&gt;

&lt;binding client=&quot;EndpointResolverModule.endpoint&quot; server=&quot;EndpointRegistryImpl.service&quot;/&gt;

&lt;binding client=&quot;EndpointResolverModule.checker&quot; server=&quot;ExchangeCheckerClientImpl.service&quot;/&gt;

&lt;binding client=&quot;TransportResolverModule.configuration&quot; server=&quot;this.configuration&quot;/&gt;

&lt;binding client=&quot;ExchangeCheckerClientImpl.jmx&quot; server=&quot;this.jmx&quot;/&gt;
&lt;/definition&gt;
[/sourcecode]

Important things here are :

1. Instantiate the component with line

[sourcecode language="xml"]
&lt;component definition=&quot;com.googlecode.chamerling.blog.petals.router.LogModule&quot; name=&quot;LogModuleImpl&quot;/&gt;
[/sourcecode]

2. Adding the module to sender and receiver modules list :

[sourcecode language="xml"]

&lt;binding client=&quot;RouterServiceImpl.sendermodule-3&quot; server=&quot;LogModuleImpl.logSender&quot;/&gt;

&lt;binding client=&quot;RouterServiceImpl.receivermodule-1&quot; server=&quot;LogModuleImpl.logReceiver&quot;/&gt;

[/sourcecode]

Note that the lists are alphabetically inversed (The logger module "sendermodule-3" will be called before the sendermodule-2 and the sendermodule-1 modules).

Here we are, the module will be called at each message emission/reception. Note that this is a basic usage of routing modules, you can do many things with these modules like real time monitoring, security check, authentication, endpoint filtering, content based routing, etc etc etc
