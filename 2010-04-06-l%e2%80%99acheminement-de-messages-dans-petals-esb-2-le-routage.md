---
layout: post
title: ! 'L’acheminement de messages dans Petals ESB : #2 Le routage'
categories:
- PEtALS
tags:
- bus
- documentation
- esb
- Java
- jbi
- module
- opensource
- ow2
- PEtALS
- petalspourlesnuls
- routage
- router
- service
- SOA
- tip
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:56:"http://chamerling.files.wordpress.com/2010/04/router.jpg";s:6:"images";a:1:{s:56:"http://chamerling.files.wordpress.com/2010/04/router.jpg";a:6:{s:8:"file_url";s:56:"http://chamerling.files.wordpress.com/2010/04/router.jpg";s:5:"width";s:3:"377";s:6:"height";s:3:"448";s:4:"type";s:5:"image";s:4:"area";s:6:"168896";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-05-20
    09:47:43";}
  _ideation_attached_images: '413'
  _elasticsearch_indexed_on: '2010-04-06 14:45:11'
---
<strong>Intro</strong>

Dans <a href="http://chamerling.wordpress.com/2010/03/29/lacheminement-de-messages-dans-petals-esb-1-lintro/">l'article précédent</a> sur l'acheminement de messages dans Petals ESB, j'introduisais les notions, aujourd'hui nous allons regarder en détails la couche de routage du bus. Cette couche est largement appelée Router par les développeurs du Bus, et le sera donc dans cette article et dans la suite de la série.

<strong>L'archi simplifiée</strong>

L'architecture du Router utilise la notion très répandue et  fort utile de modules ie le Router invoque séquentiellement une liste de modules pour élire les services à appeller. A la fin de la traversée on se retrouve avec toutes les informations nécessaires pour appeler les services. On peut modifier cette liste en ajoutant ou supprimant des modules par configuration. Un schéma simplifié donne un Router avec cette tête :
<p style="text-align:center;"><a href="http://chamerling.files.wordpress.com/2010/04/router.jpg"><img class="aligncenter size-full wp-image-413" title="router" src="http://chamerling.files.wordpress.com/2010/04/router.jpg" alt="" width="377" height="448" /></a></p>
Plus de détails sur l'utilisation des modules dans Petals et leur injection <a href="http://chamerling.wordpress.com/2010/01/21/adding-routing-modules-in-petals-esb/">dans un vieil article</a> (à non pas si vieux...).

<strong>L'implémentation</strong>

L'implémentation de base de Petals ESB est composée de 2 modules en émission :
<ol>
	<li>Le module qui interroge le registre de services et retourne une liste de point d'accès (endpoint dans le jargon service) en fonction des informations contenues dans le message d'entrée.</li>
	<li>Le module de résolution de la couche de transport. En fonction de la localisation des endpoints trouvés par le module précédent, un contexte est créé pour définir la couche de transport à utiliser (cette couche de transport sera détaillée dans le prochain article de la série).</li>
</ol>
On peut imaginer un grand nombre d'utilisations de ces modules. Personnellement, je les utilise à outrance par exemple pour :
<ul>
	<li>Ajouter des informations de timestamp sur les messages</li>
	<li>Notifier une couche de monitoring que des messages sont échangés entre consommateurs et fournisseurs de service</li>
	<li>Logger les appels</li>
	<li>Mettre à jour le choix de la couche de transport avec ma propre implémentation du transport de messages inter container</li>
	<li>...</li>
</ul>
<strong>Outro</strong>

Vous en savez un peu plus sur le routage dans le bus de service Petals. il faut vraiment retenir que cette couche est vraiment extensible et customisable sans grand effort, en effet pas besoin de recompiler le coeur de Petals pour ajouter des fonctionnalités pour la résolution des endpoints. Dans le prochain article de la série, nous regarderons de plus prêt comment sont échangés les messages entre les instances de Petals.
