---
layout: post
title: Using Nexus as Proxy for corporate Open Source project
categories:
- Java
tags:
- Java
- maven
- pom
- proxy
- settings
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _wpas_done_yup: '1'
  _elasticsearch_indexed_on: '2009-10-12 10:10:15'
---
The problem when working on an Open Source project which is hosted on a consortium (OW2 in our case) is that the users and developers community which are not working in the same organization than you must be able to compile your project without the help of your Maven2 proxy...

The current article will focus on how to use Sonatype Nexus on the corporate side and how to deliver your independant Open Source project. I will not speak about how to use Nexus as artifact repository here (will probably come in another post).

<strong>1. Always define repositories in your project POMs</strong>
This is because the community must be able to compile all without any proxy. It is quite important to give IDs to each repository and to keep the ID list in mind...
For example, here is the OW2 Release repository definition :

[sourcecode language="xml"]
		&lt;repository&gt;
			&lt;id&gt;ow2.release&lt;/id&gt;
			&lt;name&gt;OW2 Repo&lt;/name&gt;
			&lt;url&gt;http://maven.ow2.org/maven2&lt;/url&gt;
			&lt;snapshots&gt;
				&lt;enabled&gt;false&lt;/enabled&gt;
			&lt;/snapshots&gt;
			&lt;releases&gt;
				&lt;enabled&gt;true&lt;/enabled&gt;
			&lt;/releases&gt;
		&lt;/repository&gt;
[/sourcecode]

<strong>2. Give the list of repositories to proxify</strong>
<em> 2.1 Add the repository to Nexus</em>
Ask your Nexus administrator to add the repositories you want to proxify to Nexus (this is not part of this blog entry, refer to the Nexus guide which is quite complete for that).

<em>2.2 Define the list in your local settings.xml file</em>
In the settings.xml file located under $HOME/.m2/, you must define the list of repository IDs you want to be handled by your Nexus Proxy:

[sourcecode language="xml"]
&lt;mirror&gt;
&lt;id&gt;nexus&lt;/id&gt;
&lt;name&gt;Nexus Mirror&lt;/name&gt;
&lt;url&gt;http://m2proxy:8081/nexus/content/groups/public/&lt;/url&gt;
&lt;mirrorOf&gt;central,ow2.release,ow2-plugin.release,ow2-plugin&lt;/mirrorOf&gt;
&lt;/mirror&gt;
[/sourcecode]

This configuration will say to the Maven engine that all the calls to the 4 repositories ('central,ow2.release,ow2-plugin.release,ow2-plugin') will be forwarded to the corporate Nexus proxy running on http://m2proxy:8081.

Following this procedure, you will be able to compile your project with AND without the proxy but the proxy will really optimize your compilation time.
