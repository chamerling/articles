---
layout: post
title: Communication de noeuds Petals via XMPP
categories:
- PEtALS
tags:
- distributed
- esb
- Google
- gtalk
- ow2
- PEtALS
- SOA
- SOA4All
- xmpp
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _wpas_done_yup: '1'
  tagazine-media: a:7:{s:7:"primary";s:87:"http://chamerling.files.wordpress.com/2010/05/capture-d_ecran-2010-05-27-a-10-54-42.png";s:6:"images";a:2:{s:61:"http://chamerling.files.wordpress.com/2010/05/xmpp-petals.png";a:6:{s:8:"file_url";s:61:"http://chamerling.files.wordpress.com/2010/05/xmpp-petals.png";s:5:"width";s:3:"543";s:6:"height";s:3:"445";s:4:"type";s:5:"image";s:4:"area";s:6:"241635";s:9:"file_path";s:0:"";}s:87:"http://chamerling.files.wordpress.com/2010/05/capture-d_ecran-2010-05-27-a-10-54-42.png";a:6:{s:8:"file_url";s:87:"http://chamerling.files.wordpress.com/2010/05/capture-d_ecran-2010-05-27-a-10-54-42.png";s:5:"width";s:3:"976";s:6:"height";s:3:"589";s:4:"type";s:5:"image";s:4:"area";s:6:"574864";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"2";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-05-27
    10:13:23";}
  _ideation_attached_images: '511'
  _elasticsearch_indexed_on: '2010-05-27 10:13:23'
---
Dans le cadre de mes travaux de recherche sur la fédération de Bus de Service (et en particulier de Petals ESB dans un premier temps), je suis en train d'étudier la fédération via Jabber/XMPP. L'approche la plus rapide sans avoir l'utilité de monter un serveur Jabber est d'utiliser GoogleTalk comme support et ainsi profiter aussi de l'infrastructure de Google et de sa robustesse.

Les premiers tests n'ont rien a voir avec la fédération mais valident que l'on peut utiliser une couche de transport générique que j'ai développé (et que je détaillerais dans un futur article) pour rapidement implémenter un transport utilisant le protocole désiré, ici XMPP. Le schéma suivant donne une vision de haut niveau de cette architecture et du cas d'usage :

<a style="text-decoration:none;" href="http://chamerling.files.wordpress.com/2010/05/xmpp-petals.png"><img class="aligncenter size-full wp-image-511" title="xmpp-petals" src="http://chamerling.files.wordpress.com/2010/05/xmpp-petals.png" alt="" width="543" height="445" /></a>

Description du scénario ci dessus:
<ul>
	<li>Chaque noeud est connecté à GTalk avec un compte dédié</li>
	<li>Un service est lié à Petals ESB, disons que c'est un Web service bindé avec le composant SOAP (Proxy Out)</li>
	<li>L'endpoint ainsi présent dans Petals ESB est exposé sous forme de Web service par le composant SOAP sur l'autre container (Proxy In)</li>
	<li>Tout client envoyant une requête au service via le Proxy In voit sa requête traverser les différentes couches de Petals jusqu'à arriver à la couche de transport. La communication avec le container cible se fait grâce à la notion de Chat de Jabber. Un Chat est créé entre le container 1 et le container 2 et le message contenant la requête est alors publié au container 2 via le Chat.</li>
	<li>Le container 2 reçoit le message et le fait remonter jusqu'au service final. La réponse prends le chemin inverse.</li>
</ul>
On profite aussi de l'archivage des communications Gtalk :

<a href="http://chamerling.files.wordpress.com/2010/05/capture-d_ecran-2010-05-27-a-10-54-42.png"><img class="aligncenter size-full wp-image-509" title="Archivage" src="http://chamerling.files.wordpress.com/2010/05/capture-d_ecran-2010-05-27-a-10-54-42.png" alt="" width="595" height="359" /></a>

Evidemment, passer par GTalk pour invoquer des services d'un même domaine est pénalisant et l'intérêt est assez limitée car la communication entre les noeuds ne se fait pas uniquement via la couche de transport (la recherche de endpoints passe par un canal différent par exemple). Et c'est la que la fédération du Bus va entrer en jeu. La suite donc au prochain numéro.
