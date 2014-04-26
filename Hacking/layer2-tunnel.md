---
title: Build a layer 2 network tunnel between instances
date: 2014-04-25 09:28 +01:00
category: Hacking
tags: bridge, network
---

Having a private network over an layer 3 network on a cloud can
enable you to build advanced configurations and testing 
scenarios.
For this we will setup a layer 2 tunnel, with built in features of the 
linux kernel. This setup is run over Ubuntu 14.04 but should run on
any recent 2.6.29+ kernel.

## Prerequisites

launch 2 instances (Ubuntu) in exoscale on the same zone.

## Install the bridge utilities in your instance

    apt-get install bridge-utils

Load the corresponding kernel modules

    lsmod | grep vhost
    modprobe vhost_net
        vhost_net              18063  0
        vhost                  29009  1 vhost_net
        macvtap                18255  1 vhost_net

Start a bridge interface:

    brctl addbr br0


