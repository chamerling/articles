---
layout: post
title: From Linux to Mac OS X – Tip 1 : The Aliases
categories:
- Mac OS X
tags:
- mac osx
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2009-03-11 17:40:27'
---
Here are a serie of tips when developing under Mac OS X after mostly 8 years in the Linux word.

First tip : The aliases in the Terminal (because I really think that the Terminal is cool :o) )

<strong>1. Create a .profile file in your $HOME folder</strong>
<em>cd $HOME
touch .profile</em>

<strong>2. Add aliases in this .profile file</strong>
<em>pico .profile</em>
and then syntax is alias foo='bar'. For example, here are some of my aliases :
<em>alias tx='open /Applications/TextEdit.app/'
alias mvn2='/Users/chamerling/Developpement/bin/mvn/apache-maven-2.0.10/bin/mvn'
alias meclipse='mvn2 eclipse:eclipse'
alias mpetals='mvn2 archetype:generate -DarchetypeCatalog=http://petals.ow2.org' </em>
