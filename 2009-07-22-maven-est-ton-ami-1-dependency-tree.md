---
layout: post
title: ! 'Maven est ton ami #1 : Dependency tree'
categories:
- Java
tags:
- maven
- tip
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-07-22 12:58:28'
---
Someones are saying that Maven is hard to domesticate... That's true but when you you become Maven aware it really facilitate the developer's life. More posts will come later on Maven... For now just an answer to guys who are always asking me (and others):
<blockquote>"Hey! Which module include this dependency? I need to update the version but I can not find where it is defined?!"</blockquote>
<em>(Dédicace à Nico Jr ;) )</em>
<blockquote>mvn dependency:tree</blockquote>
This plugin will give you the complete dependency tree of the current module. Thanks Maven!
