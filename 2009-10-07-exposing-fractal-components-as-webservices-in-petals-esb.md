---
layout: post
title: Exposing Fractal Components as Webservices in PEtALS ESB
categories:
- PEtALS
tags:
- PEtALS
- SOA
- WebService
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _elasticsearch_indexed_on: '2009-10-07 10:23:42'
---
I just added a new feature to PEtALS ESB kernel in order to expose Fractal Components as Webservices in the PEtALS ESB kernel. As an example, let's expose some PEtALS runtime information as Web service.

Here are the steps to follow :

<strong><em>1. Create your component interface and add JAXWS annotations</em></strong>

[sourcecode language="java"]
package org.ow2.petals.kernel.ws.api;

import java.util.Date;

import javax.jws.WebMethod;
import javax.jws.WebResult;
import javax.jws.WebService;

@WebService
public interface InformationService {

    /**
     * Get the container version
     *
     * @return
     * @throws PEtALSWebServiceException
     */
    @WebMethod()
    @WebResult(name = &quot;version&quot;)
    String getVersion() throws PEtALSWebServiceException;

    /**
     * Get the container type : standalone, platform, quickstart...
     *
     * @return
     * @throws PEtALSWebServiceException
     */
    @WebMethod()
    @WebResult(name = &quot;type&quot;)
    String getType() throws PEtALSWebServiceException;

    @WebMethod
    Date getLocalTime() throws PEtALSWebServiceException;

}
[/sourcecode]

<em><strong>2. Implement your Fractal component</strong></em><strong>
</strong>

[sourcecode language="java"]
package org.ow2.petals.ws;

import java.util.Date;

import org.objectweb.fractal.api.Component;
import org.objectweb.fractal.fraclet.annotation.annotations.FractalComponent;
import org.objectweb.fractal.fraclet.annotation.annotations.Interface;
import org.objectweb.fractal.fraclet.annotation.annotations.LifeCycle;
import org.objectweb.fractal.fraclet.annotation.annotations.Monolog;
import org.objectweb.fractal.fraclet.annotation.annotations.Provides;
import org.objectweb.fractal.fraclet.annotation.annotations.Requires;
import org.objectweb.fractal.fraclet.annotation.annotations.type.LifeCycleType;
import org.objectweb.util.monolog.api.Logger;
import org.ow2.petals.jbi.management.admin.AdminService;
import org.ow2.petals.kernel.ws.api.InformationService;
import org.ow2.petals.kernel.ws.api.PEtALSWebServiceException;
import org.ow2.petals.tools.ws.KernelWebService;
import org.ow2.petals.util.LoggingUtil;

@FractalComponent
@Provides(interfaces = { @Interface(name = &quot;webservice&quot;, signature = InformationService.class),
        @Interface(name = &quot;service&quot;, signature = KernelWebService.class) })
public class InformationServiceImpl implements KernelWebService, InformationService {

    @Monolog(name = &quot;logger&quot;)
    private Logger logger;

    private LoggingUtil log;

    @org.objectweb.fractal.fraclet.annotation.annotations.Service(name = &quot;component&quot;)
    private Component component;

    @Requires(name = &quot;adminService&quot;, signature = AdminService.class)
    private AdminService adminService;

    @LifeCycle(on = LifeCycleType.START)
    protected void start() {
        this.log = new LoggingUtil(this.logger);
        this.log.debug(&quot;Starting...&quot;);
    }

    @LifeCycle(on = LifeCycleType.STOP)
    protected void stop() {
        this.log.debug(&quot;Stopping...&quot;);
    }

    /**
     * {@inheritDoc}
     */
    public Component getComponent() {
        return this.component;
    }

    /**
     * {@inheritDoc}
     */
    public String getType() throws PEtALSWebServiceException {
        return &quot;Platform&quot;;
    }

    /**
     * {@inheritDoc}
     */
    public String getVersion() throws PEtALSWebServiceException {
        return this.adminService.getSystemInfo();
    }

    /**
     * {@inheritDoc}
     */
    public Date getLocalTime() throws PEtALSWebServiceException {
        return new Date();
    }

}
[/sourcecode]

In the implementation, the important points are :
<ol>
	<li>The @Provides part. The KernelWebService.class interface MUST be named "service". The 'business' interface to be exposed (here InformationService.class MUST be named "webservice")</li>
	<li>The Component field is mandatory (used by the webservice manager). Its accessor is mandatory too!</li>
	<li>The implementation MUST implement KernelWebService and the interface you want to expose</li>
</ol>
The implementation will not be exposed as Web service if the previous points are not followed.

<strong><em>3. Add the component to the WebServiceManager component with the help of Fractal descriptors</em></strong>

For now, the Fractal composite used to expose Fractal components as Web services is defined in the Tools.fractal descriptor (located under your favorite petals distribution). Here are the important steps :

<em>A. Instanciate the component
</em> <span style="font-family:Monaco, 'Times New Roman', 'Bitstream Charter', Times, serif;line-height:normal;font-size:11px;">&lt;component definition="org.ow2.petals.ws.InformationServiceImpl" name="InformationWebServiceImpl"/&gt;</span>
<div><em>B. Bind component (dependencies to other components)</em></div>
<div><span style="font-family:Monaco, 'Times New Roman', 'Bitstream Charter', Times, serif;line-height:normal;font-size:11px;">&lt;binding client="InformationWebServiceImpl.adminService" server="this.adminService"/&gt;</span></div>
<div>
<div><span style="font-family:Monaco, 'Times New Roman', 'Bitstream Charter', Times, serif;font-size:small;"><span style="line-height:normal;">
</span></span></div>
</div>
<div><em>C. Say to the Web service Manager to expose this component as Web service</em></div>
<div>
<p style="font:11px Monaco;margin:0;">&lt;binding client="WebServiceManagerImpl.webservice-information" server="InformationWebServiceImpl.service"/&gt;</p>

<div><span style="font-family:Monaco, 'Times New Roman', 'Bitstream Charter', Times, serif;"><span style="line-height:normal;font-size:small;">
</span></span></div>
</div>
<div>Note that in the last point, you MUST always give 'WebServiceManagerImpl-webservice-SOMETHING' as client and give the implementation service as server.</div>
<div>Done! Your component is now exposed as Web service and accessible at 'http://HOST:7600/petals/ws/SERVICE' where HOST is the host on which you start petals and SERVICE depends on the JAXWS WebService annotation or class name.</div>
