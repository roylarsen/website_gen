---
type: "post"
draft: false
author: "roy."
date: "2018-01-09T06:30:15-05:00"
description: "quick intro to saltstack gitfs"
keywords: ["saltstack", "gitfs", "git"]
topics: ["gitfs"]
tags: ["gitfs", "saltstack", "git"]
title: "saltstack gitfs"
---

# Saltstack gitfs

This is mostly notes for myself, but I wanted to write up an easy reference.

### basics
This is to get a basic setup with a map from saltenv to git branch.

This requires some basic knowlege of saltstack concepts like pillar, salt environments.

My setup is pretty simplistic, I have two main branches: master and dev. The branches are
present in the state and pillar repos. Top files are managed per-repo, per-branch (there are
probably better ways of handling it, but this is fine for right now).

Here's the link to the saltstack docs on gitfs: https://docs.saltstack.com/en/latest/topics/tutorials/gitfs.html
For my notes/usage, I use pygit2 and ssh key auth.

First things first, you need to add your git host key to your master's known hosts:
https://docs.saltstack.com/en/latest/topics/tutorials/gitfs.html#adding-the-ssh-host-key-to-the-known-hosts-file

At this point you've created your repos and the branches that you want ot work with. My mapping is pretty simplistic since
I just use two branches:</br>
**branch -> saltenv**</br>
master -> base</br>
dev -> dev</br>


## actual configuration stuff

So, there are three things that you really need to configure before we're off to the races.
These are all in your /etc/salt/master

Configuring your backend:
```yaml
fileserver_backend:
  - git
  - roots
```

Configuring your state repo:
```yaml
gitfs_remotes:
  - ssh://<state git repo address>.git:
    - pubkey: /root/.ssh/id_rsa.pub
    - privkey: /root/.ssh/id_rsa
    - saltenv:
      - dev:
        - ref: dev
      - base:
        - ref: master
```

In the above config, you configure your saltenv's per remote. I'm assuming you you do this over multiple
backend definitions and use saltstack's merging function. I haven't played around with it, so, don't
quote me on any of this.

Pillar repo config:

```yaml
ext_pillar:
  - git:
    - master ssh://<pillar git repo address>.git:
      - env: base
      - privkey: /root/.ssh/id_rsa
      - pubkey: /root/.ssh/id_rsa.pub
    - dev ssh://<pillar repo address>.git:
      - privkey: /root/.ssh/id_rsa
      - pubkey: /root/.ssh/id_rsa.pub
```

If you've noticed, the pillar definition is a tad different from the state definition. Where in the state, you could define the remote once and then map the
env to the branch/ref.

For the pillar definition, you need to define the branch/ref and the repo in the same lone, then map the saltenv under it (if you want to map
saltenvs to branches that are named differently - master/base for me)


## conclusion

That's pretty much it. Restart the salt-master and you should have it all set up:

```
sudo salt '*' state.show_top
<id>:
    ----------
    base:
       - state
       - state
<id2>:
    ----------
    dev:
       - state
       - state
```
(I removed my company specific info from that output)


I think my next post will be about my mhqa tool and the Saltstack API
