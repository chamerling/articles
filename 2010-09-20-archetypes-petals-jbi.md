---
layout: post
title: Archetypes Petals JBI
categories:
- PEtALS
tags:
- Apache Maven
- Java
- jbi
- PEtALS
- petalsesb
- questiondujour
status: publish
type: post
published: true
meta:
  _wp_old_slug: ''
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _ideation_attached_images: '645'
  _elasticsearch_indexed_on: '2010-09-20 09:56:12'
---
Je pense avoir déjà fait un article sur les archetypes Petals JBI qui permettent de créer les structures de base des projets <a class="zem_slink" title="Apache Maven" rel="homepage" href="http://maven.apache.org">Maven</a> pour les composants JBI et leurs artefacts de configuration (les so-called SA et SU). En cherchant un archetype dans la liste (énorme) donnée par la commande '<em>mvn archetype:generate</em>', je suis tombé sur ces fameux archetypes Petals JBI :

<a href="http://chamerling.files.wordpress.com/2010/09/capture-d_ecran-2010-09-20-a-11-46-40.png"><img class="aligncenter size-full wp-image-645" title="Capture d’écran 2010-09-20 à 11.46.40" src="http://chamerling.files.wordpress.com/2010/09/capture-d_ecran-2010-09-20-a-11-46-40.png" alt="" width="480" height="30" /></a>

La question : Comment sont ils arrivés là? Maven introspecte t-il les artefacts sur quelque repository ou une ame bienveillante les a ajouté au fichier de configuration du plugin archetype? J'opte plus pour la première réponse...
