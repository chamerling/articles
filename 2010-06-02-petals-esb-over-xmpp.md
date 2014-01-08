---
layout: post
title: ! 'Petals ESB over XMPP #2'
categories:
- PEtALS
tags:
- api
- callback
- distributed
- esb
- federation
- Google
- gtalk
- Java
- jbi
- module
- opensource
- ow2
- PEtALS
- SOA
- xmpp
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wp_old_slug: ''
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:53:"http://chamerling.files.wordpress.com/2010/06/fed.png";s:6:"images";a:1:{s:53:"http://chamerling.files.wordpress.com/2010/06/fed.png";a:6:{s:8:"file_url";s:53:"http://chamerling.files.wordpress.com/2010/06/fed.png";s:5:"width";s:3:"454";s:6:"height";s:3:"273";s:4:"type";s:5:"image";s:4:"area";s:6:"123942";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-07-20
    12:35:11";}
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _ideation_attached_images: '522'
  _elasticsearch_indexed_on: '2010-06-02 13:20:25'
---
Dans <a href="http://chamerling.wordpress.com/2010/05/27/communication-de-noeuds-petals-via-xmpp/">l'article précédent</a>, je parlais de l'utilisation de XMPP et de Google Talk pour faire communiquer des instances de Petals ESB. L'utilisation était seulement faite au niveau de la couche de transport de Petals (<a href="http://chamerling.wordpress.com/2010/05/20/l’acheminement-de-messages-dans-petals-esb-3-le-transport/">cf cet article sur la couche de transport</a>) et ne couvrait donc pas tous les canaux de communication qui entrent en jeu (et en particulier ceux du registre de services).
Dans la continuité de mes travaux sur la fédération de bus de service, je me suis donc intéressé à la possibilité de faire communiquer des domaines distincts (un domaine étant un ensemble de noeuds au sein d'une institution par exemple) en utilisant XMPP non pas seulement pour la couche de transport mais aussi pour les communications du registre de service.

En pratique, les domaines sont complètement indépendants, ils ne sont aucunement connectés via Internet ou toute autre connection alternative. Dans cette première itération sur la fédération, il n'y a pas de notion de hierarchie en domaine et sous domaine. Un noeud d'un domaine peut demander à la fédération une résolution de endpoint et aussi envoyer un message à des endpoints de la fédération pour invoquer des services. Une vue de haut niveau sur deux noeuds dans deux domaines différents est la suivante :

<a href="http://chamerling.files.wordpress.com/2010/06/fed.png"><img class="aligncenter size-full wp-image-522" title="fed" src="http://chamerling.files.wordpress.com/2010/06/fed.png" alt="" width="454" height="273" /></a>

Techniquement, l'intégration de la fédération intervient à plusieurs niveaux :
<ol>
	<li>Chaque noeud implémente le service de fédération qui permet de donner un point d'entrée à la fédération. Ce service expose une opération de recherche de endpoint et une opération d'invocation de service.</li>
	<li>Chaque noeud s'enregistre dans la fédération en donnant un callback (ici c'est le point d'accès au service exposé en 1 qui est publié dans la fédération).</li>
	<li>Un module de fédération permet de faire une recherche de endpoint dans la fédération. Dans le cas de Petals, c'est un module du routeur (cf cet article sur les modules) qui permet de faire cette recherche.</li>
	<li>Une implémentation au niveau fédération de la couche de transport permet d'aller invoquer un service de la fédération.</li>
</ol>
Une première approche simple (et pas forcément efficace) est de laisser le module de routage faire une requête à la fédération à chaque invocation pour trouver une liste de endpoints qui peut répondre à la requête, une version plus évoluée permettrait d'avoir un cache à un niveau intermédiaire (pas si évident en fait dans une approche très dynamique). A voir...

En attendant, un premier prototype permets de faire communiquer des noeuds appartenant à des domaines différents sans que ceux ci est une connaissance des autres noeuds (au niveau de Petals ESB, les noeuds des domaines ne sont pas renseignés au niveau du service de topologie). La vidéo qui suit montre deux noeuds, un sur mon laptop et un autre sur un serveur OVH. Je n'ai ouvert aucun port sur ma Box, sur mon laptop ou sur le serveur pour faire passer du trafic supplémentaire : C'est un des avantages à utiliser XMPP.
Le scénario est simple, un client sur mon laptop veut invoquer un service qui fournit une interface X. Cette interface X n'est fournie par aucun service localement (d'ailleurs on voit bien qu'il n'y a pas de endpoint sur le container coté client). Et la magie, il existe un endpoint dans la fédération qui fournit cette interface et il se trouve que cet endpoint est sur le container tournant sur le serveur OVH.
Et alors? Ca a l'air bête comme çà mais on arrive quand même à récupérer la référence du endpoint distant et à invoquer le service sans avoir besoin d'ouvrir quoi que ce soit, de faire du polling ou autre. Merci XMPP.
Comment? Pour dire simple, chaque noeud se connecte sur un serveur XMPP. En pratique c'est un peu plus compliqué et je laisse ca pour un prochain article!

[vimeo http://vimeo.com/26671357 w=500&amp;h=400]
