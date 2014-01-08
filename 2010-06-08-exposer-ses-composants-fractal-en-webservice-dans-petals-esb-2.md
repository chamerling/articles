---
layout: post
title: ! 'Exposer ses composants Fractal en Webservice dans Petals ESB #2'
categories:
- PEtALS
tags:
- cxf
- esb
- fractal
- jaxws
- ow2
- PEtALS
- SOA
- WebService
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wp_old_slug: ''
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-06-08
    09:19:23";}
  _elasticsearch_indexed_on: '2010-06-08 09:19:23'
---
Dans un <a href="http://chamerling.wordpress.com/2009/10/07/exposing-fractal-components-as-webservices-in-petals-esb/">précédent article</a> sur l'exposition de composants Fractal en Web service dans Petals ESB, je décrivais la démarche technique à suivre qui comprenait un certain nombre d'étapes (contraignantes), telles que :
<ol>
	<li>Avoir l'obligation de créer un composant spécifique par service à exposer</li>
	<li>Avoir l'obligation d'implémenter une interface spécifique (KernelWebService)</li>
	<li>Avoir l'obligation de décrire et de binder le service dans les fichiers de description ADL</li>
</ol>
Disons que cette approche est finalement assez simple mais devient vite lourde lors que l'ajout de services. La solution du jour va supprimer les 3 points cités ci dessus. Comment?
<ol>
	<li>Un composant implémentant une interface annoté en JAXWS doit être la condition suffisante pour être exposé en Web service</li>
	<li>Un composant spécifique est en charge d'exposer les composants Fractal qui satisfont la condition 1.</li>
</ol>
Le composant introduit dans le point 2 ci dessus, que l'on appellera le WSM ('Web service Manager') est le composant qui introspecte le composite Fractal dans lequel il est instancié. Il a ainsi la visibilité des composants qui implémentent une interface annotée en JAXWS et peut l'exposer, ici en utilisant Apache CXF.
En allant plus loin, on peut facilement imaginer avoir un WSM de haut niveau qui peut se balader dans l'arbre complet du modèle Fractal et exposer tout les services du bus (ou une partie que l'on juge intéressante par filtrage).

Note pour l'équipe : Ce code est disponible sur la forge de mon projet recherche, disponible sur demande ;)
