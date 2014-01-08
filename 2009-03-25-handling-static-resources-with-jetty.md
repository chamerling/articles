---
layout: post
title: Handling static resources with Jetty
categories:
- Java
tags:
- Java
- jetty
status: publish
type: post
published: true
meta:
  _oembed_742c7cac83007fb3a17cab0dbb4cd06d: ! '{{unknown}}'
  _oembed_f70128bb43a4aab354337314ef63b7be: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2009-03-25 10:45:23'
---
Here is a quick (and easy) tip on how to handle static resources with Jetty6 (this is really nice and quick to setup such things for a demo ;o)). Here is the code :
<pre>package foo.bar;

import org.mortbay.jetty.Server;
import org.mortbay.jetty.handler.ContextHandlerCollection;
import org.mortbay.jetty.nio.SelectChannelConnector;
import org.mortbay.jetty.servlet.Context;

/**
 * @author Christophe HAMERLING - eBM WebSourcing
 *
 */
public class JettyServer {

    public static void main(String[] args) throws Exception {
        Server server = new Server();
        final ContextHandlerCollection contexts = new ContextHandlerCollection();
        final Context context = new Context(contexts, "/", Context.SESSIONS);
        context.setResourceBase(System.getProperty("user.home"));
        context.addServlet("org.mortbay.jetty.servlet.DefaultServlet", "/");

        final SelectChannelConnector nioConnector = new SelectChannelConnector();
        nioConnector.setPort(1978);
        server.addConnector(nioConnector);
        server.setHandler(contexts);
        server.start();
    }
}</pre>
This will display your home directory on <a href="http://localhost:1978">http://localhost:1978</a>

Cheers
