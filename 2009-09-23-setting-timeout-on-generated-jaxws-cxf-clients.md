---
layout: post
title: Setting timeout on generated JAXWS CXF Clients
categories:
- WebService
tags:
- cxf
- jaxws
- timeout
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _elasticsearch_indexed_on: '2009-09-23 14:26:25'
---
When generating client with the CXF (2.2.2 in this case, should apply to all...) Java API without any configuration file, here is the way to set the client timeout :

[sourcecode language="java"]
package org.ow2.petals.kernel.ws.client;

import org.apache.cxf.endpoint.Client;
import org.apache.cxf.frontend.ClientProxy;
import org.apache.cxf.jaxws.JaxWsProxyFactoryBean;
import org.apache.cxf.transport.http.HTTPConduit;
import org.apache.cxf.transports.http.configuration.HTTPClientPolicy;
import org.ow2.petals.kernel.ws.api.RuntimeService;

public class Main {

    private void createService() {
        long timeout = 10000L;
        JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
        factory.setServiceClass(RuntimeService.class);
        factory.setAddress(&quot;http://localhost:9999/service/Runtime&quot;);
        RuntimeService runtimeService = (RuntimeService) factory.create();

        Client client = ClientProxy.getClient(runtimeService);
        if (client != null) {
            HTTPConduit conduit = (HTTPConduit) client.getConduit();
            HTTPClientPolicy policy = new HTTPClientPolicy();
            policy.setConnectionTimeout(timeout);
            policy.setReceiveTimeout(timeout);
            conduit.setClient(policy);
        }
    }
}
[/sourcecode]
<em>Note that here the RuntimeService class is my JAXWS annotated class.</em>
