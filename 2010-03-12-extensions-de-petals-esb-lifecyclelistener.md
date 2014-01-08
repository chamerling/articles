---
layout: post
title: ! 'Extensions de Petals ESB : LifeCycleListener'
categories:
- PEtALS
tags:
- documentation
- fractal
- Java
- jbi
- management
- opensource
- ow2
- PEtALS
- SOA
- tip
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2010-03-12
    15:39:53";}
  _elasticsearch_indexed_on: '2010-03-12 15:39:53'
---
<a href="http://petals.ow2.org">Petals ESB</a> offre des possibilités d'extensions assez impressionnantes venant du fait que l'on utilise le Framework à composants <a href="http://fractal.ow2.org">Fractal</a> (bon c'est pas Spring mais on peut faire des choses intéressantes avec des produits issus de la recherche). Dans cet article je vais m'intéresser à la customisation du démarrage et de l'arrêt de Petals en proposant une extension sympa et ce sans modifier énormément le code du kernel de Petals (Ca peut aussi servir de tutorial Fractal pour les débutants...).
Pourquoi? Parce-que actuellement pour moi Petals ESB est le framework SOA que j'étends pour lui ajouter des fonctionnalités qui ne sont pas fournies par défaut. La fonctionnalité du jour doit me permettre d'attendre que tous les composants par défaut soit démarrés pour démarrer les miens : Je dois installer des composants JBI (ne marchera pas si je laisse Fractal les installer trop tôt), lier des services (ne marchera pas si mes composants ne sont pas installés et si le service de management de Petals n'est pas démarré), lancer des tâches de fond, etc...

l'API du kernel de Petals propose l'interface o<em>rg.ow2.petals.kernel.api.server.PetalsStateListener</em> :

[sourcecode language="java"]
package org.ow2.petals.kernel.api.server;

/**
 * Listener notified when a Petals container finish to start or to stop.
 */
public interface PetalsStateListener {

    /**
     * Callback method on PEtALS start up
     */
    void onPetalsStarted();

    /**
     * Callback method on PEtALS stop
     *
     * @param success
     *            {@code true} if PEtALS has stopped successfully
     * @param exception
     *            the exception if the stop has failed, {@code null} otherwise
     */
    void onPetalsStopped(boolean success, Exception exception);
}
&lt;pre&gt;[/sourcecode]

Cette interface n'est malheureusement utilisée que statiquement et une seule fois pour informer le 'launcher' que le kernel est bien démarré. On a pourtant <em>org.ow2.petals.kernel.api.server.PetalsServer</em> qui définit la méthode <em>addPetalsStateListener(PetalsStateListener)</em>. je vais donc utiliser cette API et modifier simplement l'implémentation de <em>org.ow2.petals.kernel.api.server.PetalsServer</em> pour ajouter des listeners qui seront appelés lors du démarrage et de l'arrêt de Petals.

Pour ce faire deux approches sont possibles :
<ol>
	<li>Rechercher tous les composants Fractal qui implémentent <em>org.ow2.petals.kernel.api.server.PetalsStateListener</em></li>
	<li>Créer un manager qui contient une liste ordonnée de tous les <em>org.ow2.petals.kernel.api.server.PetalsStateListener</em></li>
</ol>
On va ici se pencher sur la solution 2 car la solution 1 ne permet d'ordonner facilement les listeners à appeler (ca peut valoir le coup de les appeler dans un sens bien définit!).

<strong>1/ Le manager de listeners</strong>

Simplement un composant Fractal qui détient la liste des listeners définis. Ces listeners sont définis par configuration dans les fichiers ADL de Petals. Son interface :

[sourcecode language="java"]
package foo.bar.petals.kernel.listener;

import java.util.Set;

import org.ow2.petals.kernel.api.server.PetalsStateListener;

/**
 * @author chamerling - eBM WebSourcing
 *
 */
public interface LifeCycleListenerManager {

    /**
     * The binding prefix of the component which is listening petals state
     */
    static final String PREFIX = &quot;petals-state-listener-&quot;;

    /**
     * Get an ordered set of listeners
     *
     * @return
     */
    Set&lt;PetalsStateListener&gt; getListeners();

}
[/sourcecode]

et son implémentation :

[sourcecode language="java"]
package foo.bar.petals.kernel.listener;

import java.util.HashSet;
import java.util.Hashtable;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

import org.objectweb.fractal.fraclet.annotation.annotations.FractalComponent;
import org.objectweb.fractal.fraclet.annotation.annotations.Interface;
import org.objectweb.fractal.fraclet.annotation.annotations.LifeCycle;
import org.objectweb.fractal.fraclet.annotation.annotations.Monolog;
import org.objectweb.fractal.fraclet.annotation.annotations.Provides;
import org.objectweb.fractal.fraclet.annotation.annotations.Requires;
import org.objectweb.fractal.fraclet.annotation.annotations.type.Cardinality;
import org.objectweb.fractal.fraclet.annotation.annotations.type.Contingency;
import org.objectweb.fractal.fraclet.annotation.annotations.type.LifeCycleType;
import org.objectweb.util.monolog.api.Logger;
import org.ow2.petals.kernel.api.server.PetalsStateListener;
import org.ow2.petals.util.LoggingUtil;

/**
 * @author chamerling
 *
 */
@FractalComponent
@Provides(interfaces = { @Interface(name = &quot;service&quot;, signature = LifeCycleListenerManager.class) })
public class LifeCycleListenerManagerImpl implements LifeCycleListenerManager {

    @Monolog(name = &quot;logger&quot;)
    private Logger logger;

    private LoggingUtil log;

    @Requires(name = PREFIX, signature = PetalsStateListener.class, cardinality = Cardinality.COLLECTION, contingency = Contingency.OPTIONAL)
    private final Map&lt;String, Object&gt; listeners = new Hashtable&lt;String, Object&gt;();

    @LifeCycle(on = LifeCycleType.START)
    protected void start() {
        this.log = new LoggingUtil(this.logger);
        this.log.debug(&quot;Starting...&quot;);
    }

    @LifeCycle(on = LifeCycleType.STOP)
    protected void stop() {
        this.log.debug(&quot;Stopping...&quot;);
    }

    /**
     * {@inheritDoc}
     */
    public Set&lt;PetalsStateListener&gt; getListeners() {
        HashSet&lt;PetalsStateListener&gt; result = new HashSet&lt;PetalsStateListener&gt;();
        if (this.listeners != null) {
            Iterator&lt;String&gt; iter = this.listeners.keySet().iterator();
            while (iter.hasNext()) {
                String key = iter.next();
                Object o = this.listeners.get(key);
                if (o instanceof PetalsStateListener) {
                    result.add((PetalsStateListener) o);
                } else {
                    // warning
                    this.log.warning(o + &quot; is not an instance of &quot;
                            + PetalsStateListener.class.getCanonicalName());
                }
            }
        }
        return result;
    }
}
[/sourcecode]

A noter que la Map 'listeners' est remplie par Fractal au démarrage de Petals.

<strong>2/ Modification de PetalsServerImpl</strong>

Dans <em>org.ow2.petals.kernel.server.PetalsServerImpl</em> je rajoute la méthode <em>initListeners()</em> :

[sourcecode language="java"]
   private void initListeners() {
        // get the component which owns the listeners
        Component listenerManagerComponent = FractalHelper.getRecursiveComponentByName(
                this.petalsContentController, Constants.FRACTAL_LISTENERS_MANAGER);
        if (listenerManagerComponent != null) {
            try {
                LifeCycleListenerManager manager = (LifeCycleListenerManager) listenerManagerComponent
                        .getFcInterface(&quot;service&quot;);

                if (manager != null) {
                    Set&lt;PetalsStateListener&gt; listeners = manager.getListeners();
                    for (PetalsStateListener petalsStateListener : listeners) {
                        this.addPetalsStateListener(petalsStateListener);
                    }
                } else {
                }
            } catch (Exception e) {
            }
        }

    }
[/sourcecode]

Ce code permet de retrouver mon 'listener manager' (puisque PetalsServerImpl n'est pas 'Fractalisé') et de remplir la liste des listeners. Allez maintenant, on va dire que j'appelle cette méthode d'initialisation juste avant d'appeler les listeners (dans <em>start()</em> de <em>PetalsServerImpl</em>):

[sourcecode language="java"]
    public void start() throws PetalsException {
        // get listeners which have been defined by configuration (Fractal)
        this.initListeners();

        // notify the listener if there is one
        for (PetalsStateListener petalsListener : this.petalsListeners) {
            petalsListener.onPetalsStarted();
        }
    }
[/sourcecode]

<strong>3/ Configuration des listeners</strong>

Dans les fichiers de définition des composants Fractal, je peux maintenant instancier mes listeners et les ajouter à la liste des listeners gérée par le manager de listeners :

[sourcecode language="xml"]
	&lt;!-- lifeCycle Listeners --&gt;
	&lt;component definition=&quot;foo.bar.petals.kernel.FooService&quot; name=&quot;Foo&quot; /&gt;
	&lt;component definition=&quot;foo.bar.petals.kernel.BarService&quot; name=&quot;Bar&quot; /&gt;

	&lt;!-- Listeners manager --&gt;
	&lt;component definition=&quot;eu.soa4all.dsb.petals.kernel.listener.LifeCycleListenerManagerImpl&quot; name=&quot;LifeCycleListenerManagerImpl&quot; /&gt;

	&lt;!-- Fill the map --&gt;
	&lt;binding client=&quot;LifeCycleListenerManagerImpl.petals-state-listener-foo&quot; server=&quot;Foo.service&quot; /&gt;
	&lt;binding client=&quot;LifeCycleListenerManagerImpl.petals-state-listener-bar&quot; server=&quot;Bar.service&quot; /&gt;
[/sourcecode]

Le composant <em>foo.bar.petals.kernel.*Listener</em> implémentent  <em>org.ow2.petals.kernel.api.server.PetalsStateListener</em> et sont ajoutés à la liste des listeners managés par <em>LifeCycleListenerManagerImpl</em> lors de la définition des liens (lignes 'binding'). Tous les listeners ajoutés au manager doivent avoir une valeur de l'attribut client commençant obligatoirement par <em>LifeCycleListenerManagerImpl.petals-state-listener-</em> (remplissage de la Map par Fractal).

Facile et pratique, non?!
