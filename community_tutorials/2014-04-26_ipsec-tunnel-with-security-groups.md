---
title: "Using a IPSec tunnel with security groups"
slug: "using-a-ipsec-tunnel-with-security-groups"
date: 2014-04-26 09:52 +01:00
tags: "security"
---

Ipsec is a native component of the Linux kernel since 2.6, so this is available
in all exoscale Linux flavors.
IPSec will enable a layer 3 IP connection which will have the benefit to
be encrypted between your instances.

## Prerequisites

for this tutorial you need 2 instances. For this tutorial, Ubuntu 14.04
is assumed, but a similar configuration is possible on other versions.

If your instances are in the same security group, then this SG should allow
incoming traffic for 3 UDP ports. The UDP ports are:

* 4500 (IPsec/UDP)
* 500 (IKE) 
* 1701 (L2TP)

On each instance, install the tools to manage the IPSec connection:

    sudo apt-get install ipsec-tools


