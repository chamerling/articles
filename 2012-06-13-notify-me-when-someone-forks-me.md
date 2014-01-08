---
layout: post
title: Notify me when someone forks me
categories:
- Java
tags:
- Application programming interface
- GitHub
- Heroku
- JSON
- playframework
- Pubsubhubbub
- quickhub
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_skip_tumblr: '1'
  _wpas_mess: Getting notified when someone forks you
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _wp_old_slug: getting-notified-when-someone-forks-you
  _oembed_9e53c39e49afa53cc8589bd4d729a1f6: ! '{{unknown}}'
  _oembed_74c35b5efd87e308ba27ab59017a90d1: ! '{{unknown}}'
  _oembed_28b9c69fd34198164cd4df18e15b5844: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2012-06-13 08:21:01'
  twitter_cards_summary_img_size: a:6:{i:0;i:1117;i:1;i:908;i:2;i:3;i:3;s:25:"width="1117"
    height="908"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}
---
All is cool in <a href="http://github.com" target="_blank">Github</a> and the API really rocks! I created several tools around it and especially <a href="http://quickhubapp.com" target="_blank">QuickHub</a>, an OS X app to quickly access to your Github space and be notified when something changes.

Last night, I was thinking about creating something which can notify you when someone forks or start watching your repositories even if you are offline. So I had a look to the <a href="http://developer.github.com/v3/repos/hooks/" target="_blank">Repository Hook API</a> and just found that almost all the hooks are targeting push ie you can use tens of connector to be notified on the system of your choice only when a push occurs on your repository. Let's have a look to the event API but this is not a really good idea, I already use it in QuickHub to notify about activities and it's just about polling for changes.
So what? Hopefully, there is one alternative: The <a href="http://code.google.com/p/pubsubhubbub/" target="_blank">pubsubhubbub API</a>. According to the GitHub documentation, one can subscribe to any event type using this API. You just have to tell which one on which repository and give it a callback. Giving it a callback is just what we want: Having something which runs online and which can receive events and notify me even if I am offline... Here we go for some Web development with the <a href="http://playframework.org" target="_blank">PlayFramework</a> and <a class="zem_slink" title="Heroku" href="http://www.heroku.com/" rel="homepage" target="_blank">Heroku</a> for hosting, here is <strong>mygithubnotifier</strong>.

<strong>mygithubnotifier</strong> uses the <a href="https://github.com/eclipse/egit-github" target="_blank">GitHub Java API from Eclipse</a>. Even if JSON is easy to parse, having it already done is faster. The application retrieves all your personal repositories and provides the user the choice to subscribe/unsubscribe using the pubsubhubbub protocol. This protocol is really simple to use: One endpoint and three parameters are enough. If I want to subscribe to forks on my repository, I just need an authenticated HTTP call like to <em>https://api.github.com/hub</em> with some parameters for defining the topic (repository), the mode (subscribe/unsubscribe), the event type (fork, watch, ...) and the callback URL which will be some REST endpoint exposed by the Play application. The application will send an email every time someone forks one of your repository if you subscribed to this event before.

<a href="http://f.cl.ly/items/3v3e1A211H2J360F3U2A/mygithubnotifier.png"><img class="aligncenter" title="MyGithubNotifier" src="http://f.cl.ly/items/3v3e1A211H2J360F3U2A/mygithubnotifier.png" alt="" width="1117" height="908" /></a>

The code is really simple and will not be detailed here. You can get it from GitHub at <a href="https://github.com/chamerling/mygithubnotifier" target="_blank">chamerling/mygithubnotifier</a>. I assume that deploying the Play application to Heroku is as easy as described in their documentation. You just have to configure the application by specifying the right email and GitHub properties as described in the <a href="https://github.com/chamerling/mygithubnotifier/blob/master/README.md" target="_blank">README.md</a>.
