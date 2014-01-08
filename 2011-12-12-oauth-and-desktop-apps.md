---
layout: post
title: OAuth and Desktop Apps
categories:
- QuickHub
- WebService
tags:
- Application programming interface
- GitHub
- Hypertext Transfer Protocol
- OAuth
- petalslink
- quickhub
- Uniform Resource Locator
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  _wpas_mess: ! 'OAuth and Desktop Apps: http://wp.me/pcSx0-mL'
  _wpas_skip_yup: '1'
  _wpas_skip_twitter: '1'
  _wpas_done_linkedin: '1'
  _ideation_attached_images: '1424'
  _elasticsearch_indexed_on: '2011-12-12 11:39:23'
  twitter_cards_summary_img_size: a:7:{i:0;i:1296;i:1;i:968;i:2;i:2;i:3;s:25:"width="1296"
    height="968"";s:4:"bits";i:8;s:8:"channels";i:3;s:4:"mime";s:10:"image/jpeg";}
---
I started to develop <a href="http://quickhubapp.com" target="_blank">QuickHub</a> some weeks ago by focusing on features and without taking into account security issues such as this critical information which are login and password credentials... As mentioned in the <a class="zem_slink" title="GitHub" href="http://github.com" rel="homepage">GitHub</a> developer pages, I started to use Basic Auth for all the requests QuickHub does to get retrieve data from GitHub. I started to think about moving to <a class="zem_slink" title="OAuth" href="http://oauth.net" rel="homepage">OAuth</a> when a user said me that he bought QuickHub but that he did not use it because of Basic Auth. He was afraid that I can rob its credentials and so have a look to its repositories and more. I totally understand this argument and so I started to think that it limits QuickHub adoption by developers and that I should do something.

So, I discussed with GitHub guys through their support channel (Here I want to say that I am really impressed on how fast and professional is the GitHub support team!). Of course, they also told me that they will never use QuickHub if OAuth is not provided. After discussions with the guys, I started to implement a workaround to provide OAuth support in QuickHub by using the OAuth Web flow and adding some stuff to QuickHub.

Let's see how we can do that in a generic way so that you can use it in your app if needed since every serious Internet service also provide OAuth support... Note that I will not give many details about OAuth itself but I will use some terms, so have a look to OAuth documentation for more details.
<h2>Register a Web application</h2>
The first step is to register your application to the developer portal. Here you need to provide some information such as the app name and its callback URL. Even if we are developing a desktop app and not a Web one, we need to be able to provide this HTTP based callback URL. We will see in the next section what is inside this application. By registering our application, the service will generate and give us some keys to be used in the next steps, mostly to recognize us when calling the service.
<h2>Create a Web application</h2>
We need a Web application to handle callback calls from the service we want to use OAuth. This application will only be used at authorization time and just have to be able to receive the service call, get the code sent by the service, then call the service again with the code and our application keys (there is a secret one to be used here). As a result, the service will send you back your OAuth token to be used in all the future service calls. This token is used to authenticate your service calls, no more password stuff inside! Here is a quick sequence diagram showing all the exchanges between actors...

<img class="aligncenter size-full wp-image-1424" title="Sequence" src="http://chamerling.files.wordpress.com/2011/12/photo-4.jpg" alt="" width="584" height="436" />

On the QuickHub side, I chose to create this Web application by using the <a class="zem_slink" title="Play Framework" href="http://www.playframework.org" rel="homepage">Play Framework</a> I already mentioned several times in this blog. I used the <a class="zem_slink" title="Heroku" href="http://www.heroku.com/" rel="homepage">Heroku</a> paas to provide the Web application publicly.

As showed in this Gist (<a href="https://gist.github.com/1466592" target="_blank">https://gist.github.com/1466592</a>), the callback method is really simple (as usual with Play!): just get the code and call GitHub with it and your application keys. When all lis done, just display a result page with a specific URL. Here it starts with 'quickhubapp://oauth?'. Let's see what it means in the next section...
<h2>Add custom URL handlers to your desktop app</h2>
Once the desktop application is authorized, our Web application redirects the user to a Web page which embeds a button targeting an URL starting by 'quickhubapp://oauth?'. Here you already understand that by clicking on such a link must drive you to something special. The cocoa framework allows developers to register specific URL handlers for their applications. So we need to register QuickHub handlers so that OS X knows that every link  with the quickhubapp scheme must me routed to QuickHub. This is as easy as showed in this Gist <a href="https://gist.github.com/1466628" target="_blank">https://gist.github.com/1466628</a>.

This code snippet just tells QuickHub to invoke the getUrl method when it receives a quickhubapp event. Up to the getUrl method to handle things. In QuickHub, I just extract the OAuth token in order to persist it locally for future use.
<h2>Done!</h2>
So now we are able to use OAuth Web flows for desktop applications. In the best of the worlds, every service provider may provide a real desktop flow to be used without the need to create this additional web application. In the real world, it is not the case but as you see it can be bypassed quite easily.

I will provide more technical details on the second section 'Create a Web application' with real sample and code in the next weeks.
