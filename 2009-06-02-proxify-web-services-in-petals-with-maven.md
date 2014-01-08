---
layout: post
title: Proxify Web Services in PEtALS with Maven
categories:
- PEtALS
tags:
- maven
- PEtALS
- WebService
status: publish
type: post
published: true
meta:
  _oembed_905d783a48e7c09156f66b118a24d641: ! '{{unknown}}'
  _oembed_dd19db0b2d7ef4084eb7f97846908db3: ! '{{unknown}}'
  _oembed_a6d829e8e270f617273c5b9b02d82c25: ! '{{unknown}}'
  _oembed_171dfbcc8f34804faba273a803ea9be1: ! '{{unknown}}'
  _oembed_9372cde3ee5d53de713c436ca4cfc8b0: ! '{{unknown}}'
  _oembed_6216aa0f7cf6484a576c71cd36820862: ! '{{unknown}}'
  _oembed_32f44fb5d1eed672bc545b6749880164: ! '{{unknown}}'
  _oembed_1c425d55ab123031311a2fc8943b0f4e: ! '{{unknown}}'
  _oembed_22092ded62c25b16f058650d09453cd9: ! '{{unknown}}'
  _oembed_de7c081303d5a3e08612681d1b2fb810: ! '{{unknown}}'
  _oembed_1d6cdd8d8048a51c7635034cd90c51e0: ! '{{unknown}}'
  _oembed_14b89e4e1daaf073d49ed08ba364a86b: ! '{{unknown}}'
  _oembed_c7105e5d3a325f0d150bbaf3c3b94b2b: ! '{{unknown}}'
  _oembed_1834714fc5cae60750cedb27e5ba5bc5: ! '{{unknown}}'
  _oembed_a97da5486b293a655b39007d78a9ac57: ! '{{unknown}}'
  _oembed_54a96569d506973b347af8d29161ddf4: ! '{{unknown}}'
  _oembed_265fcb87e1d4a7e4fa6a68d0132a6978: ! '{{unknown}}'
  _oembed_5d44fc10598492beaae04a90920b64ed: ! '{{unknown}}'
  _oembed_3ff3568d592002ee0315b99112277e03: ! '{{unknown}}'
  _oembed_7e0201ba08333a6b05b21f4bb10b9909: ! '{{unknown}}'
  _oembed_124cee35cc2bf21482d371c144cc4117: ! '{{unknown}}'
  _oembed_2520644b11f6eab50a8efe516c6ad8d5: ! '{{unknown}}'
  _oembed_bc156eb174fd9c16032bb7edefc54c18: ! '{{unknown}}'
  _oembed_be6d62493fe31674d93020e8da948a31: ! '{{unknown}}'
  _oembed_933d1031adb927a49f613830eda837dd: ! '{{unknown}}'
  _oembed_800622d3268bfe99b2b1249a01d4f9e1: ! '{{unknown}}'
  _oembed_509749ff1da153095de3639ce7925cda: ! '{{unknown}}'
  _oembed_55ee9304103a34db113ca246fc3fa4c0: ! '{{unknown}}'
  _oembed_91f79f4948af4a2a74a14a3f09b42df5: ! '{{unknown}}'
  _oembed_9b489d56612ac9d25adbfee0a1ed0879: ! '{{unknown}}'
  _oembed_e1b5e87034bf4eb58034676c71e62f67: ! '{{unknown}}'
  _oembed_76bca5a5b7b2de3d3aa8356933355f79: ! '{{unknown}}'
  _oembed_017e575918bcc2caa8ae9d7ab0de4ba9: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2009-06-02 14:16:21'
---
This is quite the same thing than the previous post where I introduced how to expose JAXWS service in <a href="http://petals.ow2.org">PEtALS ESB</a> with Maven. This time, let's proxify a Web service in PEtALS with Maven.

Here is the Maven descriptor snippet :

<pre>
&lt;build&gt;
	&lt;plugins&gt;
		&lt;plugin&gt;
			&lt;groupId&gt;org.ow2.petals&lt;/groupId&gt;
			&lt;artifactId&gt;maven-petals-wsproxy&lt;/artifactId&gt;
			&lt;version&gt;1.0-SNAPSHOT&lt;/version&gt;
			&lt;executions&gt;
				&lt;execution&gt;
					&lt;id&gt;generate-jbi&lt;/id&gt;
					&lt;phase&gt;package&lt;/phase&gt;
					&lt;configuration&gt;
						&lt;wsdl&gt;
							http://localhost:8080/Service?wsdl
						&lt;/wsdl&gt;
					&lt;/configuration&gt;
					&lt;goals&gt;
						&lt;goal&gt;wsproxy&lt;/goal&gt;
					&lt;/goals&gt;
				&lt;/execution&gt;
			&lt;/executions&gt;
		&lt;/plugin&gt;
	&lt;/plugins&gt;
&lt;/build&gt;
</pre>

This will generate a JBI Service Assembly that you can then deploy intoo PEtALS to proxify the service defined in http://localhost:8080/Service?wsdl

You can give it a try, the snapshot version is available on the <a href="http://maven.ow2.org">OW2 Maven repository</a>...
