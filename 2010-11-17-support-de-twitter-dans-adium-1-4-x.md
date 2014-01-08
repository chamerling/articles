---
layout: post
title: Support de Twitter dans Adium 1.4.x
categories:
- Mac OS X
- SOCIAL
tags:
- Adium
- Instant messaging
- Internet Relay Chat
- Open source
- Planet-Libre
- twitter
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  tagazine-media: a:7:{s:7:"primary";s:82:"http://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Adium.png/300px-Adium.png";s:6:"images";a:1:{s:82:"http://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Adium.png/300px-Adium.png";a:6:{s:8:"file_url";s:82:"http://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Adium.png/300px-Adium.png";s:5:"width";s:3:"300";s:6:"height";s:3:"300";s:4:"type";s:5:"image";s:4:"area";s:5:"90000";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-11-17
    14:00:00";}
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2010-11-17 16:59:26'
---
<div class="zemanta-img">

[caption id="" align="alignright" width="300" caption="Image via Wikipedia"]<a href="http://commons.wikipedia.org/wiki/File:Adium.png"><img title="Adium icon, also known as the Evil Menu Duck (EMD)" src="http://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Adium.png/300px-Adium.png" alt="Adium icon, also known as the Evil Menu Duck (EMD)" width="300" height="300" /></a>[/caption]

</div>
J'utilise déjà <a class="zem_slink" title="Adium" rel="homepage" href="http://www.adium.im">Adium</a> pour Gtalk, jabber perso et pro et en temps que <a class="zem_slink" title="Twitter" rel="homepage" href="http://twitter.com">Twitter</a>-addict je me suis dit qu'il était temps de voir comment est supporté ce service dans la nouvelle version de Adium sortie il y a quelques jours (le support dans la beta était un peu juste et ne m'avait pas spécialement satisfait...). Voici donc un premier retour d'expérience sur l'utilisation de Adium et de Twitter.
<h1><span style="font-size:20px;">Les mentions</span></h1>
<h3>Mentionner</h3>
Le premier soucis de l'intégration de Twitter dans un logiciel initialement conçu pour la messagerie instantanée est le mappage entre le monde du microblogging et de l'IM. Les développeurs d'Adium ont décidé que chaque abonnement Twitter serait mappé sur un buddy IM. Ok bon, je suis actuellement 538 personnes (mais activement quelques dizaines, merci les listes!). Bref, cela me donne une fenêtre de liste de contacts assez énorme, qui de plus est affreusement longue à charger au démarrage (l'enrichissement se fait pas tranche d'une centaine de contacts, je pense que les limitations de l'API Twitter doit en prendre un coup). En plus je me retrouve avec a peu prêt autant de notifications <a class="zem_slink" title="Growl (software)" rel="homepage" href="http://growl.info/">Growl</a> me donnant le statut actuel de mes contacts...
Du coup, pas d'autre choix que de désactiver l'option '<em>Afficher qui je suis dans la liste de contact</em>' dans les préférences du compte Twitter.

Oui mais... En désactivant cette option, il se trouve que la completion du nom lors d'un @mention ne marche pas pour tous les utilisateurs... Pour info, la completion est disponible en tapant '@xxx + Tab' xxx étant le début du nom de la personne à mentionner.
Les développeurs ont apparemment choisit de ne pas charger la liste des personnes suivies si on ne veut pas les afficher. Dommage, l'affichage n'étant pas la seule utilisation que l'on puisse faire et il arrive souvent que l'on veuille mentionner quelqu'un dans un tweet... Après avoir discuté avec les développeurs sur le canal IRC d'adium (#adium sur freenode), la seule solution est de réactiver l'option que l'on vient de désactiver un peu plus haut et de choisir le masquage des contacts pour le compte depuis '<em>Affichage&gt;Masquer certains contacts&gt;Masquer les contacts du compte</em>'.

Bref, on se retrouve avec quelque chose qui marche mais qui dans mon cas prends énormément de temps à démarrer...
<h3>Être mentionné</h3>
Dommage, il semble que la seule façon de voir que l'on est mentionné dans le tweet d'un contact est une minuscule marque dans la barre de défilement... Une notification Growl aurait bien fait l'affaire (ou même une fenêtre de dialogue spéciale).
<h2>Les DM</h2>
Puisque au point précédent j'ai du désactiver l'affichage de la liste des contacts (et que je ne peux plus 'double-cliquer' sur le contact), comment alors envoyer un message direct?
Seule solution, créer une nouvelle conversation (cmd-N). Ouf la completion marche bien si on a bien activé l'option qui va bien (cf point précédent)...

Par contre, cette fois ci un DM venant de quelqu'un affiche bien une nouvelle fenêtre de dialogue de type chat.
<h2>Les listes</h2>
Support absent... Dommage, je ne suis activement qu'une vingtaine de personnes que j'ai placé dans une liste privée (la timeline globale étant vraiment trop horrible à suivre). Il y a sûrement ici une astuce pour créer un groupe d'utilisateur mais je n'ai pas envie de dupliquer les choses. Je m'en passerais pour le moment.
<h1>--</h1>
<h1><span style="font-weight:normal;font-size:13px;">Pour conclure cet article, il est clair que le 'mapping' entre deux mondes pas tout à fait identiques est assez complexe mais pour une première intégration c'est déjà vraiment pas mal (en plus je ne pense pas qu'il y ait une société derrière Adium comme on peut le voir pour pas mal de projets Open Source de nos jours ie quid du business model d'un logiciel de messagerie instantanée). Ce genre d'exercice peut évidement tirer aussi les fonctionnalités de base vers le haut, a voir dans les prochaines versions.</span></h1>
Les points cités précédemment sont certainement contournables, et discuter avec les développeurs via le canal IRC du projet est un bon moyen de trouver des solutions alternatives en attendant la version 1.5 (le tracker est déjà bien rempli)...
