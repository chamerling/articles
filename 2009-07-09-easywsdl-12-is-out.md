---
layout: post
title: EasyWSDL 1.2 is out
categories:
- SOA
- WebService
tags:
- easywsdl
- release
- SOA
- WebService
- wsdl
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _elasticsearch_indexed_on: '2009-07-09 07:26:20'
---
OW2 EasyWSDL WSDL manipulation library has just been released. This release fix many bugs and introduces some new tooling such as java2wsdl (create WSDL from Java interfaces) and xsd2xml (create XML from XSD definition).

EasyWSDL is easy since you can manipulate both WSDL 1.1 and WSDL 2.0 with the same object model.
[sourcecode language="java"]
// Read a WSDL 1.1 or 2.0
WSDLReader reader = WSDLFactory.newInstance().newWSDLReader();
Description desc = reader.readWSDL(new URI(&quot;http://file/path/document.wsdl&quot;));

// Write a WSDL 1.1 or 2.0 (depend of desc version)
Document doc = WSDLFactory.newInstance().newWSDLWriter().getDocument(desc);

// Create a WSDL 1.1 or 2.0
Description desc11 = WSDLFactory.newInstance().newDescription(WSDLVersionConstants.WSDL11);
Description desc20 = WSDLFactory.newInstance().newDescription(WSDLVersionConstants.WSDL20);
[/sourcecode]
Once the Description object is loaded from the WSDL document, you can maniulate all parts of the service description. Let's look at endpoints :
[sourcecode language="java"]
// Endpoints take place in services.
// Select a service
Service service = desc.getServices().get(0);

// List endpoints
List endpoints = service.getEndpoints();

// Read specific endpoint
Endpoint specificEndpoint = service.getEndpoint(&quot;endpointName&quot;);

// Add endpoint to service
service.addEndpoint(specificEndpoint);

// Remove a specific enpoint
service.removeEndpoint(&quot;endpointName&quot;);

// Create endpoint
Endpoint createdEndpoint = service.createEndpoint();
service.addEndpoint(createdEndpoint);
[/sourcecode]
Easy!
