---
layout: post
title: ! 'L’acheminement de messages dans Petals ESB : #3 Le transport'
categories:
- PEtALS
tags:
- distributed
- esb
- opensource
- ow2
- PEtALS
- SOA
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-05-20
    10:20:30";}
  _elasticsearch_indexed_on: '2010-05-20 10:20:30'
---
<h2>L'intro</h2>
<a href="http://chamerling.wordpress.com/2010/04/06/l’acheminement-de-messages-dans-petals-esb-2-le-routage/">Dans le précédent article de la série</a>, j'ai présenté globalement les concepts de routage de message et comment la couche de routage de <a href="http://petals.ow2.org">Petals ESB</a> pouvait être étendue à moindre frais. Aujourd'hui, nous allons regarder de plus prêt pourquoi Petals est vraiment un Bus de Service Distribué avec un grand 'D' depuis qu'il est arrivé sur le marché des ESBs Open Source.
<h2>Blablabla et bla!</h2>
Dans l'article précédent, nous avons vu que la couche de routage était traversée par le message et que en sortie, nous obtenions toutes les informations nécessaires sur les endpoints et sur le contexte du message. Comment maintenant envoyer ce message au bon endpoint? Rien de pus simple, en effet, chaque endpoint contient les informations sur le service, son interface mais aussi sa localisation. Dans les données de localisation, le domaine, le nom du container et le nom du composant destinataire attachés au endpoint sont présentes. Rien de plus simple alors pour la couche de transport, il suffit de :
<ul>
	<li>En fonction du nom de container, il est possible de retrouver les informations sur ce container en invoquant le service de topologie. Ce service de topologie est capable de retourner la liste des informations nécessaires pour contacter une instance distante.</li>
	<li>Créer un client vers le container distant en fonction du protocole de transport</li>
	<li>Envoyer le message et attendre une réponse si besoin. Si une réponse est retournée, celle ci est encapsulée dans le message d'origine et le message repart en direction de la couche de routage. La couche de routage peut donc effectuer des opérations sur la réponse en fonction de modules de routage présents. Le message est finalement adressé au consommateur de service.</li>
</ul>
<h2>Et alors?</h2>
Et alors, contrairement aux autres ESB JBI du marché, il n'est pas nécéssaire d'utiliser des Binding Components et de les configurer pour invoquer des services distants. Un exemple de ce que cela implique est donné dans <a href="http://www.ibm.com/developerworks/java/library/j-hsb1/index.html?ca=drs-#fig7">cet article sur DeveloperWorks</a>. Avec Petals, on a ce que l'on appelle une vision unifiée du bus de service qui est distribué.
<h2>C'est tout?</h2>
Techniquement, Il y a eu beaucoup de tentatives pour implémenter un transporteur de messages digne de ce nom. Dans Petals V1, on avait un transport JMS basé sur OW2-Joram. Dans Petals V2, c'était un projet OW2 mort appelé Dream qui servait de couche de transport. Aujourd'hui dans la v3, le transport est assuré par une implémentation maison basée sur NIO très performante.
La performance c'est bien mais moi je préfère l'extensibilité. C'est pourquoi je proposerais bientôt une alternative à la couche de transport actuelle. Cette couche est très bien quand on se place au niveau de l'entreprise (le 'E' de ESB). Actuellement je travaille sur des sujets intéressants ou je remplacerais le 'E' par un 'I' pour Internet et même par 'FD' pour 'Federated Distributed'. Quand on parle d'Internet, on imagine facilement que l'on doit se confronter aux Firewalls, à échanger les messages sur HTTP, en XML avec des notions de présence etc etc... La suite bientôt!
