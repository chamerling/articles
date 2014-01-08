---
layout: post
title: Too many open files IOException with large number of Axis2 Servic
categories:
- WebService
tags: []
status: publish
type: post
published: true
meta:
  syndication_source: Christophe Hamerling
  syndication_source_uri: http://www.ebmwebsourcing.net/blog/page/christophehamerling
  rss:comments: http://www.ebmwebsourcing.net/blog/page/christophehamerling?anchor=too_many_open_files_ioexception
  syndication_feed: http://ebmwebsourcing.com/blog/rss/christophehamerling
  syndication_feed_id: '3'
  syndication_permalink: http://www.ebmwebsourcing.net/blog/page/christophehamerling?entry=too_many_open_files_ioexception
  _elasticsearch_indexed_on: '2008-01-10 07:43:42'
---
This is a problem we met in the PEtALS SOAP Binding Component with large number of Axis2 Service Clients.<br />In this case, this error happens when too many sockets are open ant are waiting to be closed. Even if I have found some JIRAs about this problem and possible solutions, the only one which work for us is to cleanup the connection after each service call.<br />It is done like this :<br /><br /><div><table id="zxl6" bgcolor="#cccccc" border="0" cellpadding="3" cellspacing="0" width="90%"><tbody><tr><td width="100%">   	 	 	 	 	 	 	  <p align="left">    <font color="#000000"><font face="Monospace"><font size="2">...</font></font></font></p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">ServiceClient client = </font><font color="#7f0055"><b>new</b></font><font color="#000000"> ServiceClient(</font><font color="#7f0055"><b>null</b></font><font color="#000000">, service);</font></font></font></p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">Options options = </font><font color="#7f0055"><b>new</b></font><font color="#000000"> Options();</font></font></font></p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">...</font></font></font></p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">options.setCallTransportCleanup(</font><font color="#7f0055"><b>true</b></font><font color="#000000">);</font></font></font></p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">client.setOptions(options);</font></font></font></p> <p align="left"><br /> </p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">OMElement outBodyElement = </font><font color="#7f0055"><b>null</b></font><font color="#000000">;</font></font></font></p> <p align="left">    <font face="Monospace"><font size="2"><font color="#7f0055"><b>try</b></font><font color="#000000"> {</font></font></font></p> <p align="left"><font color="#000000">      <font face="Monospace"><font size="2">outBodyElement = serviceClient.sendReceive(soapAction, inBodyElement);</font></font></font></p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">} </font><font color="#7f0055"><b>catch</b></font><font color="#000000"> (AxisFault e) {</font></font></font></p> <p align="left">      <font face="Monospace"><font size="2"><font color="#7f0055"><b>throw</b></font><font color="#000000"> e;</font></font></font></p> <p align="left"><font color="#000000">    <font face="Monospace"><font size="2">}</font></font></font></p> </td></tr></tbody></table></div><br /> 	 	 	 	 	 	<br />After 10 minutes test with 200 threads creating 'one shot ' ServiceClient and almost 275000 service calls, all worked fine.<br />
