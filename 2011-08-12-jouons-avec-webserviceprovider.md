---
layout: post
title: Jouons avec WebServiceProvider...
categories:
- Java
- WebService
tags:
- Annotation
- Apache CXF
- Application programming interface
- cxf
- Java
- Java Architecture for XML Binding
- javax.xml.ws
- jaxws
- petalslink
- Web service
- webserviceprovider
- XML
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_mess: ! 'Jouons avec WebServiceProvider...:'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _oembed_c2fc106149f3319cd7007c7ce4e0c91c: ! '{{unknown}}'
  _oembed_65d3de016593ca35507066b9d8cd0124: ! '{{unknown}}'
  _oembed_2665652ad8dfdf4be27b3a7a0656e787: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2011-08-12 12:16:33'
---
Il y a des jours où exposer ses classes annotés avec @WebService n'est pas satisfaisant...
Pour moi ce jour c'est aujourd'hui: Il m'est impossible de marshaller mes beans en document DOM à cause d'un contexte JAXB tordu et d'une API qui ne prend que du DOM en entrée (OK bon ça c'est nul mais c'est pas de moi...). Qu'a cela ne tienne, il est temps d'utiliser les WebServiceProvider puisque ils vont me permettre de directement récupérer mon message SOAP sous forme de document... Un peu moins <em>straight forward</em> comme approche mais qui peut convenir dans certains cas. Comme les exemples ne courent pas le Web, regardons ce que l'on peut faire pour s'en sortir...

