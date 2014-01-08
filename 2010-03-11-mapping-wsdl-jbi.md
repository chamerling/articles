---
layout: post
title: Mapping WSDL JBI
categories:
- PEtALS
tags:
- jbi
- mapping
- ow2
- PEtALS
- petalspourlesnuls
- tip
- WebService
- wsdl
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _wpas_done_yup: '1'
  _elasticsearch_indexed_on: '2010-03-11 12:52:03'
---
Cet article fait suite à une question récurrente des utilisateurs de Petals ESB : "Pourquoi j'ai une erreur au déploiement de mon service???". Je vais ici tenter de donner une réponse partielle à cette question car il peut y avoir beaucoup de raisons à une erreur de déploiement. La plus courante venant souvent de la configuration. Rien que bien technique ni compliqué dans cet article, juste un mini tutoriel pour mieux comprendre la philosophie Petals - JBI.

La spécification <a href="http://www.jcp.org/en/jsr/detail?id=208">JBI</a> est basée sur l'état de l'art des Web services et donc utilise WSDL pour la description des services 'hostés' ou liés à l'implémentation JBI. <a href="http://petals.ow2.org">Petals ESB</a> implémente (et étend - mais ça sera peut être l'objet d'un autre article sur le pourquoi du comment...) la spécification JBI depuis sa première version. Petals ESB est même certifié compatible JBI par SUN (RIP) après le passage du TCK JBI (Outil de test de compatibilité fournit par SUN). Bref, là n'est pas la question... Les descripteurs JBI servent à beaucoup de choses dans une implémentation JBI mais nous allons particulièrement regarder comment nous nous en servons dans l'exposition de services.

Un service exposé dans le bus fournit donc sa description via un WSDL associé. Ce service est activé dans le bus après le déploiement de ce que l'on appelle communément une 'SA' (Service Assembly) qui contient des 'SU' (Service Unit). Pour exposer un service dans le bus afin qu'il soit accessible à tout les consommateurs, la SU doit être en mode 'provide' ie fournisseur. Elle doit aussi définir le Endpoint, le Service et l'Interface qu'elle veut exposer. Prenons l'exemple du bon vieux service HelloWorld définit par le WSDL suivant :

[sourcecode language="xml"]
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?&gt;
&lt;ns2:definitions name=&quot;HelloServiceImplService&quot; targetNamespace=&quot;http://sample.petals.ow.org/&quot; xmlns:ns2=&quot;http://schemas.xmlsoap.org/wsdl/&quot; xmlns:ns3=&quot;http://schemas.xmlsoap.org/wsdl/soap/&quot;
	xmlns:ns9=&quot;http://www.w3.org/ns/wsdl/http&quot; xmlns:xs=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:ns5=&quot;http://schemas.xmlsoap.org/wsdl/soap12/&quot; xmlns:ns6=&quot;http://schemas.xmlsoap.org/wsdl/http/&quot;
	xmlns:ns10=&quot;http://schemas.xmlsoap.org/wsdl/mime/&quot; xmlns:ns7=&quot;http://www.w3.org/ns/wsdl&quot; xmlns:ns8=&quot;http://www.w3.org/ns/wsdl/soap&quot;&gt;
	&lt;ns2:types&gt;
		&lt;xs:schema elementFormDefault=&quot;unqualified&quot; attributeFormDefault=&quot;unqualified&quot; targetNamespace=&quot;http://sample.petals.ow.org/&quot;&gt;
			&lt;xs:element name=&quot;sayHello&quot; type=&quot;tns:sayHello&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot; /&gt;
			&lt;xs:element name=&quot;sayHelloResponse&quot; type=&quot;tns:sayHelloResponse&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot; /&gt;
			&lt;xs:complexType name=&quot;sayHello&quot;&gt;
				&lt;xs:sequence&gt;
					&lt;xs:element name=&quot;arg0&quot; minOccurs=&quot;0&quot; type=&quot;xs:string&quot; /&gt;
				&lt;/xs:sequence&gt;
			&lt;/xs:complexType&gt;
			&lt;xs:complexType name=&quot;sayHelloResponse&quot;&gt;
				&lt;xs:sequence&gt;
					&lt;xs:element name=&quot;return&quot; minOccurs=&quot;0&quot; type=&quot;xs:string&quot; /&gt;
				&lt;/xs:sequence&gt;
			&lt;/xs:complexType&gt;
		&lt;/xs:schema&gt;
	&lt;/ns2:types&gt;
	&lt;ns2:message name=&quot;sayHelloResponse&quot;&gt;
		&lt;ns2:part element=&quot;tns:sayHelloResponse&quot; name=&quot;parameters&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot; /&gt;
	&lt;/ns2:message&gt;
	&lt;ns2:message name=&quot;sayHello&quot;&gt;
		&lt;ns2:part element=&quot;tns:sayHello&quot; name=&quot;parameters&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot; /&gt;
	&lt;/ns2:message&gt;
	&lt;ns2:portType name=&quot;HelloService&quot;&gt;
		&lt;ns2:operation name=&quot;sayHello&quot;&gt;
			&lt;ns2:input message=&quot;tns:sayHello&quot; name=&quot;sayHello&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot; /&gt;
			&lt;ns2:output message=&quot;tns:sayHelloResponse&quot; name=&quot;sayHelloResponse&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot; /&gt;
		&lt;/ns2:operation&gt;
	&lt;/ns2:portType&gt;
	&lt;ns2:binding type=&quot;tns:HelloService&quot; name=&quot;HelloServiceImplServiceSoapBinding&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot;&gt;
		&lt;ns3:binding style=&quot;document&quot; transport=&quot;http://schemas.xmlsoap.org/soap/http&quot; /&gt;
		&lt;ns2:operation name=&quot;sayHello&quot;&gt;
			&lt;ns3:operation style=&quot;document&quot; soapAction=&quot;&quot; /&gt;
			&lt;ns2:input name=&quot;sayHello&quot;&gt;
				&lt;ns3:body use=&quot;literal&quot; /&gt;
			&lt;/ns2:input&gt;
			&lt;ns2:output name=&quot;sayHelloResponse&quot;&gt;
				&lt;ns3:body use=&quot;literal&quot; /&gt;
			&lt;/ns2:output&gt;
		&lt;/ns2:operation&gt;
	&lt;/ns2:binding&gt;
	&lt;ns2:service name=&quot;HelloServiceImplService&quot;&gt;
		&lt;ns2:port binding=&quot;tns:HelloServiceImplServiceSoapBinding&quot; name=&quot;HelloServiceImplPort&quot; xmlns:tns=&quot;http://sample.petals.ow.org/&quot;&gt;
			&lt;ns3:address location=&quot;http://localhost:9999/sample/HelloService&quot; /&gt;
		&lt;/ns2:port&gt;
	&lt;/ns2:service&gt;
