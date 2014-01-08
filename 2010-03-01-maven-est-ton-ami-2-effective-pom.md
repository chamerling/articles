---
layout: post
title: ! 'Maven est ton ami #2 : Effective POM'
categories:
- Java
tags:
- maven
- tip
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2010-03-01 10:34:51'
---
Après "<a href="http://chamerling.wordpress.com/2009/07/22/maven-est-ton-ami-1-dependency-tree/">Maven est ton ami #1 : Dependency tree</a>", le court article du jour est juste une note pour les personnes qui se (et me) demandent souvent pourquoi ils n'arrivent pas a releaser (entre autres)...
Ce qui est bien avec Maven2, c'est que l'on peut 'overrider', utiliser des parents, définir ses mots de passe sans les publier publiquement... Mais ce qui est moins bien, quand on ne maitrise pas Maven, c'est que l'on se perd vite dans toutes ses configurations (ajoutez par dessus un proxy, de l'authentification, des repositories privés...). Est c'est la que l'on se demande comment on peut s'y retrouver...

Heureusement, les gens de chez Maven2 ne sont pas si bêtes, le <a href="http://maven.apache.org/plugins/maven-help-plugin/usage.html">plugin Help</a> permet de lister le POM effectif du projet courant en lancant la commande
<blockquote><em># mvn help:effective-pom</em></blockquote>
Simple et efficace. Les gens qui se demandent, par exemple, pourquoi ils ne sont pas bien authentifiés lors d'une release peuvent alors verifier dans le POM généré, que leurs login/password ne sont pas définis pour le bon ID de serveur...
