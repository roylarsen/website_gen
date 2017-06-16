+++
type = "post"
draft = false
description = "certificate authority tool"
topics = [
  "topic 1",
]
title = "certificates"
author = "roy."
keywords = [
  "https",
  "ruby",
  "infrastructure",
  "utilities",
  "openssl"
]
tags = [
  "https",
  "ruby",
  "openssl",
  "love"
]
date = "2017-02-14T23:07:05-05:00"

+++

# Running a certificate authority is kinda annoying

I recently had a developer come to me with a question about an error they were getting with the [Requests](http://docs.python-requests.org/en/master/) Python Module.

The default for this module is to validate SSL certs. Makes sense, right? Validating SSL should be a thing you should do with HTTPS (you ARE using HTTPS for things, right?)

The issue came up where in development, the dev set validate to false so they could test hitting an API endpoint. Then they started moving from dev to test, removed that flag (or turned it to True), 
and then started having issues and didn't understand why.

The wheels started turning and I started thinking of a service I wanted to build.

### Enter the fact that I've been working on learning Ruby and this is a good excuse to write something 'serious-ish' in Ruby

While Python is <3 for me, I've been learning Ruby since it's also very popular within the DevOps space.

I have one project that I'm playing around with, this one will be slightly easier to write. This will be mostly a way for me to work on putting something together that's distributable.

### Nuts and bolts as I think of it now

My first release I want to provide just three API endpoints (a one-time POST, and two GETs), and a CLI utility for setting up a CA.

Requesting a Cert will work on a principle of 'unique enough' identifiers. Since this is designed strictly for internal/enterprise/microservice use, you should be able to handle 
identity management by hand (this will be something to add on to in the future, perhaps).

### tl;dr because Roy isn't a good writer

My scheme of schemes is to allow DevOps/Infrastructure management to be able to do a series of scripts (or cURL ffs) to generate a 'unique enough' identifier and then call these URLs from your
preferred configuration management tool (or even something like cloud-init) and request a certificate for the system automagically. It'll allow organizations to easily enough distribute the
ca-bundle, also automagically.
