---
layout: post
title: ! 'Petals ESB TIP : Setting logging level per JBI component'
categories:
- PEtALS
tags:
- PEtALS
- tip
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2009-12-10 16:32:06'
---
Just a quick tip to say that you can define the logger level for each JBI component you deploy into <a href="http://petals.ow2.org">Petals ESB</a> by setting things in the conf/loggers.properties of your favorite Petals distribution. If the component name is petals-bc-restproxy (the component I am currently developping), just define the log level like this :

[sourcecode language="text"]
logger.Petals.Container.Components.petals-bc-restproxy.level DEBUG
logger.Petals.Container.Components.petals-bc-restprox.handler.0 petalsConsole
logger.Petals.Container.Components.petals-bc-restprox.handler.1 petalsFile
[/sourcecode]

The only DEBUG logs which will be displayed will be the petals-bc-restproxy ones.
Good to remember!
