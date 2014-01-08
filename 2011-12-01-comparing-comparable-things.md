---
layout: post
title: Comparing comparable things
categories:
- PEtALS
tags:
- esb
- integration
- ow2
- petalslink
- SOA
- Talend
status: publish
type: post
published: true
meta:
  geo_latitude: '43.484082'
  geo_longitude: '3.676690'
  geo_accuracy: '80'
  geo_address: Chemin de Cresse, 34560 Poussan
  geo_public: '0'
  publicize_results: a:1:{s:7:"twitter";a:1:{i:13024132;a:2:{s:7:"user_id";s:10:"chamerling";s:7:"post_id";s:18:"142254416689504256";}}}
  _wpas_done_linkedin: '1'
  _wpas_done_twitter: '1'
  tagazine-media: a:7:{s:7:"primary";s:93:"http://chamerling.files.wordpress.com/2011/12/capture-d_c3a9cran-2011-12-01-c3a0-15-32-41.png";s:6:"images";a:1:{s:93:"http://chamerling.files.wordpress.com/2011/12/capture-d_c3a9cran-2011-12-01-c3a0-15-32-41.png";a:6:{s:8:"file_url";s:93:"http://chamerling.files.wordpress.com/2011/12/capture-d_c3a9cran-2011-12-01-c3a0-15-32-41.png";s:5:"width";s:3:"985";s:6:"height";s:3:"689";s:4:"type";s:5:"image";s:4:"area";s:6:"678665";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:7:"3303881";s:7:"blog_id";s:7:"3069558";s:9:"mod_stamp";s:19:"2011-12-01
    14:51:00";}
  _ideation_attached_images: '1393'
  _oembed_abcd72cd41b4e4c3705eb6b0f12b10cd: ! '{{unknown}}'
  _oembed_a8432ebb5f20d574951e6848dbd9b277: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2011-12-01 14:51:00'
  twitter_cards_summary_img_size: a:6:{i:0;i:985;i:1;i:689;i:2;i:3;i:3;s:24:"width="985"
    height="689"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}
---
Last week at <a href="http://ow2.org" target="_blank">OW2Con</a>, <a href="http://talend.com" target="_blank">Talend</a> CTO talked about their data and service integration solution. This sentence impressed me (almost this one, not sure it was so short):
<blockquote>We have more than 500 connectors!</blockquote>
Wow! Great, let's have a look to that! What is a connector? In the<a href="http://petals.ow2.org" target="_blank"> Petals ESB</a> context connectors are the bindings represented in the upper part of this well-known image

[caption id="attachment_1393" align="aligncenter" width="584" caption="Petals ESB"]<img class=" wp-image-1393 " title="Petals ESB" src="http://chamerling.files.wordpress.com/2011/12/capture-d_c3a9cran-2011-12-01-c3a0-15-32-41.png" alt="Petals ESB" width="584" height="408" />[/caption]

So, we have almost 8 connectors. Looks that we are poor... So let's look in details what is a Talend connector: <a href="http://www.talendforge.org/components/" target="_blank">http://www.talendforge.org/components/</a>.

Can you see that? A Talend component is almost an operation in a service, or let's say that it is an input/output from a data source. So a service is not as generic as a Petals service but it is a specialized service i.e. we can also have tons of 'components' for Petals ESB, it just means that we have to provide configuration artifacts for all the services that we find: Salesforce, Amazon EC2, S3, all the databases in the world, etc, ... Oh no, wait! Don't you see the Talend component on the bottom of the figure just above? Yes we have a Talend connector so we have 500+ components in Petals ESB :)

No really, Talend guys really do an incredible work and their recent press releases are really impressive. Congrats!

&nbsp;
