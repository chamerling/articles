---
layout: post
title: ! 'OpenNebula, Acte 1: Les présentations'
categories:
- Cloud
tags:
- cloud
- Cloud computing
- nuage
- Open source
- opennebula
- opensource
- Planet-Libre
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
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:116:"http://chamerling.files.wordpress.com/2010/12/high-level-architecture-of-cluster-its-components-and-relationship.png";s:6:"images";a:1:{s:116:"http://chamerling.files.wordpress.com/2010/12/high-level-architecture-of-cluster-its-components-and-relationship.png";a:6:{s:8:"file_url";s:116:"http://chamerling.files.wordpress.com/2010/12/high-level-architecture-of-cluster-its-components-and-relationship.png";s:5:"width";s:3:"453";s:6:"height";s:3:"332";s:4:"type";s:5:"image";s:4:"area";s:6:"150396";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"2";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-03-14
    11:24:38";}
  _oembed_795ad0d990738146a0a7df462726bd18: ! '{{unknown}}'
  _oembed_04fc0e17f212136ad773763639f3dd61: ! '{{unknown}}'
  _oembed_3fa37cf5696df5b187bf9a640e51daff: ! '{{unknown}}'
  _oembed_af322bb0167c6f71e080940a45072a95: ! '{{unknown}}'
  _oembed_697fa9957281358955342842f75cf905: <blockquote class="twitter-tweet" width="500"><p>"c
    bien comme truc mais il faut etre bac +15 pour installer une machine" C'est pas
    de moi... @<a href="https://twitter.com/opennebula">opennebula</a></p>&mdash;
    Christophe Hamerling (@chamerling) <a href="https://twitter.com/chamerling/status/15435938196885504"
    data-datetime="2010-12-16T15:59:49+00:00">December 16, 2010</a></blockquote><script
    src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
  _ideation_attached_images: '914'
  _elasticsearch_indexed_on: '2010-12-17 05:15:47'
---
Je m'intéresse en ce moment à <a href="http://opennebula.org" target="_blank">OpenNebula</a> qui se définit comme "<em>un gestionnaire de virtualisation de ressources pour le Cloud</em>". Cet intérêt est porté par mes travaux sur le Cloud et la SOA, ou comment fournir une solution SOA utilisant le Cloud. Pour le moment je laisse le sujet SOA de coté, je l'ai déjà évoqué et je reviendrais dessus en 2011...
<blockquote>Fully open source (not open core), thoroughly tested, customizable, extensible and with unique features and excellent performance and scalability to manage hundreds of thousands of VMs:
<ul>
	<li>Private cloud with <a class="zem_slink" title="Xen" rel="homepage" href="http://www.xen.org/">Xen</a>, <a href="http://www.linux-kvm.org/page/Main_Page" target="_blank">KVM</a> and <a class="zem_slink" title="VMware" rel="homepage" href="http://www.vmware.com/">VMware</a>,</li>
	<li>Hybrid cloud (cloudbursting) with <a class="zem_slink" title="Amazon EC2" rel="homepage" href="http://aws.amazon.com/ec2/">Amazon EC2</a>, and other providers through Deltacloud (from ecosystem),</li>
	<li>Public cloud supporting EC2 Query, OGF OCCI and vCloud (from ecosystem) APIs,…</li>
	<li>and much more.</li>
</ul>
</blockquote>
Dit comme ça, on ne voit pas forcement la différence avec le concurrent direct qu'est <a class="zem_slink" title="OpenStack" rel="homepage" href="http://openstack.org/">OpenStack</a>. Deux frameworks pour le Cloud qui se positionnent de la même façon... D'ailleurs, un représentant de chaque produit était présent à la conférence OW2 le mois dernier et les présentations était assez identiques. Bon OK, OpenStack se la joue plus à l'américaine : "Eh les gars, nous on est issue de la NASA OK?!"......
OpenNebula lui est un projet qui est en partie financé par des projets de recherche Européens depuis 2005, quand on connaît le niveau de ce genre de projets financés, on a peu de doutes sur les capacités d'un produit issu de là. On a beau pas être américains, on sait aussi faire décoller des fusées... Bref, fermons la parenthèse anti-amerlocs.

2005? On parlait déjà Cloud à cette époque? Non, on parlait grille et calcul haute performance. C'est pour adresser ces domaines que OpenNebula a été créé. Elle a évoluée au cours des années car finalement c'est toujours un peu la même chose... On change le nom et on rajoute deux/trois trucs par ci par là... Ce n'est donc pas étonnant de voir une infrastructure de type cluster du type Frontale + Noeuds:

<a href="http://chamerling.files.wordpress.com/2010/12/high-level-architecture-of-cluster-its-components-and-relationship.png"><img class="size-full wp-image-914 aligncenter" title="high level architecture of cluster, its  components and relationship" src="http://chamerling.files.wordpress.com/2010/12/high-level-architecture-of-cluster-its-components-and-relationship.png" alt="" width="453" height="332" /></a>

(Par la suite, ONE = OpenNebula; VM = Machine Virtuelle)
<ul>
	<li>Le binaire de ONE est installé et lancé en tant que daemon sur la machine frontale (<em>ONED</em>). C'est lui qui fournit les APIs Cloud propriétaire et celles qui peuvent être pluggées par dessus (du type Amazon EC2, OGF, etc...).</li>
	<li>Dans le comportement par défaut, les images permettant de lancer les machines virtuelles sont partagées entre les noeuds via un bon vieux système de fichiers réseau de type NFS.</li>
	<li>La frontale doit pouvoir se connecter à tout les noeuds en <em>SSH</em> ie partage de clés.</li>
	<li>Les Hypervisors sont les plateformes de virtualisation qui permettent de lancer des machines virtuelles sur les noeuds. Les plus connus (et supportés par défaut par ONE) sont, par exemple, Xen, KVM et VMWare.</li>
	<li>Les <em>Drivers</em> permettent de créer la communication entre le daemon tournant sur la frontale et les hyperviseurs.</li>
</ul>
D'après cette liste, on comprend mieux la philosophie d'OpenNebula. Il s'agit d'utiliser des hyperviseurs existants, de les envelopper, de leur rajouter des fonctionnalités et de les gérer intelligemment. Cette gestion intelligente doit bien sur se caler par rapport aux attentes de niveau infrastructure Cloud (IaaS) en offrant de l'élasticité, de la migration de VMs entre noeuds, etc...

Une fois les hyperviseurs installés et en état de marche (c'est vraiment la chose la plus dure à faire et celle sur laquelle j'ai perdu un temps fou à cause d'une machine trop vieille...), l'utilisation de base depuis la frontale est assez simple:
<ol>
	<li>Déclarer les noeuds physiques ie ceux qui hébergent les hyperviseurs et qui feront tourner les VMs</li>
	<li>Déclarer un ou plusieurs réseaux virtuels. On définit ici principalement les addresses IPs qui seront assignées et utilisées par les VMs.</li>
	<li>Soumettre une image système et son descripteur au scheduler. Le scheduler a pour but de faire migrer l'image vers le bon noeud physique afin de pouvoir démarrer une VM sur ce noeud.</li>
	<li>Pas de 4... Enfin si mais pour des utilisations poussées, je passe pour le moment...</li>
</ol>
Dans une utilisation simple en mode Cloud privé, c'est à peu pres tout. On peut démarrer, arrêter et migrer des VMs assez facilement. ONE se charge d'assigner les bonnes addresses IPs pour peu que l'image système soit configurée pour le faire (via quelques scripts système). On peut donc créer son Cloud privé de type IaaS avec ONE pour pas un euro (modulo le temps à appréhender le monstre) et avoir un total controle dessus.
<p style="text-align:center;"><a href="http://www.flickr.com/photos/hamerling/1117449107/in/photostream/"><img class="aligncenter" title="Mer de Nuages" src="http://farm2.static.flickr.com/1176/1117449107_85f99c7cb3_z_d.jpg" alt="" width="512" height="384" /></a></p>
<em>Ca à l'air très simple... Oui l'utilisation est simple, c'est la mise en place qui l'est moins. Tout du moins pour préparer les noeuds et les hyperviseurs. Je suis tombé sur des soucis avec la mise en place de KVM sur une machine trop vieille et qui ne fournit pas la virtualisation niveau processeur. Je passe car je n'ai pas le courage mais j'ai perdu pas mal de temps. Les développeurs qui sont très réactifs ont passé du temps avec moi pour essayer de voir ce qui collait pas. Résultat, je laisse tomber KVM que je commençais à maîtriser pour passer à Xen qui est moins simple mais qui est censé marcher mieux. Encore quelques dizaines de cafés en vue...</em>

http://twitter.com/chamerling/status/15435938196885504
<p style="text-align:center;">Dédicace à mon sysadmin préféré ;)</p>
