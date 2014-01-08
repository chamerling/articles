---
layout: post
title: ! 'Tip : Conversion de durée en Java'
categories:
- Java
tags:
- astuce
- Java
- tip
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _wp_old_slug: ''
  _wpas_done_yup: '1'
  _elasticsearch_indexed_on: '2010-08-17 10:51:58'
---
Un article pas bien avancé techniquement, juste une astuce/une utilisation de l'API du JDK...
En parcourant le code de <em>java.util.concurrent.ScheduledThreadPoolExecutor</em> je suis tombé sur une utilisation de <em>java.util.concurrent.TimeUnit</em> que je ne connaissais pas et qui est fort utile pour faire des conversions de durée : la méthode #convert(long, TimeUnit).

Plus besoin de s'embêter, ni même de réfléchir, convertir n'importe quelle unité de temps vers n'importe quelle unité de temps est simple :

[sourcecode language="java"]

long days = TimeUnit.DAYS.convert(22545455566L, TimeUnit.MILLISECONDS);

[/sourcecode]

ie 22545455566 ms sont équivalentes à 260 jours.
