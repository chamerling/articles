---
layout: post
title: ! 'Axis2 : Rerouter vos appels de services sans modifier vote code client'
categories:
- WebService
tags:
- apache
- axis2
- module
- routage
- SOA
- WebService
- wsdl
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-03-09
    16:08:14";}
  _oembed_4c703f37c1f927576be541f56ca7549a: ! '{{unknown}}'
  _oembed_a180f51f06df407aac1354bad8d35919: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2010-03-09 16:08:14'
---
Imaginons que vous ayez à votre disposition un client de Web service et que pour une raison (qui doit être bonne je l'accorde), vous avez besoin de rerouter vos appels de service vers un nouveau point d'accès. Le problème est que vous n'avez pas le droit de modifier le code client (ou que vous ne l'avez pas), que vous ne voulez pas regénérer ce code client à partir du WSDL du nouveau service a appeler (qui est exactement le même hormis l'adresse du endpoint)... Heureusement, votre client utilise la pile Web service Apache Axis2, et les développeurs d'Axis2 ont gentiment pensé à introduire des modules dans leur architecture. Une bonne intorduction aux modules est disponible dans <a href="http://ws.apache.org/axis2/1_4_1/Axis2ArchitectureGuide.html">le guide d'architecture d'Axis2</a>.

Nous allons donc développer un <a href="http://ws.apache.org/axis2/1_4_1/modules.html">module</a> qui change dynamiquement l'URL de l'endpoint à appeler (on utilisera bien sûr Maven2 pour nous aider à créer et packager le projet...).

<strong>1. Créer le projet</strong>

Utilisons Maven2 pour créer un projet :

[sourcecode language="bash"]
mvn archetype:generate
[/sourcecode]

et je réponds bêtement a Maven quand il me pose des questions sur le groupId, l'artifactId, etc... Une fois créé, je génère le projet Eclipse associé et je l'importe (Eclipse fait aussi parti de mes grand amis) :

[sourcecode language="bash"]
mvn eclipse:eclipse
[/sourcecode]

Ill faut modifier le POM pour dire a Maven que l'on est en train de créer un projet qui n'est pas une librarie Java mais un module Axis2. Ceci est possible en spécifiant le type de projet dans le POM et en déclarant que l'on veut utiliser le plugin MAR fournit par Axis2 :

[sourcecode language="xml"]

&lt;project ...&gt;

...

&lt;groupId&gt;foo.bar&lt;/groupId&gt;
&lt;artifactId&gt;reroute&lt;/artifactId&gt;
&lt;version&gt;1.0-SNAPSHOT&lt;/version&gt;
&lt;packaging&gt;mar&lt;/packaging&gt;

...

&lt;dependencies&gt;
&lt;dependency&gt;
&lt;groupId&gt;org.apache.axis2&lt;/groupId&gt;
&lt;artifactId&gt;axis2-kernel&lt;/artifactId&gt;
&lt;version&gt;1.5.1&lt;/version&gt;
&lt;/dependency&gt;
&lt;/dependencies&gt;

...

&lt;build&gt;
&lt;plugins&gt;
&lt;plugin&gt;
&lt;groupId&gt;org.apache.axis2&lt;/groupId&gt;
&lt;artifactId&gt;axis2-mar-maven-plugin&lt;/artifactId&gt;
&lt;version&gt;1.5.1&lt;/version&gt;
&lt;extensions&gt;true&lt;/extensions&gt;
&lt;configuration&gt;
&lt;moduleXmlFile&gt;src/main/META-INF/module.xml&lt;/moduleXmlFile&gt;
&lt;includeDependencies&gt;false&lt;/includeDependencies&gt;
&lt;/configuration&gt;
&lt;/plugin&gt;
&lt;/plugins&gt;
&lt;/build&gt;
...
&lt;/project&gt;

[/sourcecode]

<strong>2. Business code</strong>

Créons maintenant notre handler (le module est en fait un handler Axis2 packagé dans une archive avec un descripteur de module). Pour faire simple, je vais étendre la classe AbstractHandler pour créer mon Handler de reroutage et remplacer le 'target EPR' par le nouveau (bien sûr on peut faire quelque chose de beaucoup plus évolué, on va dire qu'ici tout part vers un seul service) :

[sourcecode language="java"]
package net.chamerling.blog.axis2module.reroute;

import org.apache.axis2.AxisFault;
import org.apache.axis2.addressing.EndpointReference;
import org.apache.axis2.context.MessageContext;
import org.apache.axis2.handlers.AbstractHandler;
import org.apache.axis2.util.LoggingControl;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

/**
 * @author chamerling
 *
 */
public class ReRouteHandler extends AbstractHandler {
	private static final Log log = LogFactory.getLog(AbstractHandler.class);

	private static final String FINAL_EPR_PREFIX = &quot;http://localhost:8082/petals/proxy/&quot;;

	/**
	 * {@inheritDoc}
	 */
	public InvocationResponse invoke(MessageContext messageContext)
			throws AxisFault {
		EndpointReference toEPR = messageContext.getOptions().getTo();
		String finalEPR = FINAL_EPR_PREFIX + toEPR.getAddress();

		if (LoggingControl.debugLoggingAllowed &amp;&amp; log.isDebugEnabled()) {
			log.debug(&quot;Reroute Handler for message : &quot;
					+ messageContext.getMessageID());
			log.debug(&quot;Initial ToEPR is &quot; + toEPR.getAddress()
					+ &quot; and is changed to &quot; + finalEPR);
		}

		toEPR.setAddress(finalEPR);
		return InvocationResponse.CONTINUE;
	}
}
&lt;pre&gt;[/sourcecode]

Avec Axis2 c'est 'facile', tout est dans le message context! Je viens de remplacer le service à appeler par le mien en modifiant le champ 'address' du 'EndpointReference'.

<strong>3. Packager le module</strong>

Pour créer un module avec le Handler que l'on vient de définir, il faut passer par l'écriture du fichier de description du module, le 'module.xml'. Ici c'est simple :

[sourcecode language="xml"]
&lt;module name=&quot;reroute&quot;&gt;
	&lt;OutFlow&gt;
		&lt;handler name=&quot;ReRouteHandler&quot; class=&quot;net.chamerling.blog.axis2module.reroute.ReRouteHandler&quot;&gt;
			&lt;order phase=&quot;reroutePhase&quot; /&gt;
		&lt;/handler&gt;
	&lt;/OutFlow&gt;
&lt;/module&gt;
&lt;pre&gt;[/sourcecode]

Mon module s'appellera donc 'reroute' et sera appelé que lors de la phase d'émission de message 'OutFlow'. Plus d'informations sur ce qui est possible avec ce fichier sur le site d'Axis2.

<strong>4. Configurer Axis2</strong>

Il y a plusieurs façcons de dire à Axis2 d'utiliser le module que l'on vient de développer. En se basant sur la contrainte de départ, je n'ai pas le droit de modifier mes services donc je ne peux pas modifier le descripteur de service 'service.xml'. Heureusement, j'ai accès au serveur et je peux modifier le fichier de configuration d'Axis2 'axis2.xml' :

[sourcecode language="xml"]
...
&lt;!-- Engage the reroute module --&gt;
&lt;module ref=&quot;reroute&quot; /&gt;
...
&lt;phaseOrder type=&quot;OutFlow&quot;&gt;
	&lt;!--      user can add his own phases to this area  --&gt;
	&lt;phase name=&quot;soapmonitorPhase&quot; /&gt;
	&lt;phase name=&quot;OperationOutPhase&quot; /&gt;
	&lt;!--system predefined phase--&gt;
	&lt;!--these phase will run irrespective of the service--&gt;
	&lt;phase name=&quot;PolicyDetermination&quot; /&gt;
	&lt;phase name=&quot;reroutePhase&quot; /&gt;
	&lt;phase name=&quot;MessageOut&quot; /&gt;
	&lt;phase name=&quot;Security&quot; /&gt;
&lt;/phaseOrder&gt;
&lt;pre&gt;[/sourcecode]

Je viens d'engager mon module pour tous les services et je l'ai aussi ajouté dans la collection de phases qui est appelé lors de l'émission d'un message. A chaque fois qu'un message est envoyé vers un service, on passe donc dans le module que l'on vient de développer. Je viens donc de modifier les services invoqués sans modifier le code client! Je reviendrais bientôt sur l'utilisation de ce genre de choses dans un vrai cas d'usage avec du REST, du SOAP, des PROXYs, de l'ESB distribué sur le Web (bien sûr avec <a href="http://petals.ow2.org">Petals ESB</a>), etc...

<strong>Resources</strong>

Le code de ce 'tutoriel' est bien sûr disponible sur mon Google Code sous <a href="http://code.google.com/p/chamerling/source/browse/#svn/trunk/blog/reroute-axis2">http://code.google.com/p/chamerling/source/browse/#svn/trunk/blog/reroute-axis2</a>.
