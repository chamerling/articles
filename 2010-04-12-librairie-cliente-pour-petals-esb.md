---
layout: post
title: Librairie cliente pour Petals ESB
categories:
- PEtALS
tags:
- Java
- PEtALS
- petalspourlesnuls
- tip
- WebService
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _syntaxhighlighter_encoded: '1'
  _oembed_f58203a8f421e020825948bb2810f3fc: ! '{{unknown}}'
  _oembed_9fcf01ffad90131da4731cbd39eac20f: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2010-04-12 09:53:39'
---
Avoir un Bus de services basé sur la spécification JBI c'est bien. Pouvoir le manager avec l'API JMX qui est définie dans le spécification c'est aussi pas mal, mais pouvoir le manager un utilisant des protocoles un peu plus 'Internet-friendly' et avec une librairie Java c'est encore mieux. Dans cet article, je vais présenter rapidement comment utiliser la librairie cliente qui est disponible depuis que Petals ESB fournit une API Web service ie depuis la version 3.0.

<strong>Le cas d'usage</strong>

Petals ESB est pleinement basé sur JBI est ne fournit pas forcément une API permettant d'exposer des services simplement. On pourrait imaginer avoir une API de management du style 'bind(wsdlURI)' où wsdlURI est l'URI du WSDL du service à lier au bus. La distribution de base ne fournissant pas ce genre d'API, il faut contourner cette lacune par l'utilisation de JBI et des APIs de management à distance.

<strong>La pratique et le code</strong>

Je passe sur la génération de Service Units et de Service Assemblies (des articles sont disponibles sur mon blog pur le faire avec des librairies Java ou avec le Petals Studio). Imaginons que nous ayons généré tout les artifacts JBI nécessaires coté client et que tous les composants JBI sont déjà installés sur le Bus. Comment soumettre ma SA au bus, et comment gérer son cycle de vie à distance?

[sourcecode language="java"]

package net.chamerling.blog.petalsclient;

import java.io.File;

import javax.activation.DataHandler;
import javax.activation.FileDataSource;

import org.ow2.petals.kernel.ws.api.DeploymentService;
import org.ow2.petals.kernel.ws.api.PEtALSWebServiceException;
import org.ow2.petals.kernel.ws.api.to.AttachmentDescriptor;
import org.ow2.petals.kernel.ws.client.PetalsClient;
import org.ow2.petals.kernel.ws.client.PetalsClientFactory;

/**
 * Sample which is using the petals-kernel-wsclient library
 *
 * @author chamerling
 *
 */
public class App {

	public static void main(String[] args) {

		try {
			// get a client
			PetalsClient client = PetalsClientFactory.getInstance().getClient(
					&amp;quot;http://localhost:7600/petals/ws&amp;quot;, 20000);

			// 'upload' the SA in the artifact repository
			File saFile = new File(&amp;quot;sa.zip&amp;quot;);
			AttachmentDescriptor ds = new AttachmentDescriptor();
			ds.setAttachment(new DataHandler(new FileDataSource(saFile)));
			client.getArtifactRepositoryService().addArtifact(ds);

			// Deploy the SA and start it
			String saId = &amp;quot;my-sa-id&amp;quot;;
			DeploymentService dClient = client.getDeploymentService();
			dClient.deploy(saId);
			dClient.start(saId);

		} catch (PEtALSWebServiceException e) {
			e.printStackTrace();
		}
	}
}

[/sourcecode]

Tout est dans le code :
<ol>
	<li>La SA est déposée dans le répertoire géré par le service d'artifact (par défaut, sous PETALS_HOME/artifacts/)</li>
	<li>La SA est déployée depuis le répertoire cité en 1</li>
	<li>La SA est démarrée.</li>
</ol>
Le code de l'exemple est disponible dans mon projet googlecode sous <a href="http://code.google.com/p/chamerling/source/browse/trunk/blog/petals-ws-client">http://code.google.com/p/chamerling/source/browse/trunk/blog/petals-ws-client</a>
