---
layout: post
title: ! 'Les classloaders des modules dans Axis2: Les ressources'
categories:
- WebService
tags:
- axis2
- classloader
- handler
- Java
- module
- properties
- routage
- SOA
- tip
- WebService
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-04-07
    14:24:03";}
  _elasticsearch_indexed_on: '2010-04-07 14:24:03'
---
J'ai expliqué le genre de choses intéressantes que l'on peut faire avec les modules dans la pile Apache Axis2 dans <a href="http://chamerling.wordpress.com/2010/03/09/axis2-rerouter-vos-appels-de-services-sans-modifier-vote-code-client/">l'article sur le </a><a href="http://chamerling.wordpress.com/2010/03/09/axis2-rerouter-vos-appels-de-services-sans-modifier-vote-code-client/">reroutage</a><a href="http://chamerling.wordpress.com/2010/03/09/axis2-rerouter-vos-appels-de-services-sans-modifier-vote-code-client/"> d'appels de services</a>. J'ai eu besoin aujourd'hui d'aller un peu plus loin dans l'utilisation de ces modules et comme d'habitude, en utilisant ce genre de choses, je partage mes aventures... On va voir rapidement ici comment se comportent les <em>ClassLoaders</em> dans le cas d'Axis2 et des modules.

<strong>Et alors?</strong>

Je suis en train de développer un module qui reroute les appels de service. L'adresse du service à appeler est donc mis à jour lors de la traversée du module en question. Pour faire bête, je me suis dit que j'aller spécifier un jeu d'adresses dans un bon vieux fichier de propriétés et que j'allais mettre ce fichier dans le module. OK, c'est bien, ça marche (heureusement...), je peux charger mon fichier dans le <em>Handler</em> du module et utiliser les adresses qui y sont définies.

Ce qui m'embête, c'est que ce fichier est packagé dans l'archive du module et donc non modifiable à souhait... C'est là que le C<em>lassloader</em> d'Axis2 entre en piste. Si je mets un fichier avec le même nom dans les ressources de mon code client, celui ci est pris en compte en priorité lors de l'appel par le module de <em>MonModule.class.getResource("/foo.properties");</em> Je peux donc cette fois ci livrer un module générique qui inclus des valeurs par défaut et les clients qui utilisent ce module peuvent 'overrider' les valeurs par défaut dans leur propre fichier de configuration.

Encore une fois, merci Axis2.
