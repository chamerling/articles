---
layout: post
title: SCA and PEtALS ESB, Yes We Can!
categories:
- PEtALS
tags:
- PEtALS
- SCA
- SOA
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-05-07 14:26:20'
---
This news has not been published on the <a href="http://petals.ow2.org">PEtALS Web Site</a> (why?) but yes we now have some SCA (Service Component Architecture) tools available with PEtALS ESB!

This work is a result of the <a href="http://www.scorware.org/" target="_blank">ScorWare</a> project in which eBM WebSourcing was involved in.

The SCA feature is provided in PEtALS by the SCA JBI Service Engine (<a href="http://forge.ow2.org/project/download.php?group_id=213&amp;file_id=12644">link to the component</a> and <a href="http://forge.ow2.org/project/download.php?group_id=213&amp;file_id=12645">link to the documentation</a>)
<blockquote>The main advantage of SCA over other approaches, like BPEL or EIP, is that you can define the composition with Java instead of XML. The SCA composite defines services, which are exposed in the bus. It also defines references, which point to services that might be called or used by the composite at runtime. These references define possible dependencies. And eventually, your composite embeds a Java implementation which allows you to manipulate the references as simple Java objects. This makes the user job easier, in particular to call a service operation or test conditions on a service call result.</blockquote>
For more details on SCA, on tools and more, take a look at the links below :
<ul>
	<li> <a href="http://www.osoa.org/display/Main/Service+Component+Architecture+Specifications">SCA</a> is a specification defined by the Open SOA consortium.</li>
	<li><a href="http://www.oasis-opencsa.org/committees">SCA</a> is in standardization process by the OASIS Consortium.</li>
	<li><a href="https://wiki.objectweb.org/frascati/Wiki.jsp?page=FraSCAti">OW2 FraSCAti</a> is the SCA platform the SCA service engine is based on.</li>
	<li><a href="http://wiki.eclipse.org/STP/SCA_Component">Eclipse SCA Tools</a> exist and are hosted by the SOA Tools Platform project. They can be used with the SCA service engine.</li>
	<li><a href="http://www.ebmwebsourcing.com/forum/topic30.html">PEtALS Eclipse tools</a> complete the STP SCA tools for PEtALS specifics (PEtALS Maven plug-in support, packaging for PEtALS...).</li>
</ul>
