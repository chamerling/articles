---
layout: post
title: Easily expose JAXWS in PEtALS with Maven
categories:
- PEtALS
tags:
- jaxws
- maven
- PEtALS
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-05-26 12:04:29'
---
Since I am always using command line tools such as Maven or Ant to create my project and to package them, I have just created a new Maven plugin to easily and quickly expose a JAXWS service in PEtALS with Maven. This plugin will generate the JBI Service Unit and Service Assembly from a Maven java project with just a few lines of Maven settings...

As an example, the following interface :
<pre>
package org.ow2.petals;

import javax.jws.WebMethod;
import javax.jws.WebService;

@WebService
public interface Service {

	@WebMethod
	String ping(String input);
}
</pre>
and its implementation :
<pre>
package org.ow2.petals;

public class ServiceImpl implements Service {
	public String ping(String input) {
		return input;
	}
}
</pre>

will generate a Service Assembly idirectly deployable to PEtALS by the help of the Maven plugin :
<pre>
&lt;build&gt;
	&lt;plugins&gt;
		&lt;plugin&gt;
			&lt;groupId&gt;org.ow2.petals&lt;/groupId&gt;
			&lt;artifactId&gt;maven-petals-jaxws2jbi&lt;/artifactId&gt;
			&lt;version&gt;1.0-SNAPSHOT&lt;/version&gt;
			&lt;executions&gt;
				&lt;execution&gt;
					&lt;id&gt;generate-jbi&lt;/id&gt;
					&lt;phase&gt;package&lt;/phase&gt;
					&lt;configuration&gt;
						&lt;className&gt;
							org.ow2.petals.ServiceImpl
						&lt;/className&gt;
					&lt;/configuration&gt;
					&lt;goals&gt;
						&lt;goal&gt;java2jbi&lt;/goal&gt;
					&lt;/goals&gt;
				&lt;/execution&gt;
			&lt;/executions&gt;
		&lt;/plugin&gt;
	&lt;/plugins&gt;
&lt;/build&gt;
</pre>

The plugin takes the Service class, generates its associated WSDL file and the JBI descriptor and then package all into a Service Assembly.
