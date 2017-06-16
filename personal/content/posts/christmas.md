+++
draft = false
author = "roy"
topics = [
  "",
]
date = "2016-12-17T11:17:15-05:00"
type = "post"
description = "Christmas week projects"
keywords = [
  "projects",
  "holiday",
]
tags = [
  "holidays",
  "projects",
  "virtualization",
  "saltstack",
  "python",
]
title = "christmas"

+++

# christmas.

It's December 17th. In a month and a half, I'll have lived in the Boston area for a year.

What.
<br />
<br />
<br />
I have one week left of work this year, then HBG is off for Christmas week. 

I'm currently working on coming up with my projects for the week so I don't get bored.

---

~~## hypervisor.~~
###### (proxmox and plex are now things in my env)

I want to rebuild my lab Hypervisor. [XenServer](http://xenserver.org/) is nice and all, but the community support and tooling just isn't there.

Right now, I'm planning on burning everything to the ground and installing [Proxmox](https://www.proxmox.com/en/proxmox-ve).

Once that's a thing, I'll be rebuilding my [Plex](https://www.plex.tv/) VM so I have something to watch.

I'll also price out some RAM and see about doubling my RAM.

---

~~## saltstack.~~
###### (I now have a salt master in my env)

[Saltstack](https://saltstack.com/) is the big reason I'm wanting to rebuild my hypervisor.

Xen is nice, but the client is Windows-only and I want to get into using more [salt-cloud](https://docs.saltstack.com/en/latest/topics/cloud/index.html).

Once my salt-master is in place, my goal is to replicate(-ish) what we're doing at HBG so I have a place to play at home.

---

## skynet-ng

This is more of a for-fun thing I'm not really planning on releasing.

This is a joke about some software HBG Infrastructure is trying to tear out. This will be mostly a overview dashboard of our systems.

Will this get done over break? Probably not

---

## test-salt

Why should chef have all the cool infrastructure testing tools?

I've started playing around with automating [Vagrant](https://www.vagrantup.com/) and the Python [testinfra](https://github.com/philpep/testinfra) module.

[test-kitchen](https://github.com/test-kitchen/test-kitchen) is a great tool for Infrastructure as Code. However, it's a Ruby tool and requires a 
Ruby toolchain to get going. You know what doesn't use Ruby and the people working with it probably wont have a Ruby toolchain installed? People using
Saltstack or Ansible.

I've done a little bit, but will it get done? Maybe.

[test-salt](https://github.com/roylarsen/test-salt)
