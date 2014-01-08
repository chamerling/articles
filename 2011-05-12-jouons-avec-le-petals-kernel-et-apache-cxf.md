---
layout: post
title: Jouons avec le Petals Kernel et Apache CXF
categories:
- PEtALS
tags:
- Apache CXF
- Application programming interface
- conduit
- cxf
- destination
- dsb
- factory
- Java
- jbi
- Petals ESB
- petalslink
- Service Bus
- transport
- Web service
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_mess: ! '#ow2 #petals #soa #cxf Jouons avec le Petals Kernel et Apache CXF:'
  _wpas_done_twitter: '1'
  _oembed_28826cc6623f105c8d209c334229532c: ! '{{unknown}}'
  _oembed_69b4a26ccf5744cfb2348bc6ae25ce05: ! '{{unknown}}'
  _oembed_48d454f232b7d505bd1c4a83f26b1884: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2011-05-11 22:08:49'
---
Oui, à chacun sa façon de s'amuser... Je viens d'ajouter une feature dans le <a href="http://research.petalslink.org/display/petalsdsb/PetalsDSB+Overview" target="_blank">Petals Distributed Service Bus</a> qui me trottait dans la tête depuis un petit moment: Pouvoir invoquer des services techniques (distants ou pas) du coeur du DSB en utilisant la couche de transport naturellement utilisée pour invoquer des services métiers (les services hostés par le bus ou liés à celui ci). Autrement dit, imaginons que j'ai un service technique qui tourne sur un noeud et que depuis un autre noeud, je dispose d'un client pour l'invoquer de façon simple mais aussi totalement transparente.

Ce que l'on pouvait faire jusqu'à présent, c'est utiliser l'API Web service exposée par le DSB pour invoquer le service pour peux que le service soit annoté comme il faut (pour rappel, cette API Web service utilise <a class="zem_slink" title="Apache CXF" href="http://cxf.apache.org/" rel="homepage">Apache CXF</a> pour exposer automatiquement les composants Fractal qui sont annotés avec JAXWS). Bien, mais par défaut on utilise deux ports pour les communications inter noeuds: Le port utilisé par Apache CXF, et l'autre par la couche de transport du DSB (en fait c'est le cas dans <a class="zem_slink" title="Petals ESB" href="http://petals.ow2.org" rel="homepage">Petals ESB</a>, mais dans le DSB, la couche de transport étant par défaut une implémentation en Web service, on utilise le même port pour les deux). En allant un peu plus dans les détails, même si on utilise le même port pour les deux modules dans le DSB, ceci n'est plus vrai si une nouvelle implémentation de la couche de transport utilise autre chose que CXF (pour info, dans le DSB, une implémentation de la couche de transport en XMPP existe aussi). C'est donc la qu'on arrive a la solution d'utiliser le canal de communication du DSB pour toute invocation entre noeuds, comme on sait déjà échanger des messages avec le DSB, pourquoi s'en priver?

