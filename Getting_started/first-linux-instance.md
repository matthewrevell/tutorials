---
title: First linux instance
date: 2014-01-16 09:28 +01:00
category: Getting started
tags: linux
---
## Creating the Linux instance

* Go to the [instances view](https://portal.exoscale.ch/compute/instances) 
and click the "add" button.
* Enter the hostname of the instance and set its various properties (*Type*,
  *Disk Size*).
* At first you can leave the *Keypair* and *Security Group* fields set to
  their default values. They will be explained later.
* Click the "Create my instance" button.

Within a couple of seconds, your instance should appear in the instance list
with the "Starting" status then switch to "Running" status.

## Logging in to your Linux instance

First, make sure the default securitygroup allows SSH access: from the
instance list or detail view, click on the security group name ("default" if
you left the default values in the instance creation dialog).

On the security group view, click the "add" button to add a rule. Set the
following values:

* Type: inbound
* Source Type: cidr
* CIDR: 0.0.0.0/0
* Protocol: TCP
* Start Port: 22
* End Port: 22

Once you have allowed traffic on port 22, simply login with SSH as root user:

`ssh root@<instance IP>`

To find your instance password, look at the detailed instance page at the password section
Click on the button to open a popover showing the instance password.

Per-instance passwords can be avoided with *keypairs*. [Read more about
keypairs
here](/tutorial/ssh-keypairs/).
