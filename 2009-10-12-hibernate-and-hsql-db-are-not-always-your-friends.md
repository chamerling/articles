---
layout: post
title: Hibernate and HSQL DB are not always your friends
categories:
- Java
tags:
- hibernate
- Java
- mapping
status: publish
type: post
published: true
meta:
  _wpas_done_yup: '1'
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2009-10-12 13:53:52'
---
There are some things to know when working with Hibernate mappings and HSQL DB. The most important one is that running something on HSQL does not mean that it will run on another DB engine...

Let's take the following annotated class :

[sourcecode language="java"]
package org.ow2.petals.registry.core.repository.bo;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

import org.hibernate.annotations.GenericGenerator;

@Entity
public class Property {

    private String id;

    private String key;

    private String value;

    public Property() {
    }

    @Id
    @GeneratedValue(generator = &quot;system-uuid&quot;)
    @GenericGenerator(name = &quot;system-uuid&quot;, strategy = &quot;uuid&quot;)
    public String getId() {
        return this.id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getKey() {
        return this.key;
    }

    public void setKey(String key) {
        this.key = key;
    }

    public String getValue() {
        return this.value;
    }

    public void setValue(String value) {
        this.value = value;
    }
}
[/sourcecode]

This will not cause any problem running on HSQL DB but it will not run at all on MySQL for example... The problem is that 'key' if a reserved SQL keyword. So renaming 'key' fixes this problem.

I just want to know why Hibernate guys do not detect reserved keywords and prefix/postfix them to be SQL compliant. The other question is why Hibernate does not throw an exception but just print a warning when the table can not be created...
