---
layout: post
title: ! 'One more JBI component : SOAP Proxy'
categories:
- PEtALS
- SOA
tags:
- Java
- jbi
- opensource
- ow2
- PEtALS
- SOA
- WebService
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _ideation_attached_images: '288'
  _oembed_573de767bf4765b0051d6fb43743d3c4: ! '{{unknown}}'
  _oembed_1c9e484c203a2da59c3e6547b2f5d3c8: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2010-02-02 14:38:22'
---
I just developped a new JBI component for <a href="http://petals.ow2.org">Petals ESB</a>. This component, named 'petals-bc-soapproxy' is based on the work I did on the standard SOAP binding component and provide an easy way to proxify SOAP based Web services with Petals ESB.

This component, based on Apache Axis2, uses a small part of WS-Addressing to provide the proxy feature. 'Small part' means that for now, by using Axis2, I need to bypass the mustUnderstand attribute of the WS-Addressing elements. Why? Because I need to use the WSA:To element to specify the Web service I want to finally invoke and I need to set the mustUnderstand attribute to false to bypass one implementation detail on Axis2 which is not 'friendly' with my own implementation. Why (again)?... By setting some mustUnderstand attribute to true in the SOAP message header, it means that the SOAP engine (ie Axis2) must understand the header during message processing. A mustUnderstand attribute set to true in the WS-Addressing header means that the SOAP engine must handle the WS-Addressing information and here I do not want to. If I set this attribute to true, I need to add the Axis2 addressing module, which will handle the WS-Addressing header, and will put all the addressing information in the Axis2 message context. I can do that and get the information from this context for for some reason it does not work like I expect... Maybe an Axis2 update will fix this...Need some time to investigate, and for now time is not my friend... So, here is how it work :

<strong>1.</strong> The SOAP client sends the SOAP message to the Petals SOAP Proxy (by default this service is exposed at http://localhost:8085/petals/services/SOAPProxyWebService). The SOAP message header contains the WS-Addressing information which will be used to reach the final Web service. A SOAP message sample is for example :

[sourcecode language="xml"]

&lt;soapenv:Envelope xmlns:soapenv=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot; xmlns:wsap=&quot;http://wsapi.petals.dsb.soa4all.eu/&quot;&gt;
&lt;soapenv:Header xmlns:wsa=&quot;http://www.w3.org/2005/08/addressing&quot;&gt;
&lt;wsa:Action&gt;http://wsapi.petals.dsb.soa4all.eu/HelloService/sayHello&lt;/wsa:Action&gt;
&lt;wsa:To&gt;http://localhost:7600/petals/ws/HelloService&lt;/wsa:To&gt;
&lt;/soapenv:Header&gt;
&lt;soapenv:Body&gt;
&lt;wsap:sayHello&gt;
&lt;!--Optional:--&gt;
&lt;arg0&gt;Say hello!&lt;/arg0&gt;
&lt;/wsap:sayHello&gt;
&lt;/soapenv:Body&gt;
&lt;/soapenv:Envelope&gt;

[/sourcecode]

where http://localhost:7600/petals/ws/HelloService is the final Web service I want to invoke.

<strong>2.</strong> The SOAP message is handled by the SOAP proxy component, transformed as JBI message and sent to the right JBI endpoint.

<strong>3.</strong> The JBI endpoint gets the WS-Addressing information from the JBI message, the SOAP message from the JBI message payload and send all of this stuff to the final Web service. Invokcation response is sent back to the initial consumer through the ESB.

A simple schema of what happens with three ESB nodes is given in the figure below :

<a href="http://chamerling.files.wordpress.com/2010/02/diapositive5.jpg"><img class="aligncenter size-full wp-image-288" title="Diapositive5" src="http://chamerling.files.wordpress.com/2010/02/diapositive5.jpg" alt="" width="500" height="375" /></a>

So, the main question is why using such a proxy and why not invoking the service directly? There are many answers to these questions and since I already said that the time is a missing resource, it will be probably part of a future article...

The first version of the component is available on the OW2 SVN repository under my sandbox (have a look to <a href="http://websvn.ow2.org/listing.php?repname=petals&amp;path=%2Fsandbox%2Fchamerling%2Fproxy%2Fpetals-bc-soap%2F#path_sandbox_chamerling_proxy_petals-bc-soap_">http://websvn.ow2.org/listing.php?repname=petals&amp;path=%2Fsandbox%2Fchamerling%2Fproxy%2Fpetals-bc-soap%2F#path_sandbox_chamerling_proxy_petals-bc-soap_</a>)

Note : Using <a href="http://soapui.org">SOAPUI</a> for WS-Addressing testing works fine (even if I had some exchanges with SOAPUI guy (http://twitter.com/olensmar) on Twitter...)... Sorry to have doubts on SOAPUI!
