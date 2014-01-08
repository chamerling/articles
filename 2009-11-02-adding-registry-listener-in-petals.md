---
layout: post
title: Adding Registry Listener in PEtALS
categories:
- PEtALS
tags:
- fractal
- ow2
- PEtALS
- SOA
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _wpas_done_yup: '1'
  _elasticsearch_indexed_on: '2009-11-02 09:11:58'
---
I just added a new feature to petals ESB (to be released quite soon in v3.0) in order to customize the Service Bus endpoint registration and unregistration...

The interface to implement is defined in <em>org.ow2.petals.jbi.messaging.registry.RegistryListener</em> such as :

[sourcecode language="java"]
package org.ow2.petals.jbi.messaging.registry;

import org.ow2.petals.jbi.messaging.endpoint.ServiceEndpoint;

public interface RegistryListener {

void onRegister(ServiceEndpoint endpoint);

void onUnregister(ServiceEndpoint endpoint);

}
[/sourcecode]

Note that for now, the listeners are just called for local endpoints (remote endpoints listeners will be added in a future version). In order to add registry listeners, you just have to implement the RegistryListener interface and to add the listeners to the registry in the Fractal configuration file (<em>JBI-Messaging.fractal</em>).

In order to illustrate how to add a listener in the Fractal configuration file, let's take an example. All the following lines are added to the <em>JBI-Messagging.fractal</em> configuration file.

1. Add a new component implementation which implements the RegistryListener interface :

[sourcecode language="xml"]
&lt;component definition=&quot;org.ow2.petals.kernel.registry.listener.wsdm.MonitoringNotifierImpl&quot; name=&quot;MonitoringNotifierImpl&quot;/&gt;
[/sourcecode]

2. Bind the new component with required ones (depends on the implementation) :

[sourcecode language="xml"]
&lt;binding client=&quot;MonitoringNotifierImpl.configuration&quot; server=&quot;this.configuration&quot;/&gt;
[/sourcecode]

3. Add the listener to the EndpointRegistry component :

[sourcecode language="xml"]
&lt;binding client=&quot;EndpointRegistryImpl.listener-wsdmnotif&quot; server=&quot;MonitoringNotifierImpl.service&quot;/&gt;
[/sourcecode]

Note that the binding client name MUST start with the "<em>listener-</em>" prefix in order to be added to the list of listeners.

Here we are, on endpoint registration and unregistration, the list of listeners will be called by the endpoint registry component. You can now specialize your endpoints lifecycle. A real use case will come in a future post, so stay tuned.
<div><span style="font-family:Monaco, 'Times New Roman', 'Bitstream Charter', Times, serif;font-size:small;">
</span></div>
