---
type: "post"
draft: false
author: "roy."
description: "devops(-ish)"
date: "2017-12-15T11:17:15-05:00"
keywords: ["devops", "saltstack", "python", "work"]
topics: ["Devops"]
tags: ["devops", "saltstack", "python", "work"]
title: "devops(-ish)"
---

![Me, basically](/img_dog.png#center)

Well, not really. I have a pretty decent idea of what's going on.. Trust me, I've got this (mostly)
 I've also figured out how to do images in Hugo, #dealwtithit


### mhqa
I'm currently working on authoring a tool in python, and that's been fun to work
on. It's currently pretty limited in scope, but soon I should be able to start
building things out and really start doing some fun Continuous Deployment/Delivery
stuff for our QA people.

Long story short, this tool uses the saltstack API (probably something I'll write a post about in the future)
to interact with the vSphere API to revert back to "clean" snapshots and reinstall our backend
software with a named version on the CLI. In the future, I'll probably kill off the snapshot part of
the tool (because you're not really supposed to use snapshots like that) and just do fresh VMs for installs
(and upgrade installs).

It'll be exciting to see this tool when I finish it. It's the first real bit of programming I've ever
done at work (I think I'm up to ~200 lines of code, probably closer to 300 with the salt runner I wrote
to revert snapshots).

### moar saltstack
A big reason I was hired into this role was because I had experience doing using Saltstack. Today (20171215)
I spent a bunch of time setting up saltenvs with gitfs branches. Now I can safely/easily work on things in my
dev branch and not have to worry about messing things up for people doing real work (not that there are other
people doing things yet, but now when people do start working - I can experiment in peace).

Soon I'll be doing work with [salt-cloud](/posts/proxmox-cloud/), just 100% less Proxmox and 100% more vSphere.

Once I start getting the keys to that kingdom, I'll be writing overwrought autounattend.xml files (I don't think
I'll ever be able to adequately express how much I hate XML - it's the worst). But the nice thing about all of that,
I'll be able to spin up Windows Server 2012r2 boxes easily.

