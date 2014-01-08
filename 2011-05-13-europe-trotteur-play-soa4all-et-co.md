---
layout: post
title: Europe-Trotteur, Play, SOA4All et co.
categories:
- PetalsLink
- Play
- SOA
- SOA4All
tags:
- Brussels
- cep
- Complex Event Processing
- dsb
- esb
- France
- opensource
- Petals ESB
- petalslink
- play
- Service-oriented architecture
- SOA4All
- software
- trip
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_skip_yup: '1'
  _wpas_done_twitter: '1'
  _wpas_mess: ! '[BLOG] #soa #trip Europe-Trotteur, Play, SOA4All et co.:'
  tagazine-media: a:7:{s:7:"primary";s:61:"http://farm6.static.flickr.com/5269/5693275664_14d6c00ef9.jpg";s:6:"images";a:1:{s:61:"http://farm6.static.flickr.com/5269/5693275664_14d6c00ef9.jpg";a:6:{s:8:"file_url";s:61:"http://farm6.static.flickr.com/5269/5693275664_14d6c00ef9.jpg";s:5:"width";s:3:"500";s:6:"height";s:3:"375";s:4:"type";s:5:"image";s:4:"area";s:6:"187500";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-05-13
    09:21:42";}
  _oembed_042edcb7ff8f37a2bf93b6756ba023bf: ! '{{unknown}}'
  _oembed_e331445f5540a4f80f2ae0b99b36fa22: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2011-05-13 09:21:42'
---
Les deux dernières semaines ont été bien remplies, en passant par Athènes pour une réunion de travail sur le projet <a href="http://play-project.eu/" target="_blank">Play</a> puis par Bruxelles pour la revue finale du projet <a href="http://soa4all.eu" target="_blank">SOA4All</a> (après un détour par Montpellier pour coder un peu), il est temps de faire un bilan de tout cela...

[caption id="" align="aligncenter" width="500" caption="Athènes - Mai 2011"]<a href="http://www.flickr.com/photos/hamerling/5693275664/"><img title="Athènes - Mai 2011" src="http://farm6.static.flickr.com/5269/5693275664_14d6c00ef9.jpg" alt="Athènes - Mai 2011" width="500" height="375" /></a>[/caption]
<p style="text-align:center;"></p>

<h1>SOA4All</h1>
Toute personne déjà venue sur ce blog devrait connaitre un minimum ce projet qui m'a occupé ces trois dernières années. C'est bon je ne devrais plus en parler trop puisque il est maintenant officiellement terminé depuis la fin du mois dernier. La revue finale s'est déroulée aujourd'hui à Bruxelles. Pas de présentation technique pour cette fois, j'ai même pas eu besoin de dire un mot... La session du matin était assez spéciale pour un projet recherche: essayer de vendre le projet a un investisseur (avec un vrai investisseur, mais qui venait sans son chéquier). Bref un jeu de rôle que je trouve un peu grotesque et qui au final n'a rien donné a part trop de travail pour les speakers.

Regardons ce que ce projet a pu m'apporter, apporter à PetalsLink et aux produits associés, les points forts, les points faibles, etc, (en essayant de faire court):
<ul>
	<li><strong>+</strong> Mon premier projet de recherche européen. Je suis entré dedans dès le début après avoir passé 2 ans a bosser sur Petals ESB à temps plein en tant que développeur, product leader et compagnie. J'avais par le passé travaillé sur un projet ANR pendant presque 3 ans aussi sur le thème du grid computing (cf <a href="http://grid-tlse.org" target="_blank">http://grid-tlse.org</a>).</li>
	<li><strong>+</strong> <a href="http://petals.ow2.org" target="_blank">Petals ESB</a> n'avait jamais été trituré de la sorte (ndlr: faut que je trouve un autre mot que trituré, je l'ai déjà utilisé hier plusieurs fois). La version utilisée dans SOA4All diffère énormément de la version communautaire OW2. En plus de l'ajout de nombreux services, il a été aussi customisé dans tout les sens pour qu'il soit non plus 'Enterprise Service Bus' mais 'Internet Service Bus'. Les protocoles et contraintes de la version entreprise n'étant pas forcément les mêmes que ceux d'Internet.</li>
	<li><strong>+</strong> Le Distributed Service Bus qui en découle est maintenant une base pour de nombreux projets de recherche chez PetalsLink, donc on en entendra encore parler sur ce blog.</li>
	<li><strong>-</strong> Les partenaires du projet avaient une toute autre vision de ce qu'est un middleware, dès la première réunion on a entendu plusieurs fois 'The middleware is the Internet'. Donc là forcément çà commence pas trop bien.</li>
	<li><strong>+</strong> Heureusement, l'INRIA était là. J'avais déjà eu le plaisir de travailler avec l'INRIA Lyon sur Grid-TLSE, et généralement ces personnes là savent de quoi elles parlent et savent ce qu'est un middleware. On travaille d'ailleurs ensemble sur de nouveaux projets dont Play.</li>
	<li><strong>-</strong> '<em>SOA pour tous</em>' mais pas pour le projet lui même... Le but du DSB était aussi de fournir cette brique SOA de base. On se retrouve avec des développeurs qui font tous leurs Webapps dans leur coin et qui s'intégrent 'à la mano'. Dommage il faut recompiler tout si le service est déployé ailleurs...J'ai presque honte de dire la taille de l'installer graphique que j'ai été chargé de faire pour installer les différents modules; allez 800Mo et encore tout les modules ne sont pas présents.</li>
	<li><strong>+</strong> C'est fini, maintenant on va pouvoir faire des vrais projets midleware, SOA et compagnie.</li>
</ul>
<h1>Play</h1>
Le projet Play a commencé depuis un peu plus de six mois (je ne parle toujours pas de <a href="http://www.playframework.org/" target="_blank">Play! Framework</a> mais vous devriez vous intéresser à ce Framework Web, le premier tuto est assez bluffant quand on a l'habitude de faire du Spring, du Struts, du GWT ou tout autre Framework Web, Play! est en train de percer et de botter le cul aux autres). Bien que le projet soit encore au début (durée 36 mois), l'activité est assez intense et promet de bonnes choses dans un futur proche. Pour faire rapide, il est question de développer une plateforme événementielle qui utilisera des technos issues/basées sur le Distributed Service Bus, de Complex Event Processing, de P2P, d'adaptation contextuelle, le Cloud, etc, etc, etc... Bref, beaucoup de choses à faire et un Bus de Service, qui va évoluer encore et encore. Ah et on parle aussi de gouvernance au niveau service et évènements, ce qui est un peu plus nouveau pour la partie évènementielle.
Donc la réunion de la semaine dernière à Athènes (ça change de Bruxelles...) a encore une fois tenue ses promesses: du contenu en masse, des cas d'usage qui évoluent pour utiliser au mieux la plateforme et le concept d'Event Marketplace qui se précise. Je pense que je parlerais mieux de tout ça d'ici peu de temps mais les bases sont posées.
<h1>Codons!</h1>
Ces voyages au sud et au nord de l'Europe m'ont laissé le temps de coder (surtout quand le TGV tombe en panne entre Montpellier et Bruxelles), <a title="Jouons avec le Petals Kernel et Apache CXF" href="http://chamerling.org/2011/05/12/jouons-avec-le-petals-kernel-et-apache-cxf/" target="_blank">un article sur une nouvelle feature</a> du <a href="http://research.petalslink.org/display/petalsdsb/PetalsDSB+Overview" target="_blank">DSB</a> a vu le jour hier soir, j'en ferais un autre plus précis dans les jours qui viennent.
