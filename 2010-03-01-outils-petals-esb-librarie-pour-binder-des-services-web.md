---
layout: post
title: ! 'Outils Petals ESB : Librarie pour binder des services Web'
categories:
- PEtALS
tags:
- Java
- jbi
- management
- maven
- PEtALS
- pom
- SOA
- tip
- WebService
- wsdl
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-03-01
    11:29:55";}
  _oembed_f9d8e406a663b8d745ad0e0007ae9801: ! '{{unknown}}'
  _oembed_566074be8a14d91da6cc540e5c33d4cd: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2010-03-01 11:29:55'
---
Dans un <a href="http://chamerling.wordpress.com/2009/06/02/proxify-web-services-in-petals-with-maven/">précédent article</a>, je décrivais comment 'proxifier' un Web service dans <a href="http://petals.ow2.org">Petals ESB</a> avec un plugin Maven maison. Ce plugin Maven utilise une librairie Java que j'ai développé pour lier et exposer des Web services dans/avec Petals ESB.

Le 'problème' actuel de Petals ESB est qu'il est <strong>pleinement</strong> basé sur la spécification JBI, cela veut dire que actuellement, si on veut exposer un service JBI en tant que Web service, il faut créer un fichier de configuration (le fameux JBI.xml), le packager dans une unité de service (Service Unit - SU), et packager cette SU dans un assemblage de services (Service Assembly - SA). Bien sur des outils comme le Petals Studio sont la pour aider les développeurs à intégrer leurs services, moi je me place plus dans la peau du 'Core Developper' et je pour le moment je n'ai pas eu l'utilité d'outils graphiques pour configurer Petals.

Dans une de mes taches actuelles, je suis en train d'étendre Petals pour le faire communiquer sur Internet et je développe ce que j'appelle l'API de Monitoring &amp; Management (M&amp;M API) qui est exposée (pour le moment seulement) en Web service. La partie Management de l'API doit permettre à un administrateur de la plateforme, de 'binder' des services (Web, REST, ...) sans se soucier de la partie JBI.
L'API fournit donc une opération du style <em>bind(URI wsdl). </em>Coté serveur, j'utilise :
<ol>
	<li>Une librairie maison de génération de SA</li>
	<li>Je déploie localement cette SA pour 'binder' un Web service au bus.</li>
	<li>J'expose ce service de management avec la feature expliquée dans ce <a href="http://chamerling.wordpress.com/2009/10/07/exposing-fractal-components-as-webservices-in-petals-esb/">post</a></li>
</ol>
Un exemple d'utilisation de la librairie est donné dans le listing suivant (binder au bus, le Service Web Version de Axis2) :

[sourcecode language="java"]

package org.ow2.petals.tools.generator.jbi;

import java.io.File;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.HashMap;
import java.util.Map;
import org.ow2.petals.tools.generator.jbi.api.JBIGenerationException;
import org.ow2.petals.tools.generator.jbi.ws2jbi.WS2Jbi;

public class BindAxis2Version {

public static void main(String[] args) throws JBIGenerationException, URISyntaxException {

String wsdl = &quot;http://localhost:8080/axis2/services/Version?wsdl&quot;;
Map&lt;String, String&gt; map = new HashMap&lt;String, String&gt;();

// let's say that we want to generate SA for SOAP component 4.0
map.put(org.ow2.petals.tools.generator.commons.Constants.COMPONENT_VERSION, &quot;4.0&quot;);

URI wsdlURI = new URI(wsdl);

File f = new WS2Jbi(wsdlURI, map).generate();

System.out.println(&quot;Service Assembly is available at &quot; + f.getAbsolutePath());

}
}

[/sourcecode]

Pour utiliser WS2Jbi, utilisez dans votre projet Maven, la dépendance :

[sourcecode language="xml"]

&lt;dependency&gt;

&lt;groupId&gt;org.ow2.petals.sandbox&lt;/groupId&gt;

&lt;artifactId&gt;petals-ws2jbi&lt;/artifactId&gt;

&lt;version&gt;1.0-SNAPSHOT&lt;/version&gt;

&lt;/dependency&gt;

[/sourcecode]

Celle ci est normalement disponible est mise a jour régulièrement sur le repository Maven d'OW2 <a href="http://maven.ow2.org/maven2-snapshot/">http://maven.ow2.org/maven2-snapshot/</a>. Les sources sont disponibles dans ma <a href="http://websvn.ow2.org/listing.php?repname=petals&amp;path=%2Fsandbox%2Fchamerling%2F#path_sandbox_chamerling_">Sandbox Petals</a>.
