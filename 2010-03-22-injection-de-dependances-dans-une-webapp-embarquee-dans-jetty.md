---
layout: post
title: Injection de dépendances dans une WebApp embarquée dans Jetty
categories:
- Java
tags:
- classloader
- Java
- jetty
- opensource
- PEtALS
- servlet
- tip
- webapp
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2010-03-22 15:55:06'
---
<blockquote>...ou comment se battre quelques heures avec les ClassLoaders...</blockquote>
<strong>Le contexte</strong>

J'aimerais bien pouvoir démarrer une WebApp depuis un Jetty embarqué dans mon application et rendre accessible des objets de mon application dans la WebApp.

<strong>Le problème</strong>

Mon application et ma WebApp ont deux ClassLoaders bien distincts = Problème de 'ClassCastException'

<strong>L'explication de la solution</strong>

Pour ma part, l'application en question qui va démarrer la tambouille et partager des objets est le bus de service <a href="http://petals.ow2.org">Petals</a>. La WebApp, elle, est développée comme une WebApp classique, elle n'a pas de lien avec mon application sinon une interface, que je qualifierais, d'échange.

L'implémentation de cette interface est injectée par mon application dans le contexte de la WebApp. Les problèmes commencent ici. En fait le seul problème est un soucis de ClassLoader. En effet, dans la WebApp, le ClassLoader est créé avec le contenu de <em>WEB-INF/lib/*</em>, de <em>WEB-INF/classes/*</em> et du ClassLoader du container Web (on fait souvent le similaire pour expliquer le ClassLoader d'un composant JBI dans le container JBI Petals). Des ressources créées dans mon application ont (généralement mais pas toujours) un ClassLoader spécifique à l'application. Si je les injecte dans la WebApp via le <em>ServletContext</em>, que je les récupère et que j'essaie de les 'caster', je me retrouve avec un beau ClassCastException du style '<em>foo.Bar cannot be cast to foo.Bar</em>'. Les personnes développant des applications standard, ne se soucient généralement pas des histoires de ClassLoader et peuvent donc rester perplexes devant un message de la sorte. Quand on développe un container de n'importe quel type, on se dit que la il y a un soucis de ClassLoader ie on essaye de 'caster' une ressource qui n'a pas le même ClassLoader que celui du Thread courant. Si on regarde plus finement en passant en mode debug ce qu'il se passe lorsque l'on fait quelque chose du style :

[sourcecode language="java"]
Bar bar = (Bar)o;
[/sourcecode]

On s'aperçoit bien que la JVM va appeler la méthode l<em>oadClass(String className)</em> du ClassLoader à un moment où à un autre et que là ca va poser problème. La solution à notre problème est simplement de créer notre propre ClassLoader et 'd'overrider'<em> loadClass(String className)</em> intelligemment pour pouvoir enfin accéder à nos objets dans notre WebApp.

<strong>Le code</strong>

Disons que j'ai une classe <em>foo.BarImpl</em> qui implémente <em>foo.Bar</em>. Je veux passer une instance de cette classe <em>BarImpl</em> à ma WebApp via le contexte de l'application Web (quelques commentaires dans le code suivant).

[sourcecode language="java"]
package foo.bar.myapp;

import java.io.File;
import java.io.IOException;

import org.mortbay.jetty.Server;
import org.mortbay.jetty.webapp.WebAppContext;

public class MyApp {

       public void startServer() Exception {

        File webapp = new File(&quot;mywebapp.war&quot;);

        if ((webapp == null) || !webapp.exists()) {
            throw new Exception(&quot;Can not find the Web application&quot;);
        }

        this.server = new Server(9999);
        WebAppContext context = new WebAppContext();
        context.setContextPath(&quot;/&quot;);
        context.setWar(webapp.getAbsolutePath());

        Foo foo = new FooImpl();
        try {
            // create classloader with current object classloader
            MyClassLoader classloader = new MyClassLoader(foo.getClass()
                    .getClassLoader(), context);
            context.setClassLoader(classloader);
        } catch (IOException e1) {
        }
// put the object in the Jetty context ie in the servlet context
        context.getServletContext().setAttribute(&quot;foo&quot;, foo);
        context.setAttribute(&quot;foo&quot;, foo);

        this.server.setHandler(context);
        try {
            this.server.start();
        } catch (Throwable e) {
        }
    }
}
[/sourcecode]

La subtilité dans le code précédent est l'utilisation de '<em>MyClassLoader</em>'. Ici '<em>MyClassLoader</em>' étend '<em>org.mortbay.jetty.webapp.WebAppClassLoader</em>' et redéfinit la méthode '<em>loadClass(String name)</em>' de la facon suivante:

[sourcecode language="java"]
   @Override
    public synchronized Class loadClass(String name) throws ClassNotFoundException {
        Class result = null;
        try {
            result = this.myAppClassLoader.loadClass(name);
        } catch (Exception e) {
        }
        if (result == null) {
            result = super.loadClass(name);
        }
        return result;
    }
&lt;pre&gt;[/sourcecode]

Où on a '<em>myAppClassLoader</em>' qui est le ClassLoader de mon application que j'ai passé lors de l'instanciation de mon ClassLoader (dans le listing 1 : <em>foo.getClass().getClassLoader()</em>).

De l'autre coté j'ai une Servlet qui peut maintenant récupérer l'objet stocké dans le contexte sous l'attribut nommé '<em>foo</em>' sans problème de ClassCastException, par exemple dans la méthode '<em>init</em>' de la Servlet :

[sourcecode language="java"]
    @Override
    public void init() throws ServletException {
        Object o = this.getServletContext().getAttribute(&quot;foo&quot;);
        if (o != null) {
            Foo foo = null;
            try {
                foo = (Foo) o;
            } catch (RuntimeException e1) {
            }
        } else {
        }
    }
[/sourcecode]

<strong>Le mot de la fin</strong>

En me mettant d'accord sur le contract de l'objet d'échange, je peux développer une application Web indépendamment, travailler avec un mock en mode test et basculer finalement sur mon application le moment venu. Finit aussi les appels Web service ou JMX locaux pour accéder à l'application qui lance le container Jetty, c'est plus propre, plus rapide, bref c'est mieux et ca marche!
