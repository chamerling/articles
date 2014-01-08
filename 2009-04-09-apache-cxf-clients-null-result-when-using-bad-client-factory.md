---
layout: post
title: ! 'Apache CXF Clients : null result when using bad client factory'
categories:
- WebService
tags:
- cxf
- Java
- WebService
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-04-09 13:12:06'
---
Just a quick note to show that the client factory choice is important with Apache CXF 2.1.4.

<strong>The service interface definition :</strong>
<pre>
package org.ow2.petals.usecase.soapaddressing.server;

import javax.jws.WebMethod;
import javax.jws.WebService;

/**
 * @author chamerling - eBM WebSourcing
 *
 */
@WebService
public interface AddressingService {

    /**
     * Get the local information
     *
     * @return
     */
    @WebMethod
    String getInfo();

}
</pre>

<strong>The client with the bad client factory :</strong>
<pre>
package org.ow2.petals.usecase.soapaddressing.client;

import org.apache.cxf.frontend.ClientProxyFactoryBean;
import org.ow2.petals.usecase.soapaddressing.server.AddressingService;

/**
 * @author chamerling - eBM WebSourcing
 *
 */
public class BadFactory {

    /**
     * @param args
     */
    public static void main(String[] args) {
        ClientProxyFactoryBean factory = new ClientProxyFactoryBean();
        factory.setServiceClass(AddressingService.class);
        factory.setAddress("http://localhost:8084/petals/services/Service01");
        AddressingService client = (AddressingService) factory.create();
        String info = client.getInfo();
        System.out.println(info);
    }
}
</pre>

With that client code, the Web Service is really invoked, but the result of the getInfo operation is null. So, let's do it with the right client factory...

<strong>The client with the good client factory :</strong>
<pre>
package org.ow2.petals.usecase.soapaddressing.client;

import org.apache.cxf.jaxws.JaxWsProxyFactoryBean;
import org.ow2.petals.usecase.soapaddressing.server.AddressingService;

/**
 * @author chamerling - eBM WebSourcing
 *
 */
public class GoodFactory {

    /**
     * @param args
     */
    public static void main(String[] args) {
        JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
        factory.setServiceClass(AddressingService.class);
        factory.setAddress("http://localhost:8084/petals/services/Service01");
        AddressingService client = (AddressingService) factory.create();
        String info = client.getInfo();
        System.out.println(info);
    }
}
</pre>

This time the result is not null and is really the one expected...
