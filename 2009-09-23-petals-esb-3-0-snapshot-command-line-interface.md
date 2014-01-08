---
layout: post
title: Petals ESB 3.0-SNAPSHOT Command Line Interface
categories:
- PEtALS
tags:
- PEtALS
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-09-23 14:50:57'
---
The current petals ESB snapshot comes with a new feature : A new command line interface.
This interface uses the petals ESB Web services which are now embedded within the container. There is no launcher for the CLI right now but it should come soon. For now, here are some snippets :

<pre>
Connecting to http://localhost:7600/petals/ws ...
PEtALS CLI. Tape 'help' for help.

/&gt; h
PEtALS prompt usage:
 - a, addart                Upload an artifact to the petals server
 - c, connect               Connects the client to a petals server
 - e, endpoint              Displays the list of endpoints
 - i, info                  Displays the container information
 - ic, install              Deals with the component installation and uninstallation
 - j, jbiart                Displays the list of JBI artefacts
 - q, stop                  Stops the container
 - r, repo                  Displays the petals repository content
 - t, topology              Displays the petals topology
 - x, shutdown              Shutdown the container

/&gt;
</pre>

<pre>
/&gt; t
 Displays the petals topology
==============================
 + Container #1
  - Name       : NODE-EBM-00
  - Address    : node00.ebmwebsourcing.com
 + Container #2
  - Name       : NODE-OVH-00
  - Address    : node00.ovh.net

/&gt; e
 Displays the list of endpoints
================================
 + Endpoint #1
  - Name       : ProductWebServicePort
  - Service    : {http://productws. partner.soa4all.com/}ProductWebServiceService
  - Interfaces : {http://productws. partner.soa4all.com/}ProductWebService,
  - Location (component / container / domain)   : petals-bc-soap / NODE-EBM-00 / subdomain1
 + Endpoint #2
  - Name       : IWebShopPort
  - Service    : {http://mamboofive.partner.soa4all.com/}IWebShopService
  - Interfaces : {http://mamboofive. partner.soa4all.com/}IWebShop,
  - Location (component / container / domain)   : petals-bc-soap / NODE-EBM-00 / subdomain1
 + Endpoint #3
  - Name       : HanivalProductWSPort
  - Service    : {http://hanivalproductws. partner.net/}HanivalProductWSService
  - Interfaces : {http://hanivalproductws. partner.net/}HanivalProductWS,
  - Location (component / container / domain)   : petals-bc-soap / NODE-EBM-00 / subdomain1
 + Endpoint #4
  - Name       : SemanticSpaceAggImplPort
  - Service    : {http://ws.aggregator.space.dsb.soa4all.eu/}SemanticSpaceAggImplService
  - Interfaces : {http://ws.aggregator.space.dsb.soa4all.eu/}SemanticSpaceWS,
  - Location (component / container / domain)   : petals-bc-soap / NODE-OVH-00 / subdomain1
 + Endpoint #5
  - Name       : SemanticSpaceWSImplServiceEndpointOVH
  - Service    : {http://ws.space.dsb.soa4all.eu/}SemanticSpaceWSImplService
  - Interfaces : {http://ws.space.dsb.soa4all.eu/}SemanticSpaceWS,
  - Location (component / container / domain)   : petals-bc-soap / NODE-OVH-00 / subdomain1
 + Endpoint #6
  - Name       : SemanticSpaceWSImplServiceEndpointEBM
  - Service    : {http://ws.space.dsb.soa4all.eu/}SemanticSpaceWSImplService
  - Interfaces : {http://ws.space.dsb.soa4all.eu/}SemanticSpaceWS,
  - Location (component / container / domain)   : petals-bc-soap / NODE-EBM-00 / subdomain1

/&gt;
</pre>

... will be extended with more services too.
