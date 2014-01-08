---
layout: post
title: Pensées sur le Cloud Hybride
categories:
- Cloud
tags:
- Amazon EC2
- Cloud computing
- hybrid cloud
- Open source
- opennebula
- ow2
- PEtALS
- petalslink
- SOA
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_done_yup: '1'
  _wpas_mess: ! 'Pensées sur le Cloud Hybride: http://wp.me/pcSx0-fH'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:63:"http://farm6.static.flickr.com/5245/5340786532_bf27d953c8_z.jpg";s:6:"images";a:1:{s:63:"http://farm6.static.flickr.com/5245/5340786532_bf27d953c8_z.jpg";a:6:{s:8:"file_url";s:63:"http://farm6.static.flickr.com/5245/5340786532_bf27d953c8_z.jpg";s:5:"width";s:3:"640";s:6:"height";s:3:"427";s:4:"type";s:5:"image";s:4:"area";s:6:"273280";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-01-13
    20:49:30";}
  _elasticsearch_indexed_on: '2011-01-13 14:45:58'
---
Dans la galaxie de termes et de technologies que contient le Cloud Computing aujourd'hui, attardons nous un peu sur le Cloud Hybride. Un Cloud Hybride est grosso-modo un mix entre un Cloud public (du type EC2) et un Cloud privé (managé par un Framework à la <a href="http://opennebula.org" target="_blank">OpenNebula</a>).

Je pense vraiment que ce genre d'approche est la plus intéressante à mettre en oeuvre. Il est clair que les entreprises ne sont pas prêtes (et ne le seront sûrement jamais) à basculer tout leur système dans le Cloud public. On pense principalement au problème qu'il peut y avoir au niveau de la sécurité des données. Par contre, on peut imaginer qu'une partie des ressources (au sens large) peut potentiellement être déportée dans un Cloud public. Dès lors que l'on fait cohabiter du Cloud public et du Cloud privée, on passe donc dans le Cloud hybride. Simple? Pas tout a fait. Bien que le concept soit à mon avis le plus intéressant, il est aussi le plus dur à mettre en oeuvre.
Heureusement, des frameworks Cloud, tels que OpenNebula (<a title="OpenNebula, Acte 1: Les présentations" href="http://chamerling.org/2010/12/17/opennebula/">brièvement présenté dans un précédent article</a>) permettent de créer et de gérer des Clouds hybrides de façon transparente. Dans un de <a href="http://blog.opennebula.org/?p=1161" target="_blank">leur récent article</a>, l'équipe qui est dérriere OpenNebula présente quelques cas d'usages qui répondent au besoin du Cloud hybride. Dans '<a href="https://support.opennebula.pro/entries/366704-scaling-out-web-servers-to-amazon-ec2" target="_blank">Scaling out Web Servers to Amazon EC2</a>', ils présentent comment on peut load-balancer une application Web en utilisant des ressources privées et publiques sur Amazon EC2. L'intérêt est ici de pouvoir fournir une qualité de service 'constante' en utilisant des ressources privées et d'avoir la possibilité de répondre à des pics de charge momentanés en utilisant des ressources publiques louées ponctuellement.

Cette approche est un bon compromis. Je ne crois vraiment pas au 'tout public'. En me plaçant au niveau purement technique, je n'y vois aucun intérêt et si c'était le cas, Petals* serait déjà dans le Cloud depuis un bon moment. Par contre au niveau business, on pourrait alors dire que l'on fait du Cloud comme certains de nos concurrents...

Une approche SOA dans le Cloud hybride est donc plus dans la mouvance de ce que je veux pousser et développer cette année. Construire un bus de services pouvant s'adresser et utiliser les fonctionnalités du Cloud hybride me parait vraiment un bon challenge, et qui plus est, qui peut le plus peut le moins...

[caption id="" align="alignnone" width="640" caption="Ca a l&#039;air compliqué, mais je vais quand même y aller"]<a href="http://www.flickr.com/photos/hamerling/5340786532/"><img title="Orage" src="http://farm6.static.flickr.com/5245/5340786532_bf27d953c8_z.jpg" alt="Ca a l'air compliqué, mais je vais quand même y aller" width="640" height="427" /></a>[/caption]
