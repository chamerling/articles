---
layout: post
title: Sur l'utilité d'utiliser des standards "Internet-Friendly"...
categories:
- PEtALS
- SOA4All
tags:
- distributed
- esb
- gwt
- Java
- jbi
- management
- opensource
- ow2
- PEtALS
- SOA
- SOA4All
- WebService
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:71:"http://chamerling.files.wordpress.com/2010/04/soa4all-dsbmanagement.png";s:6:"images";a:2:{s:58:"http://chamerling.files.wordpress.com/2010/04/img_3892.jpg";a:6:{s:8:"file_url";s:58:"http://chamerling.files.wordpress.com/2010/04/img_3892.jpg";s:5:"width";s:3:"800";s:6:"height";s:3:"533";s:4:"type";s:5:"image";s:4:"area";s:6:"426400";s:9:"file_path";s:0:"";}s:71:"http://chamerling.files.wordpress.com/2010/04/soa4all-dsbmanagement.png";a:6:{s:8:"file_url";s:71:"http://chamerling.files.wordpress.com/2010/04/soa4all-dsbmanagement.png";s:5:"width";s:4:"1449";s:6:"height";s:3:"753";s:4:"type";s:5:"image";s:4:"area";s:7:"1091097";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"2";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-04-27
    14:40:52";}
  _ideation_attached_images: '450'
  _elasticsearch_indexed_on: '2010-04-27 14:40:52'
---
... et aussi sur l'utilité de développer ses applications de facon générique. La preuve par l'exemple : L'administration du bus de service <a href="http://petals.ow2.org">Petals ESB</a>.

JMX c'est bien pour manager une application Java mais il faut savoir parler JMX et généralement, parler à une application JMX via Internet c'est pas forcément le pied et encore moins quand l'administrateur réseau ne veut pas qu'il y ait autre chose qui rentre chez lui que du bon vieux HTTP et être ce que j'appelle ici 'Firewall-Friendly'.
<p style="text-align:center;"></p>


[caption id="attachment_448" align="aligncenter" width="632" caption="Pas Firewall-Friendly?"]<a href="http://chamerling.files.wordpress.com/2010/04/img_3892.jpg"><img class="size-full wp-image-448" title="Tentative de passage de Firewall" src="http://chamerling.files.wordpress.com/2010/04/img_3892.jpg" alt="Tentative de passage de Firewall" width="632" height="421" /></a>[/caption]

Bref, je milite ici sur l'utilité d'avoir une administration d'application générique qui peut être exposée dans la technologie et le protocole qui colle le mieux au contexte. Et la je parle principalement de ce que je connais le mieux : Petals ESB. Ce bus de service est fortement basé sur la spécification JBI. Il est tellement lié que l'administration du bus ne passe que par JMX, même pour effectuer des opérations internes au bus... L'administration ne doit donc pour moi être fournie que sous forme d'API et d'implémentation indépendante de toute technologie d'exposition. A charge du développeur de l'application de fournir les mécaniques adéquates pour exposer cette API en JMX, en Web Services, en REST, etc... Cela fera surement l'objet d'un article sur une mise en pratique de ce genre d'approche dans Petals.

Le but principal et original de cet article était de parler un peu de ce que l'on peut faire en terme d'administration et de monitoring du Bus Petals. Je travaille depuis quelques temps déjà sur un projet visant à créer une infrastructure de service largement distribuée sur Internet dans le cadre du projet de recherche <a href="http://soa4all.eu">SOA4All</a>. Je viens de finir un prototype d'application Web qui permet de manager ces noeuds exposés sur Internet et d'effectuer quelques opérations qui ne sont pas disponibles dans une distribution standard de Petals (tout du moins l'API de base ne les fournies pas). Comment? Et bien forcément sans utiliser JMX (j'en suis assez allergique ce qui rajoute un allergène à ma liste déjà longue, en plus c'est le printemps...), mais plutôt en utilisant et en étendant l'API Web service que j'ai développé pour Petals 3.0. Cette application permet alors de se connecter à n'importe quelle instance de Petals (et à son réseau) exposée sur Internet via une API WS et ainsi de lier des services externes au bus, d'exposer des services internes, de voir ce qui se passe dans le bus et bien plus encore... Tout cela en utilisant GWT pour créer une application sympa avec des choses qui bougent car Petals à une activité qu'il faut pouvoir montrer en direct!
<p style="text-align:center;"></p>


[caption id="attachment_450" align="aligncenter" width="632" caption="DSB Manager Portal"]<a href="http://chamerling.files.wordpress.com/2010/04/soa4all-dsbmanagement.png"><img class="size-full wp-image-450" title="DSB Manager Portal" src="http://chamerling.files.wordpress.com/2010/04/soa4all-dsbmanagement.png" alt="DSB Manager Portal" width="632" height="328" /></a>[/caption]
