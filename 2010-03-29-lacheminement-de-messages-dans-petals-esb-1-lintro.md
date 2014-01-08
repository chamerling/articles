---
layout: post
title: ! 'L''acheminement de messages dans Petals ESB : #1 L''intro'
categories:
- PEtALS
tags:
- distributed
- jbi
- opensource
- ow2
- PEtALS
- petalspourlesnuls
- SOA
- tip
- tuto
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-03-29
    13:38:04";}
  _elasticsearch_indexed_on: '2010-03-29 13:38:04'
---
<strong>L'intro</strong>

Je commence ici une petite série d'articles sur l'acheminement  de messages dans <a href="http://petals.ow2.org">Petals ESB</a> pour décrire d'abord simplement et par la suite plus techniquement (avec du code ) comment le message se balade dans le bus de services lors de l'invocation de services. Cette invocation passe par plusieurs étapes, qui sont 'customisables' et extensibles facilement pour répondre à des besoins distincts, on verra cette partie plus tard...

<strong>Le blahblah</strong>

On le dit depuis la première version de Petals ESB, Petals est Le bus de services <strong>distribué</strong> qui implémente la spécification JBI. Ici distribué signifie que des instances du bus de service sont lancées sur des hotes distincts et que du point de vue de l'utilisateur, cela est totalement transparent ie l'utilisateur n'a rien à configurer de plus qu'un fichier de description du réseau (contrairement aux concurrents où l'on doit généralement configurer des composants JBI pour exposer des services). On ne se place pas ici à un transport de messages au niveau applicatif mais vraiment à un niveau (abstrait) de transport. Pour être plus clair, l'architecture de Petals ESB est divisée en couches et pour faire simple on a :
<ol>
	<li>Le canal de communication. Le canal de communication est la couche d'accès utilisée au niveau des composants JBI et donc au niveau des consommateurs et fournisseurs de services. Lors de l'invocation d'un service, le composant qui invoque un service envoi un message au canal. Le canal possède aussi tout un jeu de 'listeners' qui permettent de recevoir des messages de la couche de niveau N-1 (ie la couche de routage) en mode fournisseur de services.</li>
	<li>La couche de routage. En mode consommateur de service, cette couche reçoit le message du canal de communication et est chargé de trouver le point d'accès (endpoint) à qui le message doit être envoyé. Cet endpoint peut être local ou distant (sur un autre instance de Petals ESB). Elle permet aussi une remontée du message vers le bon canal de communication et donc vers le bon fournisseur de service en mode fournisseur.</li>
	<li>La couche de transport. Cette couche reçoit le message et le endpoint auquel le message doit être envoyé. Elle envoi ce message au endpoint, qu'il soit local ou distant. La couche de transport gère bien sûr la réception des messages envoyés depuis les instances distantes pour l'invocation des services qui se trouvent sur l'instance locale.</li>
</ol>
<strong>La suite</strong>

Dans les prochains articles, je détaillerais les couches de routage et de transport, leurs features, leurs points d'extensions, etc, etc... Et avec peu de code, on verra que l'on peut customizer Petals ESB.
