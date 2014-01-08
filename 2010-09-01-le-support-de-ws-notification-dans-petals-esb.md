---
layout: post
title: Le support de WS-notification* dans Petals ESB
categories:
- PEtALS
- SOA
tags:
- architecture
- distributed
- eda
- esb
- jbi
- notification
- ow2
- PEtALS
- Petals ESB
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
  _wp_old_slug: ''
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:1:{s:65:"http://upload.wikimedia.org/wikipedia/en/5/5f/PEtALS_ESB_logo.png";a:6:{s:8:"file_url";s:65:"http://upload.wikimedia.org/wikipedia/en/5/5f/PEtALS_ESB_logo.png";s:5:"width";s:3:"200";s:6:"height";s:3:"200";s:4:"type";s:5:"image";s:4:"area";s:5:"40000";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-09-01
    07:48:58";}
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2010-09-01 07:48:58'
---
<div class="zemanta-img">

[caption id="" align="alignright" width="200" caption="Image via Wikipedia"]<a href="http://en.wikipedia.org/wiki/File:PEtALS_ESB_logo.png"><img title="Petals ESB" src="http://upload.wikimedia.org/wikipedia/en/5/5f/PEtALS_ESB_logo.png" alt="Petals ESB" width="200" height="200" /></a>[/caption]

</div>
<em>Je profite de passer un peu de temps à aider les collègues de l'équipe produit sur le sujet des notifications dans </em><a class="zem_slink" title="Petals ESB" rel="homepage" href="http://petals.ow2.org"><em>Petals ESB</em></a><em> qui est assez récent dans la suite Petals pour proposer un article sur une vision de haut niveau du support des notifications dans le bus de service distribué.</em>
<h3>L'architecture et l'intégration</h3>
Je ne reviens pas ici sur l'architecture de Petals ESB basée sur <a class="zem_slink" title="Java Business Integration" rel="wikipedia" href="http://en.wikipedia.org/wiki/Java_Business_Integration">JBI</a> (il y a assez d'articles la dessus sur mon blog) mais je voudrais parler de l'architecture choisie pour fournir un support aux notifications dans Petals. Malgré tout le support JBI est important ici car cette intégration est basée sur l'aspect composants JBI de Petals. Pour rappel, un composant JBI est une sorte de plugin qui vient enrichir le container JBI en offrant des fonctionnalités supplémentaires en tant que connecteur protocolaire ou de moteur service (les fameux Binding Components et Service Engines).
L'intégration des notifications est donc exclusivement basée sur des composants JBI via des composants dédiés d'une part et via une intégration dans le Kit de Développement de Composants (CDK) développé au sein du projet.

Du point de vue des spécifications, il est choisit de fournir une implémentation des spécifications <a href="http://www.oasis-open.org/committees/tc_home.php?wg_abbrev=wsn">WS-Notification de chez Oasis</a>. Une vision simpliste de cette spécification fait intervenir un broker (un intermédiaire intelligent) pour gérer et découpler les émetteurs de notifications et les destinataires. C'est sur ce broker que les émetteurs s'enregistrent, et c'est aussi sur lui que les destinataires informent qu'ils sont intéressés par uu/des types de notifications précises. Pour plus de détails, je vous recommande de jeter un coup d'oeil à la spec elle même ou à Googler le sujet.

Du point de vue de l'intégration, le broker est implémenté dans un composant JBI dédié, le petals-se-notification. Comme tout le travail se passe généralement dans les autres composants JBI, le CDK (qui est le support pour le développement des composants) propose une activation du support des notifications en tant que producteur. Cette activation implique un enregistrement envers le broker dès son démarrage. Malgré cette activation, si aucun producteur n'est enregistré au niveau du broker, il ne se passera rien! En effet, l'architecture actuelle force les consommateurs de notifications à informer le broker de leur interêt pour des notifications via un système de topics. Je passe vite la dessus mais en gros, lorsque le consommateur s'enregistre au niveau du broker, le broker va relayer cette demande avec un contexte associé aux producteurs de notifications. Une fois cette étape achevée, des échanges de messages standard provoqueront potentiellement des notifications envoyées au broker qui fera ensuite son travail de relai.

Les principaux points forts de cette intégration sont :
<ol>
	<li>la possibilité d'utiliser ce médium de notifications pour tracer les échanges entre consommateurs et fournisseurs de services standards. Ces informations peuvent alors être remontées vers une console de BAM par exemple.</li>
	<li>L'utilisation du canal JBI pour échanger les notifications. Pas de nouveau canal de communication à créer, tout passe par les couches de communications du bus de service. Comme Petals est un bus de service nativement distribué, cette approche est vraiment un point important.</li>
	<li>Le support rapide via un CDK qui fournit toute la mécanique nécessaire pour implémenter ses producteurs et consommateurs de notifications.</li>
</ol>
Cette intégration a bien sûr des points faibles. Premièrement, il faut absolument que le composant qui fait acte de broker soit présent et robuste pour pouvoir gérer les notifications venant de N fournisseurs en tant que point central. Ensuite, ce broker doit (pour le moment) être unique dans le contexte du bus. En placer un autre sur un autre noeud du bus n'est pas possible actuellement (enfin si c'est possible mais des messages seront perdus).
<h3>La suite?</h3>
De nouveaux projets sont au programme en cette fin d'année avec pour but (un des buts plutôt) d'aller vers une architecture full EDA intégrée dans le coeur du bus. Cela va normalement supprimer les points négatifs cités précédemment avec notamment la suppression des composants JBI et le développement d'un broker distribué se basant sur l'architecture distribuée de Petals. Cela fera bien sûr de la matière pour les prochains articles de ce blog courant 2011.

En attendant, un prochain article portera surement sur quelque chose de plus technique avec un peu de code, je conseille de faire tourner la démo (<a href="http://doc.petalslink.com/display/petalsview/Tutorial">disponible ici</a>) qui met en oeuvre Petals ESB, Petals View et les notifications. Elle éclaircira mes dires, car le blahblah c'est bien, passer à l'action c'est mieux!
