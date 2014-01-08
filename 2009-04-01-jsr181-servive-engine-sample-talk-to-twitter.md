---
layout: post
title: ! 'JSR181 Servive Engine sample : Talk to Twitter'
categories:
- PEtALS
- SOCIAL
tags:
- PEtALS
- SOCIAL
- twitter
status: publish
type: post
published: true
meta:
  _oembed_940f0de01770cca215e29b6e10d61cd6: ! '{{unknown}}'
  _oembed_bb9a6e91e3c5ef0268979e15e353bc4e: ! '{{unknown}}'
  _elasticsearch_indexed_on: '2009-04-01 11:29:11'
---
Just for fun...

The current article will show you that the JSR181 Service Engine really provides an easy way to create JBI services. Since creating simple HelloWorld service is quite boring, let's talk to twitter micro blogging site.

I will use the Twitter4J API () to talk to Twitter (right all the Twitter job is done here... The current article is not a Twitter tutorial but just a small PEtALS one...).

Interface definition
<pre>
package net.chamerling.petals.twitter;

import javax.jws.WebMethod;
import javax.jws.WebParam;
import javax.jws.WebService;

/**
 * @author chamerling
 *
 */
@WebService
public interface TwitterService {

	@WebMethod
	public String update(@WebParam(name = "id") String login,
			@WebParam(name = "password") String password,
			@WebParam(name = "status") String status) throws TwitterServiceException;

	@WebMethod
	public String[] getTimeLine(@WebParam(name = "id") String login,
			@WebParam(name = "password") String password,
			@WebParam(name = "user") String user) throws TwitterServiceException;
}</pre>

Implementation (quick)
<pre>package net.chamerling.petals.twitter;

import java.util.ArrayList;
import java.util.List;

import javax.jws.WebMethod;
import javax.jws.WebParam;
import javax.jws.WebService;

import twitter4j.Status;
import twitter4j.Twitter;
import twitter4j.TwitterException;

/**
 * @author chamerling
 *
 */
@WebService(serviceName = "TwitterService", name = "TwitterService", targetNamespace = "http://twitter.chamerling.net/petals")
public class TwitterServiceImpl implements TwitterService {

	/*
	 * (non-Javadoc)
	 * @see net.chamerling.petals.twitter.TwitterService#update(java.lang.String, java.lang.String, java.lang.String)
	 */
	@WebMethod
	public String update(@WebParam(name = "id") String login,
			@WebParam(name = "password") String password,
			@WebParam(name = "status") String status) throws TwitterServiceException {
		String result = null;
		try {
			Status s = getTwitter(login, password).update(status);
			result = s.getText();
		} catch (twitter4j.TwitterException e) {
			throw new TwitterServiceException(e.getMessage());
		}
		return result;
	}

	/*
	 * (non-Javadoc)
	 * @see net.chamerling.petals.twitter.TwitterService#getTimeLine(java.lang.String, java.lang.String, java.lang.String)
	 */
	@WebMethod
	public String[] getTimeLine(@WebParam(name = "id") String login,
			@WebParam(name = "password") String password,
			@WebParam(name = "user") String user) throws TwitterServiceException {
		Twitter twitter = getTwitter(login, password);
		List result = new ArrayList();
		List status = null;
		try {
			if (user == null) {
				status = twitter.getUserTimeline();
			} else {
				status = twitter.getUserTimeline(user);
			}
		} catch (TwitterException e) {
			throw new TwitterServiceException(e.getMessage());
		}

		for (Status status2 : status) {
			result.add(status2.getText());
		}

		return result.toArray(new String[0]);
	}

	/**
	 * TODO : Some work to do for caching...
	 *
	 * @param login
	 * @param password
	 * @return
	 */
	private Twitter getTwitter(String login, String password) {
		return new Twitter(login, password);
	}
}</pre>

Put all of this in a JSR181 Service Unit, ie create the good Service Unit descriptor (cf to source attchment), package it (use PEtALS Maven plugin) and that's all. JSR181 makes it easy ;o)

So now you can publish some status to Twitter with PEtALS.
I have created a test acount here <a href="http://twitter.com/chamerlingtest">http://twitter.com/chamerlingtest</a> on which I have published messages with PEtALS.

Service Unit sources are available here : <a href="http://dl.getdropbox.com/u/73785/blog/twitter-jsr181.zip">http://dl.getdropbox.com/u/73785/blog/twitter-jsr181.zip</a>
