---
layout: post
title: Créer sa couche de transport pour Petals ESB en 4 classes
categories:
- PEtALS
tags:
- cxf
- Java
- jbi
- module
- opensource
- ow2
- PEtALS
- SOA
- WebService
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-08-13
    13:59:07";}
  _elasticsearch_indexed_on: '2010-08-13 13:59:07'
---
Je profite d'avoir les doigts bien chauds (je viens de passer quelques jours à faire du M$ Word à plein temps, dure reprise après des vacances au grand air...) pour écrire un article que j'ai dans la tête depuis un petit moment : Comment développer sa propre couche de transport pour OW2-Petals ESB. Pour savoir ce qu'est la couche de transport dans Petals, allez jeter un coup d'oeil les précédents articles .

<em>Note : Ce travail se base sur Petals ESB 3.0.2 mais est normalement compatible avec la v3.x. Tout le code est disponible sur le SVN du projet Petals ESB chez OW2 (le lien plus bas) et je ne l'ai pas inséré dans l'article car l'intérêt n'est pas de montrer que je sais coder en Java...</em>
<h3><strong>Le contexte</strong></h3>
Petals ESB v3.x est livré avec une couche de transport permettant de faire communiquer les instances du bus de service basée sur une implémentation Java NIO. Le but de l'article est de présenter un framework de couche de transport et de montrer que développer une nouvelle couche en se basant sur le framework est simple et rapide.
<h3><strong>Le framework</strong></h3>
Le but n'est pas de détailler complètement le framework, mais juste les points d'extension qu'il offre. Le framework gère la logique d'envoi synchrone et asynchrone des messages, ce qu'il reste à implémenter est finalement juste la partie permettant de recevoir des messages (ie le serveur) et d'envoyer les messages sur le noeud distant (indépendamment de la logique Petals).
<ul>
	<li>Le serveur: Il doit être capable de recevoir un message de la forme de son choix (tout dépend du protocole utilisé) et d'informer le framework qu'un nouveau message est recu après l'avoir converti en message Petals (ici JBI), rien de plus...</li>
	<li>Le client: Doit pouvoir retrouver le serveur à appeler et lui envoyer le message après avoir transformé le message Petals (JBI) en message serveur-dépendant.</li>
</ul>
Le  framework est basé sur une abstraction de la couche de transport Java NIO de Petals ESB v3 et son code source est disponible dans ma <a href="http://websvn.ow2.org/listing.php?repname=petals&amp;path=%2Fsandbox%2Fchamerling%2Fmodules%2F" target="_blank">sandbox</a> dans les projets petals-transport-api et petals-transport.
<h3><strong>Une implémentation Web service?</strong></h3>
Oui parce que c'est la plus rapide et qu'elle est bien utile! Allez pour faire encore plus simple (et pour ne pas changer une équipe qui gagne), on va utiliser JAXWS &amp; Apache CXF.
<h4>1. Le coté serveur</h4>
Il est en charge de démarrer le service de réception de messages, ici on va utiliser JAXWS pour exposer une instance de <em>org.ow2.petals.transport.cxf.TransportServiceImpl</em> qui implémente <em>org.ow2.petals.esb.api.TransportService</em>.

L'implémentation cré un message JBI depuis le message reçu par la pile Web service (via la méthode/opération <em>#receive</em>), et passe ce message au <em>Receiver</em> en appelant l'unique méthode #<em>onMessage(MessageExchange)</em>. Le receiver n'est ici rien d'autre que le composant de transport du Framework (<em>org.ow2.petals.transport.TransporterImpl</em> dans le module petals-transport) qui implémente le service de transport de Petals ESB (<em>org.ow2.petals.transport.Transporter</em>) et qui possède l'intelligence pour faire remonter le message dans les couches de Petals ESB.
<h4>2. Le coté client</h4>
Ici aussi, rien de bien méchant. Il suffit de savoir créer un client Web service pour parler au serveur que l'on vient de présenter au point 1. Une fois encore, JAXWS et CXF font ca bien. Une factory est à implémenter (<em>org.ow2.petals.transport.api.ClientFactory</em>) et permet de créer des instance du client pour chaque container Petals ESB à invoquer.
<h4>3. What else?</h4>
Un peu de configuration via Fractal et le tour est joué. Une nouvelle couche de transport permet de faire communiquer les instances de Petals ESB via Web service au lieu de passer par NIO, tout cela avec quelques classes...
<h3><strong>Et alors?</strong></h3>
Avec quelques classes (même pas une dizaine, dans le meilleur des cas implémenter 4 interfaces doit être suffisant), on peut créer un nouveau type de transporter pour Petals ESB sans se soucier de :
<ol>
	<li>JBI. Ou presque la seule chose à faire est créer un message JAXWS depuis un message JBI et inversement (ca va il y a plus dur...)</li>
	<li>La logique de message synchrone et asynchrone. Tout cela est géré par le framework.</li>
	<li>La remontée du message vers le bon fournisseur de service.</li>
</ol>
On focalise ici vraiment sur la partie communication pure entre client et serveur. Pour valider le principe d'implémentation facile et rapide, j'ai créé un transporteur XMPP en à peu près un jour... Le code de ce transporteur XMPP n'est pas encore public mais ca ne devrait pas tarder. Il me faut juste un peu de temps...

Tout cela pour montrer qu'utiliser Petals ESB n'est pas limitant. L'architecture permet quand même des points d'extension intéressants, il faut juste s'y connaitre un peu (beaucoup). C'est juste notre métier...