&lt;/ns2:definitions&gt;
&lt;pre&gt;[/sourcecode]

Son descripteur JBI associé sera donc (lié a Petals via le composant Web service dans cet exemple) :

[sourcecode language="xml"]
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;jbi:jbi version=&quot;1.0&quot;
        xmlns:generatedNs=&quot;http://sample.petals.ow.org/&quot;
        xmlns:jbi=&quot;http://java.sun.com/xml/ns/jbi&quot;
	xmlns:petalsCDK=&quot;http://petals.ow2.org/components/extensions/version-5&quot;
	xmlns:soap=&quot;http://petals.ow2.org/components/soap/version-4&quot;
	xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;&gt;

	&lt;jbi:services binding-component=&quot;true&quot;&gt;
		&lt;jbi:provides interface-name=&quot;generatedNs:HelloService&quot;
			service-name=&quot;generatedNs:HelloServiceImplService&quot;
			endpoint-name=&quot;HelloServiceImplPort&quot;&gt;

			&lt;!-- CDK specific elements --&gt;
			&lt;petalsCDK:timeout&gt;60000&lt;/petalsCDK:timeout&gt;
			&lt;petalsCDK:wsdl&gt;Service.wsdl&lt;/petalsCDK:wsdl&gt;

			&lt;!-- Component specific elements --&gt;
			&lt;soap:address&gt;http://localhost:9999/sample/HelloService&lt;/soap:address&gt;
			&lt;soap:soap-version&gt;11&lt;/soap:soap-version&gt;
			&lt;soap:add-root&gt;false&lt;/soap:add-root&gt;
			&lt;soap:chunked-mode&gt;false&lt;/soap:chunked-mode&gt;
			&lt;soap:cleanup-transport&gt;true&lt;/soap:cleanup-transport&gt;
			&lt;soap:mode&gt;SOAP&lt;/soap:mode&gt;
		&lt;/jbi:provides&gt;
	&lt;/jbi:services&gt;
&lt;/jbi:jbi&gt;
[/sourcecode]

Où on respecte bien que :
<ol>
	<li>JBI '<strong>endpoint-name</strong>' = Valeur de l'attribut 'name' du <strong>Port</strong> dans le WSDL</li>
	<li>JBI '<strong>service-name</strong>' = Valeur de l'attribut 'name' du <strong>Service</strong> dans le WSDL (avec JBI Service NameSpace = targetNamespace du WSDL)</li>
	<li>JBI '<strong>interface-name</strong>' = Valeur de l'attribut 'name' du <strong>PortType</strong> dans le WSDL (avec JBI Interface NameSpace = targetNamespace du WSDL)</li>
</ol>
Petals ESB v3.x ne vous insultera plus lors du déploiement de vos services. Une solution simple pour ne pas se faire insulter, est d'utiliser le <a href="http://www.petalslink.com/fr/produits/petals-studio">Studio Petals</a> qui facilite grandement la vie des développeurs!
