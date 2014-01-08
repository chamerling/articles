---
layout: post
title: ! 'Simple & Useful : Remote debugging with Ant'
categories:
- Java
tags:
- Java
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-03-25 15:13:27'
---
A Ant java call like :

<em>&lt;target name="run"&gt;
  &lt;java classname="foo.bar.App" fork="false"&gt;
    &lt;classpath refid="app.classpath" /&gt;
  &lt;/java&gt;
&lt;/target&gt;</em>

becomes :

<em>&lt;target name="run"&gt;
  &lt;java classname="foo.bar.App" fork="<strong>yes</strong>"&gt;
    &lt;classpath refid="app.classpath" /&gt;
 <strong>   &lt;sysproperty key="DEBUG" value="true" /&gt;
    &lt;jvmarg value="-Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=8000" /&gt;</strong>
  &lt;/java&gt;
&lt;/target&gt;</em>

and is ready to be launch and 'remote debugged' from a tool like Eclipse IDE.
