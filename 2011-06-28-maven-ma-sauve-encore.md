---
layout: post
title: Maven m'a sauvé... encore!
categories:
- Java
tags:
- Apache Maven
- Build Management
- dependency
- Java
- petalslink
- plugin
status: publish
type: post
published: true
meta:
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_done_twitter: '1'
  geo_latitude: '43.484082'
  _elasticsearch_indexed_on: '2011-06-28 14:56:26'
---
Encore une fois <a class="zem_slink" title="Apache Maven" href="http://maven.apache.org" rel="homepage">Apache Maven</a> m'a sauvé, ou presque... Je cherchais un moyen de récupérer toutes les sources des dépendances d'un module pour un faire une archive complète. Le module en question étant une distribution contenant quelques dizaines de modules gérés par Maven, la solution est de regarder ce qu'offre l'ami Maven en question. J'utilise souvent le plugin dependency pour avoir l'arbre de dépendance via le goal tree et résoudre certains conflits de versions (oui bon m2eclipse le fait surement mais je reste fan du Terminal...), et donc il me semblait bien que le plugin dependency faisait plus que ca. C'est là qu'entre en jeu le goal <a href="http://maven.apache.org/plugins/maven-dependency-plugin/unpack-dependencies-mojo.html" target="_blank">unpack-dependencies</a>. Deux/trois ajustement dans le POM du projet et il est simple de récupérer les sources:

[source language="xml"]
&lt;project&gt;
&lt;!--...--&gt;
  &lt;plugins&gt;
  		&lt;plugin&gt;
		        &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
		        &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
		        &lt;version&gt;2.2&lt;/version&gt;
		        &lt;executions&gt;
		          &lt;execution&gt;
		            &lt;id&gt;src-dependencies&lt;/id&gt;
		            &lt;phase&gt;package&lt;/phase&gt;
		            &lt;goals&gt;
		              &lt;!-- use copy-dependencies instead if you don't want to explode the sources --&gt;
		              &lt;goal&gt;unpack-dependencies&lt;/goal&gt;
		            &lt;/goals&gt;
		            &lt;configuration&gt;
		              &lt;classifier&gt;sources&lt;/classifier&gt;
		              &lt;failOnMissingClassifierArtifact&gt;false&lt;/failOnMissingClassifierArtifact&gt;
		              &lt;outputDirectory&gt;${project.build.directory}/sources&lt;/outputDirectory&gt;
					  &lt;includeGroupIds&gt;org.ow2.petals&lt;/includeGroupIds&gt;
		            &lt;/configuration&gt;
		          &lt;/execution&gt;
		        &lt;/executions&gt;
		      &lt;/plugin&gt;
   &lt;!--...--&gt;
  &lt;/plugins&gt;
&lt;/project&gt;
[/source]
