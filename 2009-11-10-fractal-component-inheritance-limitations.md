---
layout: post
title: Fractal Component Inheritance Limitations
categories:
- PEtALS
tags:
- fractal
- Java
- ow2
- PEtALS
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _oembed_74f9cd254d4310d30e0b908a212cee7d: ! '{{unknown}}'
  _oembed_c4123cf9ee31e069075f024f7a03399e: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2009-11-10 15:49:41'
---
<div id="_mcePaste">Here is the problem I had this afternoon and I said really bad things on Fractal and Spoon (if we had a Fractal Box cf  <a href="http://wp.me/pcSx0-3U" target="_blank">http://wp.me/pcSx0-3U</a> we will be able to have liters of beer to drink tonight...).</div>
<div>Here is the compilation stack trace :</div>
<blockquote>
<div>
<div>[WARNING] FractalComponentProcessor &gt;&gt;  No value found for property 'generatorClass' in processor org.objectweb.fractal.fraclet.annotation.processor.FractalComponentProcessor</div>
<div>[INFO] ------------------------------------------------------------------------</div>
<div>[ERROR] BUILD ERROR</div>
<div>[INFO] ------------------------------------------------------------------------</div>
<div>[INFO] fail to execute</div>
</div></blockquote>
... which is really helpful...

After spending too much time on searching why, the cause was 'simply' because my Fractal component (java class) was extending another Fractal component (java class). Not sure that it is a component problem or a component import/requirement one but when one component inherits one other it does not compile at all.

Good to know!

<p>&nbsp;</p>

Update on november 13:

Sometimes things are strange... It seems that this bug has been fixed some months ago (according to the Fractal bug tracker...) but I have it when I try to extend a component which has already been processed by spoon... ie In the same project all works fine... The problem occurs when I have a fractal based project which depends on another one...
