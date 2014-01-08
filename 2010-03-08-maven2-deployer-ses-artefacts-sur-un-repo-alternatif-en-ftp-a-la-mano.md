---
layout: post
title: ! 'Maven2 : Deployer ses artefacts sur un repo alternatif en FTP ''a la mano'''
categories:
- Java
tags:
- altDeploymentRepository
- deploy
- ftp
- maven
- tip
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2010-03-08 10:24:00'
---
Le besoin du jour : "J'ai des artefacts packagés avec Maven2 et je dois les partager avec des partenaires sans passer par un déploiement standard de mon projet mais en passant par un serveur FTP". En plus, en terme de contrainte majeure, je ne veux absolument pas modifier mes bon vieux POMs...

Bon, comme je le dis souvent "Google est ton ami", mais aujourd'hui Google ne l'est pas (comme le temps dehors, de la neige en Mars a Montpellier, mais ou va-t-on?). Bref, faisons le bêtement :

[sourcecode language="bash"]

mvn deploy -DaltDeploymentRepository=chamerling.maven.snapshot::default::ftp://ftpperso.free.fr/maven/repository/snapshot

[/sourcecode]

Je viens de dire a Maven2 de déployer mon artefact sur un repository dont l'ID est chamerling.maven.snapshot (pour rappel, cela veut dire que Maven2 va aller chercher dans mon settings.xml un serveur dont l'ID est chamerling.maven.snapshot pour récupérer le login et le mot de passe) en FTP sur l'URL ftp://ftpperso.free.fr/maven/repository/snapshot. Et c'est la que mon autre ami Maven2 (bon OK je n'ai que des amis virtuels...) me dit :

[sourcecode language="text"]

[INFO] Error retrieving previous build number for artifact 'foo:bar:pom': repository metadata for: 'snapshot foo:bar:1-SNAPSHOT' could not be retrieved from repository: chamerling.maven.snapshot due to an error: Unsupported Protocol: 'ftp': Cannot find wagon which supports the requested protocol: ftp
Component descriptor cannot be found in the component repository: org.apache.maven.wagon.Wagonftp.

[/sourcecode]

Ok bon c'est normal... Wagon est la librairie de transport abstraite utilisée par Maven2 et par défaut FTP n'est pas supporté. D'ailleurs sur le site du plugin deploy de Maven2 on remarque bien qu'il faut normalement définir dans le POM que l'on veut utiliser l'extension FTP de Wagon :

[sourcecode language="xml"]

&lt;build&gt;
&lt;extensions&gt;
&lt;!-- Enabling the use of FTP --&gt;
&lt;extension&gt;
&lt;groupId&gt;org.apache.maven.wagon&lt;/groupId&gt;
&lt;artifactId&gt;wagon-ftp&lt;/artifactId&gt;
&lt;version&gt;1.0-beta-6&lt;/version&gt;
&lt;/extension&gt;
&lt;/extensions&gt;
&lt;/build&gt;

[/sourcecode]

Oui mais moi je ne veux pas modifier mon POM! L'astuce c'est de rajouter l'extension FTP-Wagon dans le classpath de Maven2 ie ajouter la librairie Wagon FTP dans M2_HOME/lib et la 'Oh Miracle', ca ne marche pas...

[sourcecode language="text"]

[INFO] ------------------------------------------------------------------------
[ERROR] BUILD ERROR
[INFO] ------------------------------------------------------------------------

[INFO] Error retrieving previous build number for artifact 'foo:bar:pom': repository metadata for: 'snapshot foo:bar:1-SNAPSHOT' could not be retrieved from repository: chamerling.maven.snapshot due to an error: Unsupported Protocol: 'ftp': Cannot find wagon which supports the requested protocol: ftp
org.apache.commons.net.ProtocolCommandListener
[INFO] ------------------------------------------------------------------------

[/sourcecode]

Normal, Wagon FTP utilise commons-net pour tout ce qui est communication, j'ajoute donc le JAR de commons-net  2.0 dans mon M2_HOME/lib et là ça marche beaucoup mieux :

[sourcecode language="text"]

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ------------------------------------------------------------------------

[/sourcecode]
