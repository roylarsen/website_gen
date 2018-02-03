+++
description = "salt-cloud and proxmox"
keywords = [
  "saltstack",
  "proxmox",
  "virtualization",
  "salt-cloud",
  "python",
]
topics = [
  "salt-cloud and proxmox",
]
title = "proxmox cloud"
type = "post"
draft = false
date = "2017-06-17T20:41:45-05:00"
author = "roy."
tags = [
  "virtualization",
  "cloud",
  "saltstack",
  "python",
  "salt-cloud",
]

+++

# Proxmox + salt-cloud = <kinda3

So, I've been working on getting salt-cloud playing nicely with Proxmox. The biggest issue I've seen so far is within salt-cloud's proxmox driver is more of a limitation within proxmox itself.

Unless you statically assign you IP address, you're not going to see your IP address in the GUI or in the standard salt-cloud query to your hypervisor. The proxmox driver also allows your to query DNS for
the IP address of your machine.

Neither of those really work for me at home, since I like to have my VMs get an address from DHCP, then convert that to a static reservation when I'm sure that VM will stick around.

My prefered solution would be to pull the IP address straight from the hypervisor, which would make my life so much easier. That's doable when you're using the VMWare/ESXi driver when the agent is
installed, I figure that there should be a way of handling that within Proxmox.

Enter the [QEMU Guest Agent](https://pve.proxmox.com/wiki/Qemu-guest-agent)

As it turns out, installing this is pretty easy. You install the service within your target VM, set the Agent option under hardware in the PVE web GUI to 'On', and then you reboot the VM.

Boom. Easy.

At this point you have better access to the guest through the API (either through the web or pvesh command on the host).

Now I'm not going to do into details about how long it took me to dig through the PVE API, but I found what I want.

[PVE APIv2 Docs!](http://pve.proxmox.com/pve-docs/api-viewer/index.html)

Dig down to nodes/{node}/qemu/{vmid}/agent - That's the API commands for querying the guest agent directly. Not a lot is implemented, however, network-get-interfaces looks promising.

(spoiler: that's what we want)

So playing around with pvesh on my VM host gets me to this command:

```
pvesh create nodes/technodrome/qemu/101/agent -command network-get-interfaces

(yes, my main node name is a reference to TMNT)
```

That returns a JSON response with all the network information on the interfaces on your guest:

```
pvesh create nodes/technodrome/qemu/101/agent -command network-get-interfaces
200 OK
{
  "result" : [
    {
      "hardware-address" : "00:00:00:00:00:00",
      "ip-addresses" : [
        {
          "ip-address" : "127.0.0.1",
          "ip-address-type" : "ipv4",
          "prefix" : 8
        },
        {
          "ip-address" : "::1",
          "ip-address-type" : "ipv6",
          "prefix" : 128
        }
      ],
      "name" : "lo"
    },
    {
      "hardware-address" : "<MAC address>",
      "ip-addresses" : [
        {
          "ip-address" : "<ipv4 address",
          "ip-address-type" : "ipv4",
          "prefix" : 24
        },
        {
          "ip-address" : "<ipv6 address>",
          "ip-address-type" : "ipv6",
          "prefix" : 64
        }
      ],
      "name" : "eth0"
    }
  ]
}

sanitised so you can't see my internal numbering scheme
```

### OH SHIT.

So, now the biggest obstacles I have are as follow:
  
- Getting what I found into: https://github.com/saltstack/salt/blob/develop/salt/cloud/clouds/proxmox.py
  - Need to create a function `wait_for_ip_address()`
  - Need to add some options to the Proxmox profile to handle being able to use DHCP
- Support having multiple interfaces that aren't lo
- Get patch accepted into salt-cloud
  - (Or I just keep it for myself)

### UPDATE, NERDS

So, as I've been working on the code for my patch to the proxmox provider, I was running some other solutions to my problem in my head.

Enter: [cloud-init](https://cloudinit.readthedocs.io/en/latest/)

What I've been realizing that I can do is use cloud-init to run a python scipt to set the hostname of the box.

- Get the VMID of the VM
  - possibly use the MAC address as my unique identifier?
- Use VMID to call /qemu/{VMID}/status/current
- Potentially use a shell script instead of a Python script to lower the amount of dependencies used

### Update 2, nerds

I think at this point I am going to eventually end up with the cloud-init solution. I think it's the easiest to support and it's [really](http://cloudinit.readthedocs.io/en/latest/topics/datasources/digitalocean.html) [interesting](http://cloudinit.readthedocs.io/en/latest/topics/datasources/ec2.html) [what](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) [other](http://cloudinit.readthedocs.io/en/latest/topics/datasources/azure.html)
clouds do.

I'm going to write my patch for the Proxmox provider, but probably just keep that fork as an example in my github account as something to show off. It'll be a fun exercise, but I don't see it as supportable
beyond a stopgap until I get cloud-init working.

Plus, I'm pretty sure cloud-init will be a better solution for when I want to experiment with building LXC containers.

### Update 3, nerds

So, I've been neglecting my blog because I'm a dummy.

I've actually written my patch for the proxmox provider and have been using it in my homelab for a while at this point. 

It was pretty cool, I got to learn how to do list comprehensions and now I can easily spin up new VMs in my lab!