C'est bien beau de vouloir exposer des services, des composants Fractal, ou quoi que ce soit, si on veut rester en Java et ne pas avoir besoin de se triturer le peu de cerveau qu'il nous reste, autant penser à utiliser tout ou partie d'une solution qui existe deja. En allant dans ce sens, comme on doit au final avoir une sorte de serialization des messages, autant utiliser quelque chose qui sait deja le faire, j'ai nommé le couple inséparable JAXB+JAXWS. Ceci impliquera seulement d'annoter ses interfaces et de créer des types propres et JAXB-aware mais au final, je ne pense pas que cela soit une contrainte forte... C'est bien beau tout cela mais JAXB + JAXWS tout seul ca ne fait pas grand chose. C'est la qu'entre en jeu Apache CXF. Puisque l'utilisation de JAXWS est simple avec CXF, autant s'intéresser un peu à la bete. Pour le coup cà tombe bien, dans CXF il y a deja une notion de transport multiple: HTTP, XMPP, JMS et si on regarde bien on a même JBI (les potes de chez JonAS ont d'ailleurs implémenté un transport CXF utilisant les EJB ou un truc J2EE dans le style si ma mémoire est bonne). Tiens donc, il y a un transport JBI? Ca tombe (presque) bien, le DSB est basé sur Petals ESB qui est JBI compliant. Oui mais en fait non. J'aurais presque pu l'utiliser mais le but est d'implémenter quelque chose d'assez 'JBI independant'. Le tout est maintenant d'implémenter un nouveau transport pour le DSB et grace au code source de CXF et à cette page <a href="http://cxf.apache.org/custom-cxf-transport.html" target="_blank">http://cxf.apache.org/custom-cxf-transport.html</a> on comprend assez bien comment faire. Dans la terminologie CXF, le client est le 'conduit' et le serveur est la 'destination'. Il suffit donc d'implémenter nos propres conduit et destination pour pouvoir échanger des messages avec CXF.

Une fois CXF configuré avec nos conduit et destination, ou plutôt une fois la bonne transport factory implémentée, on devrait pouvoir faire quelque chose du style:

[sourcecode language="java"]
// 1. Initialize CXF Bus with factory
Bus bus = BusFactory.getDefaultBus();
DestinationFactoryManager dfm = bus.getExtension(DestinationFactoryManager.class);
DSBTransportFactory petalsTransportFactory = new DSBTransportFactory();
petalsTransportFactory.setBus(bus);
petalsTransportFactory.registerWithBindingManager();

// 2. Start service
System.out.println(&quot;Starting CXF server...&quot;);
JaxWsServerFactoryBean sf = new JaxWsServerFactoryBean();
sf.setAddress(&quot;dsb://host/serviceA&quot;);
sf.setServiceClass(HelloService.class);
sf.setServiceBean(new HelloService() {
    public String sayHello(String input) throws PEtALSWebServiceException {
        System.out.println(&quot;SAY HELLO INVOKED!&quot;);
        return pre+input;
    }
});
sf.create();

// 3. Create the client and invoke
JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
factory.setAddress(&quot;dsb://host/serviceA&quot;);
factory.setServiceClass(HelloService.class);
HelloService hello = (HelloService) factory.create();
String out = null;
try {
    out = hello.sayHello(in);
} catch (PEtALSWebServiceException e) {
}
[/sourcecode]

Je passe les détails d'implémentation (que j'ai eu le plaisir de faire dans un bon vieux TGV en panne entre Montpellier et Bruxelles) pour le moment, mais ce qui se passe se résume à:
<ol>
	<li>On configure CXF en lui ajoutant notre Transport Factory. Au niveau de l'implémentation, il y a ce qu'il faut pour que lorsque une client ou une server factory est configurée avec une adresse qui commence par 'dsb://', ce soit notre DSBFactory qui soit automatiquement appelée par CXF.</li>
	<li>La création du service se fait comme si on créait un Web service sur http sauf que là c'est un 'Web service sur DSB'.</li>
	<li>Idem que pour 2 mais on génère un client qui nous permettra d'appeler le service créé auparavant.</li>
</ol>
En résultat, pour peu que le client (en 3) et le serveur (en 2) soient exécutés sur des noeuds séparés, le DSB se chargera d'acheminer la requête et la réponse comme il faut entre les acteurs et ceci de façon complètement transparente. Au niveau de l'implémentation coté kernel du DSB (ceci vaut aussi pour Petals ESB), il suffit de connaître quand même un peu le coeur pour créer des fakes de composants JBI en farfouillant dans les APIs disponibles, de savoir jouer avec le DeliveryChannel, le ComponentContext, le NormalizedMessageRouter, ajouter quelques modules de routage, savoir recevoir des messages et y répondre, et surtout implémenter le Conduit et la Destination CXF en s'inspirant de ce qui existe déjà et en mettant les doigts dans le cambouis. Pour les curieux, je vous conseille de jeter un oeil au code de CXF assez compréhensible et pas mal fait du tout.

Enfin pour étendre cela aux services techniques du coeur du DSB, il suffit, au runtime, de scanner les services disponibles offerts par les composants du kernel et automatiquement créer les services comme décrit dans l'étape 2. Chose que l'on sait déjà faire dans le DSB via le scan pour exposer les services en Web service sur HTTP avec CXF (oui toujours lui, avant je m'en prenais a Axis2...).

Et voila donc une belle et simple façon de simplifier la vie du développeur du DSB (c'est à dire moi). Il va être maintenant possible d'invoquer n'importe quoi de n'importe où avec une API Java. Pour peu que l'on plugge quelques modules au bon endroit, on peut imaginer faire des choses assez sympa: Le Cloud en fait partie.
<blockquote>Note: Le code sera disponible d'ici la fin de la semaine sur le SVN du DSB. Surement sous https://svn.petalslink.com/svnroot/trunk/research/commons/dsb/modules/dsb-kernel, je mettrais a jour l'article en fonction...</blockquote>
