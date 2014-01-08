---
layout: post
title: WTF Spoonlet?
categories:
- Java
tags:
- Apache Maven
- Application programming interface
- bug
- Java
- Languages
- petalslink
- Programming
- Source code
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_done_twitter: '1'
  _wpas_skip_yup: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-06-21
    10:14:19";}
  _elasticsearch_indexed_on: '2011-06-21 10:13:19'
---
Ca fait longtemps que je n'ai pas parlé de code. Qu'a cela ne tienne, dans la série "Le bon gros bug de m****", je vous présente Spoon... Dans un élan de refactoring, je me suis mis à extraire une vrai API du kernel de Petals DSB. L'idée est simple, le faire aurait du l'être: Extraire toutes les interfaces d'un projet A pour créer un projet A-api et en dépendre dans le projet A. Trois clics de souris et deux drag&amp;drop plus loin, testons la compilation:

[sourcecode language="text"]
[INFO] ------------------------------------------------------------------------
[INFO] Building dsb-kernel
[INFO] task-segment: [install]
[INFO] ------------------------------------------------------------------------
[INFO] [resources:resources {execution: default-resources}]
[INFO] Using default encoding to copy filtered resources.
[INFO] [compiler:compile {execution: default-compile}]
[INFO] Nothing to compile - all classes are up to date
[INFO] [spoon:recompile {execution: default}]
[INFO] org.objectweb.fractal.fraclet.annotation.processor.FractalComponentProcessor
[WARNING] FractalComponentProcessor &amp;gt;&amp;gt; No value found for property 'generatorClass' in processor org.objectweb.fractal.fraclet.annotation.processor.FractalComponentProcessor
[INFO] ------------------------------------------------------------------------
[ERROR] BUILD ERROR
[INFO] ------------------------------------------------------------------------
[INFO] fail to execute&lt;/code&gt;

[INFO] ------------------------------------------------------------------------
[INFO] For more information, run Maven with the -e switch
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 23 seconds
[INFO] Finished at: Mon Jun 20 23:27:21 CEST 2011
[INFO] Final Memory: 50M/123M
[INFO] ------------------------------------------------------------------------
[/sourcecode]

Ahah, qu'a cela ne tienne, le même en mode debug donc...

[sourcecode language="text"]
[DEBUG] Trace
org.apache.maven.lifecycle.LifecycleExecutionException: fail to execute
at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoals(DefaultLifecycleExecutor.java:719)
at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoalWithLifecycle(DefaultLifecycleExecutor.java:556)
at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoal(DefaultLifecycleExecutor.java:535)
at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoalAndHandleFailures(DefaultLifecycleExecutor.java:387)
at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeTaskSegments(DefaultLifecycleExecutor.java:348)
at org.apache.maven.lifecycle.DefaultLifecycleExecutor.execute(DefaultLifecycleExecutor.java:180)
at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:328)
at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:138)
at org.apache.maven.cli.MavenCli.main(MavenCli.java:362)
at org.apache.maven.cli.compat.CompatibleMain.main(CompatibleMain.java:60)
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
at java.lang.reflect.Method.invoke(Method.java:597)
at org.codehaus.classworlds.Launcher.launchEnhanced(Launcher.java:315)
at org.codehaus.classworlds.Launcher.launch(Launcher.java:255)
at org.codehaus.classworlds.Launcher.mainWithExitCode(Launcher.java:430)
at org.codehaus.classworlds.Launcher.main(Launcher.java:375)
Caused by: org.apache.maven.plugin.MojoExecutionException: fail to execute
at net.sf.alchim.spoon.contrib.maven.AbstractSpoonMojo.execute(AbstractSpoonMojo.java:96)
at org.apache.maven.plugin.DefaultPluginManager.executeMojo(DefaultPluginManager.java:490)
at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoals(DefaultLifecycleExecutor.java:694)
... 17 more
Caused by: java.lang.NullPointerException
at spoon.support.reflect.declaration.CtAnnotationImpl.convertValue(CtAnnotationImpl.java:160)
at spoon.support.reflect.declaration.CtAnnotationImpl.getElementValue(CtAnnotationImpl.java:253)
at spoon.support.reflect.declaration.CtAnnotationImpl$AnnotationInvocationHandler.invoke(CtAnnotationImpl.java:69)
at $Proxy31.name(Unknown Source)
[/sourcecode]

Yeah cool, une NPE. Ah dommage, sur $Proxy31.name... Genre le proxy généré par Spoon. Et donc bon, on fait quoi la? On revert tout et on tente de bouger les interfaces une par une pour voir qui fout son bordel? Mouais... Trève de blahblah, après avoir passé quelques dizaine de minutes à bouger, compiler, reverter, voici la conclusion: <strong>Définir des constantes dans des interfaces qui sont dans des dépendances et les utiliser dans les annotations des implémentations n'est pas possible</strong>, par exemple, dans une implémentation du style :

[sourcecode language="java"]
@FractalComponent
@Provides(interfaces = { @Interface(name = &quot;service&quot;, signature = ServiceBinderRegistry.class) })
public class ServiceBinderRegistryImpl implements ServiceBinderRegistry {

@Requires(name = BINDER_PREFIX, signature = ServiceBinder.class, cardinality = Cardinality.COLLECTION, contingency = Contingency.OPTIONAL)
private final Map&lt;String, Object&gt; binders = new Hashtable&lt;String, Object&gt;();
[/sourcecode]

On ne peut pas avoir BINDER_PREFIX dans l'interface ServiceBinderRegistry qui est dans une dépendance, par contre ca marche bien quand l'interface est dans le même projet... Quand on tombe la dessus à minuit, c'est rude...
