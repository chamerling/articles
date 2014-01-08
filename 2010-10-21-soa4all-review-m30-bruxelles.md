---
layout: post
title: SOA4All Review M30 - Bruxelles
categories:
- SOA4All
tags:
- cloud
- dsb
- esb
- federation
- opensource
- PEtALS
- petalslink
- SOA
- SOA4All
- WebService
status: publish
type: post
published: true
meta:
  geo_latitude: '50.858507'
  geo_longitude: '4.368510'
  geo_accuracy: '166000'
  geo_address: Koninginneplein, 1030 Schaerbeek, Belgium
  geo_public: '0'
  _wp_old_slug: ''
  _wpas_mess: ! 'SOA4All Review M30 - Bruxelles: http://wp.me/pcSx0-cm'
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:58:"http://chamerling.files.wordpress.com/2010/10/img_1025.jpg";s:6:"images";a:1:{s:58:"http://chamerling.files.wordpress.com/2010/10/img_1025.jpg";a:6:{s:8:"file_url";s:58:"http://chamerling.files.wordpress.com/2010/10/img_1025.jpg";s:5:"width";s:3:"450";s:6:"height";s:3:"520";s:4:"type";s:5:"image";s:4:"area";s:6:"234000";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-01-07
    13:41:54";}
  _ideation_attached_images: '778'
  _elasticsearch_indexed_on: '2010-10-21 21:20:49'
---
<a href="http://chamerling.files.wordpress.com/2010/10/img_1025.jpg"><img class="alignleft size-full wp-image-778" title="IMG_1025" src="http://chamerling.files.wordpress.com/2010/10/img_1025.jpg" alt="" width="270" height="312" /></a>

Bon, comme d'habitude, beaucoup de choses autour de la sémantique, sémantique de services, linked data, RDF et compagnie mais surtout l'occasion de montrer les avancées et faire un point sur le Distributed Service Bus ie le bus servant de base de l'infrastructure de service du projet.

Pour faire court, le scope à beaucoup varié et divergé de ce que nous pensions faire initialement. A la base, nous pensions nous engager vers le développement d'un bus de service massivement distribué à l'échelle d'Internet ie une extension de <a class="zem_slink" title="Petals ESB" rel="homepage" href="http://petals.ow2.org">Petals ESB</a> qui lui est tout juste distribué. Au final, cet aspect massivement distribué est forcément complexe et demande encore de la réflexion (surtout sur son utilité). Le scope s'est recadré vers quelque chose qui est plus dans l'air du temps en se rapprochant d'une solution <a class="zem_slink" title="Service-oriented architecture" rel="wikipedia" href="http://en.wikipedia.org/wiki/Service-oriented_architecture">SOA</a> pour le Cloud.
Bref, on parle aujourd'hui de Federated Distributed Service Bus (fDSB) ou des réseaux/îlots de noeuds (les DSBs) sont interconnectés de façon intelligente pour créer des infrastructures fédérées. Pour ceux qui connaissent un peu Petals ESB, cela veut à peu près dire (et pout faire simple) que ces îlots ont une connaissance locale de leurs services. La connaissance des autres services n'est possible qu'a travers la couche de fédération et via des politiques de propagation des informations bien définies (un outils de gouvernance tel que Petals Master peut jouer ce role de définition des visibilités).
La couche de fédération est aujourd'hui le medium qui permet de faire communiquer ces ilots de DSBs via des protocoles hétérogènes, traverser les firewalls etc etc. On arrive alors vers les prémices du so-called "Hybrid Cloud" ie créer un Cloud SOA au sens large en utilisant du Cloud public (<a class="zem_slink" title="Amazon EC2" rel="homepage" href="http://aws.amazon.com/ec2/">Amazon EC2</a> par exemple), du Cloud privé (via OpenNebula et compagnie), etc.
Et c'est bien le cas d'usage que l'on à dans les cartons : des DSBs sur Amazon EC2, des DSBs sur Grid5000, des DSBs sur les laptops et des invocations de services possibles (dans tous les sens et sans connaissance préalable des services disponibles).
Pour le moment on ne parle pas d'élasticité et compagnie qui font la force (à mon avis) du Cloud. Ca c'est autre chose que je suis en train de préparer en parallèle, qui devrait voir le jour dans les prochains mois et dont je parlerais sûrement à la prochaine "OW2 Annual Conference" dans un mois à <a class="zem_slink" title="La Cantine" rel="homepage" href="http://lacantine.org/">La Cantine</a> - Paris.
