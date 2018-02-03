+++
keywords = [
  "devops",
  "IoC",
  "hashicorp",
  "Digital Ocean",

]
tags = [
  "devops",
  "IoC",
  "hashicorp",
  "Digital Ocean",
]
date = "2017-06-29T22:47:03-04:00"
type = "post"
draft = false
author = "roy."
description = "A Poorly Written Introduction to Terraform"
topics = [
  "Terraform Intro",
]
title = "terraform"

+++

# A Poorly Written Intro to Terraform

So, I'm probably late to this party. Very late.

I still have a lot to play around with, but after creating and destroying some Digital Ocean droplets, I'm already starting to see the power and utility of this tool (it's fun
when those things are easily seen right away).

I'm a pretty firm believer that [Hashicorp](https://www.hashicorp.com/) deserves their reputation in the Devops space. Everyone uses at least some of their tools.
 
 * Vagrant
 * Packer
 * Consul
 * Vault
 * Nomad
 * Terraform


So, back to the topic this is about: [Terraform](https://www.terraform.io) (here out referred to as, TF)

TF, in a basic sense, is pretty simple. It uses a pretty basic DSL for it's config files and just the Terraform application.

```
provider "digitalocean" {
  token = "{api_token}"
}

resource "digitalocean_droplet" "resource-name" {
  image = "ubuntu-17-04-x64"
  region = "nyc2"
  name = "droplet-name"
  size = "512mb"
}
```

That's about as basic a Digital Ocean config as you can get without having to declare anything extra on the cli.

At this point, all you need to do is 
```
terraform plan
```

```
terraform apply
```
in your shell, and you should have an email in you inbox shortly telling you about your fancy new droplet named 'droplet-name'.

That's seriously it. Literally all you need.

Now, the complexity of TF comes from your specific infrastructure. But the building blocks are all basically the same.

Obviously I haven't gotten far in this, but I'm already seeing the potential in what I want to do with it. The biggest thing I see to do with this is using it to
build a travel homelab. Yeah, it'll cost money, but I'll be able to play with things while I'm away easily.

That's about it for now. I'll probably have follow ups to this when I get further in learning TF.
