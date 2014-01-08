---
layout: post
title: An operation with name '' already exists in this service
categories:
- WebService
tags:
- cxf
- Java
- jaxws
status: publish
type: post
published: true
meta:
  _oembed_252d68b01e5f1bfe5c417880b18452ef: ! '{{unknown}}'
  _oembed_de25b69cebfb83dd0321d06e00c82e1b: ! '{{unknown}}'
  _oembed_d7eae0529de5d5e4b5d9e30d9e728e30: ! '{{unknown}}'
  _oembed_46d631fa64c822c8ccd41f0371e88da6: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2009-05-14 13:22:00'
---
Here is a tip on how to avoid this type of exception with JAXWS but before let's try to understand why your Service construciton fails...

This exception occurs when your service definition (the Java interface in our case) have some duplicates methods without the same number of parameters (ie method overloading). This exception is totally normal since it is quite logic that the resulting Web Service can only have unique operation names :
<ul>
	<li>Check a WSDL file. Did you ever see the same operation X times?</li>
	<li>The Web Service Engine generally tries to get the operation context from the operation name. Then it will unmarshall the parameters (it will not have a look to the operation parameters to choose the operation like it should be done in the Java Runtime).</li>
</ul>
Here is a service definition sample which will be used in this article :
<pre>package com.googlecode.chamerling.blog.jaxws.duplicate;

/**
 * @author Christophe HAMERLING
 *
 */
public interface Service {

	void foo();

	void foo(String bar);
}</pre>
So let's implement this interface :
<pre>package com.googlecode.chamerling.blog.jaxws.duplicate;

import javax.jws.WebService;

/**
 * @author Christophe HAMERLING
 *
 */
@WebService
public class ServiceKo implements Service {

	public void foo() {
	}

	public void foo(String bar) {
	}
}</pre>
Let's use the JaxWsServerFactoryBean CXF factory (other WS stacks should throw the same exception...) to expose this service :
<pre>import org.apache.cxf.jaxws.JaxWsServerFactoryBean;

/**
 * @author Christophe HAMERLING
 */
public class App {

	private static final String URL = "http://localhost:9999/chamerling/services/";

	public static void main(String[] args) {
		JaxWsServerFactoryBean svrFactory = new JaxWsServerFactoryBean();
		Service space = new ServiceKo();
		svrFactory.setAddress(URL + "ServiceKO");
		svrFactory.setServiceBean(space);

		org.apache.cxf.endpoint.Server service = svrFactory.create();

		System.out.println("Service is available at "
				+ service.getEndpoint().getEndpointInfo().getAddress());

	}
}</pre>
You should have a beautiful stack trace :
<pre>java.lang.IllegalArgumentException: An operation with name [{http://duplicate.jaxws.blog.chamerling.googlecode.com/}foo] already exists in this service
	at org.apache.cxf.service.model.InterfaceInfo.addOperation(InterfaceInfo.java:71)
	at org.apache.cxf.service.factory.ReflectionServiceFactoryBean.createOperation(ReflectionServiceFactoryBean.java:774)
	at org.apache.cxf.jaxws.support.JaxWsServiceFactoryBean.createOperation(JaxWsServiceFactoryBean.java:484)
	at org.apache.cxf.service.factory.ReflectionServiceFactoryBean.createInterface(ReflectionServiceFactoryBean.java:766)
	at org.apache.cxf.service.factory.ReflectionServiceFactoryBean.buildServiceFromClass(ReflectionServiceFactoryBean.java:361)
	at org.apache.cxf.jaxws.support.JaxWsServiceFactoryBean.buildServiceFromClass(JaxWsServiceFactoryBean.java:525)
	at org.apache.cxf.service.factory.ReflectionServiceFactoryBean.initializeServiceModel(ReflectionServiceFactoryBean.java:422)
	at org.apache.cxf.service.factory.ReflectionServiceFactoryBean.create(ReflectionServiceFactoryBean.java:190)
	at org.apache.cxf.jaxws.support.JaxWsServiceFactoryBean.create(JaxWsServiceFactoryBean.java:164)
	at org.apache.cxf.frontend.AbstractWSDLBasedEndpointFactory.createEndpoint(AbstractWSDLBasedEndpointFactory.java:100)
	at org.apache.cxf.frontend.ServerFactoryBean.create(ServerFactoryBean.java:117)
	at org.apache.cxf.jaxws.JaxWsServerFactoryBean.create(JaxWsServerFactoryBean.java:168)
	at com.googlecode.chamerling.blog.jaxws.duplicate.AppTest.testKoService(AppTest.java:37)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:585)
	at junit.framework.TestCase.runTest(TestCase.java:154)
	at junit.framework.TestCase.runBare(TestCase.java:127)
	at junit.framework.TestResult$1.protect(TestResult.java:106)
	at junit.framework.TestResult.runProtected(TestResult.java:124)
	at junit.framework.TestResult.run(TestResult.java:109)
	at junit.framework.TestCase.run(TestCase.java:118)
	at junit.framework.TestSuite.runTest(TestSuite.java:208)
	at junit.framework.TestSuite.run(TestSuite.java:203)
	at org.eclipse.jdt.internal.junit.runner.junit3.JUnit3TestReference.run(JUnit3TestReference.java:130)
	at org.eclipse.jdt.internal.junit.runner.TestExecution.run(TestExecution.java:38)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:460)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:673)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.run(RemoteTestRunner.java:386)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.main(RemoteTestRunner.java:196)</pre>
So the solution is to use the <span style="text-decoration:line-through;">@WebParam</span> @WebMethod annotation. Simply use this annotation to specify unique operation names in your service implementation :
<pre>package com.googlecode.chamerling.blog.jaxws.duplicate;

import javax.jws.WebMethod;
import javax.jws.WebService;

/**
 * @author Christophe HAMERLING
 *
 */
@WebService
public class ServiceOk implements Service {

	@WebMethod(operationName = "foo1")
	public void foo() {
	}

	@WebMethod(operationName = "foo2")
	public void foo(String bar) {
	}
}</pre>
Sources for this example are availble on my google code project (<a href="http://code.google.com/p/chamerling/" target="_blank">http://code.google.com/p/chamerling/</a>) under <a href="http://code.google.com/p/chamerling/source/browse/#svn/trunk/blog/jaxws-duplicate" target="_blank">http://code.google.com/p/chamerling/source/browse/#svn/trunk/blog/jaxws-duplicate</a>