(Tout le code source de cet article est disponible sur Github : <a href="https://github.com/chamerling/chamerling.org-samples/tree/master/cxf-serviceprovider-072011">http://github.com/chamerling/chamerling.org-samples/tree/master/cxf-serviceprovider-072011</a>)

Une première solution simple est d'implémenter javax.xml.ws.Provider, de rajouter deux/trois annotations et le tour est quasiment joué:

[sourcecode language="java"]
package org.chamerling.blog.cxf.serviceprovider;

import javax.xml.soap.SOAPMessage;
import javax.xml.ws.Provider;
import javax.xml.ws.ServiceMode;
import javax.xml.ws.WebServiceProvider;

/**
* @author chamerling
*
*/
@WebServiceProvider
@ServiceMode(value = javax.xml.ws.Service.Mode.MESSAGE)
public class SimpleServiceProvider implements Provider {

/*
* (non-Javadoc)
*
* @see javax.xml.ws.Provider#invoke(java.lang.Object)
*/
@Override
public SOAPMessage invoke(SOAPMessage in) {
return null;
}
}
[/sourcecode]

Reste à l'exposer avec CXF en utilisant <em>org.apache.cxf.jaxws.JaxWsServerFactoryBean</em>. Cette approche simpliste a le mérite de marcher, il ne reste plus qu'a manipuler les SOAPMessage dans l'implémentation de la méthode <em>invoke</em>.

Besoin de l'opération qui est appelée? Ajoutons <em>javax.xml.ws.WebServiceContext</em> en tant que <em>javax.annotation.Resource</em>. Ce contexte sera automatiquement rempli pour nous par CXF et accessible dans le corps de la méthode <em>invoke</em>. Par exemple, on peut travailler de la sorte:

[sourcecode language="java"]
/**
 *
 */
package org.chamerling.blog.cxf.serviceprovider;

import javax.annotation.Resource;
import javax.xml.namespace.QName;
import javax.xml.soap.SOAPMessage;
import javax.xml.ws.Provider;
import javax.xml.ws.ServiceMode;
import javax.xml.ws.WebServiceContext;
import javax.xml.ws.WebServiceProvider;
import javax.xml.ws.handler.MessageContext;

/**
 * @author chamerling
 *
 */
@WebServiceProvider
@ServiceMode(value = javax.xml.ws.Service.Mode.MESSAGE)
public class SimpleServiceProvider implements Provider {

	@Resource
	WebServiceContext wsContext;

	/*
	 * (non-Javadoc)
	 *
	 * @see javax.xml.ws.Provider#invoke(java.lang.Object)
	 */
	@Override
	public SOAPMessage invoke(SOAPMessage in) {
		System.out.println(&quot;Operation : &quot; + getOperation());
		System.out.println(&quot;Message In :&quot;);
		try {
			in.writeTo(System.out);
		} catch (Exception e) {
			// bad
		}
		System.out.println();
		return null;
	}

	private QName getOperation() {
		QName result = null;
		if (wsContext != null &amp;&amp; wsContext.getMessageContext() != null) {
			Object o = wsContext.getMessageContext().get(
					MessageContext.WSDL_OPERATION);
			if (o != null &amp;&amp; o instanceof QName) {
				result = (QName) o;
			}
		}
		return result;
	}
}
[/sourcecode]

Et invoquer le service:

[sourcecode language="java"]
package org.chamerling.blog.cxf.serviceprovider;

import org.apache.cxf.jaxws.JaxWsProxyFactoryBean;
import org.apache.cxf.jaxws.JaxWsServerFactoryBean;

import junit.framework.TestCase;

/**
 * @author chamerling
 *
 */
public class SimpleServiceTest extends TestCase {

	public void testExpose() throws Exception {
		String url = &quot;http://localhost:9999/foo/bar/SimpleService&quot;;
		final JaxWsServerFactoryBean ssf = new JaxWsServerFactoryBean();
		ssf.setAddress(url);
		ssf.setServiceBean(new SimpleServiceProvider());
		ssf.create();

		HelloService client = getClient(url);
		client.sayHello(&quot;Rock N Roll!&quot;);
	}

	/**
	 * @param url
	 * @return
	 */
	private HelloService getClient(String url) {
		JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
		factory.setAddress(url);
		factory.setServiceClass(HelloService.class);
		Object client = factory.create();
		return HelloService.class.cast(client);
	}
}
[/sourcecode]

Dans ce cas, on a une opération affichée qui est <em>{http://serviceprovider.cxf.blog.chamerling.org/}invoke</em> et le message SOAP

[sourcecode language="xml"]
Rock N Roll!
[/sourcecode]

Pas très convaincant pour l'opération avec cette approche... Le mieux est de pousser un peu plus loin et partir du contrat de service (WSDL) de notre Provider. CXF permet de le spécifier via ses factories lors de la construction du service. Cette utilisation plus avancée est détaillée en partant de <em><a href="http://github.com/chamerling/chamerling.org-samples/blob/master/cxf-serviceprovider-072011/src/main/java/org/chamerling/blog/cxf/serviceprovider/CXFExposer.java" target="_blank">org.chamerling.blog.cxf.serviceprovider.CXFExposer</a></em>. L'approche utilisée dans CXFExposer permet aussi de cacher toute la tambouille JAXWS à l'utilisateur, au final il a besoin d'implementer seulement <em><a href="http://github.com/chamerling/chamerling.org-samples/blob/master/cxf-serviceprovider-072011/src/main/java/org/chamerling/blog/cxf/serviceprovider/Service.java" target="_blank">org.chamerling.blog.cxf.serviceprovider.Service</a></em>

[sourcecode language="java"]
package org.chamerling.blog.cxf.serviceprovider;

import javax.xml.namespace.QName;

import org.w3c.dom.Document;

/**
 * @author chamerling
 *
 */
public interface Service {

    /**
     * Get the WSDL description
     *
     * @return
     */
    String getWSDLURL();

    /**
     * Get the service URL ie where to publish it...
     *
     * @return
     */
    String getURL();

    QName getEndpoint();

    QName getInterface();

    QName getService();

    /**
     * Invoke the service
     *
     * @param request
     * @param action
     */
    Document invoke(Document in, QName operation) throws ServiceException;

}
[/sourcecode]
