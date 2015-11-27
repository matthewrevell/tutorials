---
title: "Compute Quick Start"
slug: "quick-start"
meta_desc: "Learn how to start your first Linux-based Instance on Exoscale Compute and connect via SSH, or how to deploy a Windows Instance and access it with RDP"
tags: "compute,quick-start"
---

Through this quick start you will create and access your first Instance.

## Setup

Before creating an Instance, if you plan to use a Linux based operative system,
we strongly encourage you to set up a SSH Keypair for added security and ease of
access. This article assumes you did so.
Here is a detailed tutorial on
[setting up SSH Keypairs](/documentation/compute/ssh-keypairs/).

You also need to have
[enough credit](/documentation/platform/quick-start/#minimum-credit) to start and run your
Instance.

## Create your Instance

Select the `INSTANCES` sub-menu from the `COMPUTE` menu on the left.
From the Instances list screen, clicking on the `ADD` button will bring you to
the Instance creation wizard.

### Choose a name for your Instance

You can freely choose a display name for your Instance. Only letters, digits and
hyphens are allowed. If you leave this field blank the Instance's name will be
the Instance's uuid.

A good naming convention for your Instances will help you work on them later on,
by filtering your Instances list. You can also change the display name of your
Instance later on on the Instance detail screen.

### Choose your template

From the templates list you can choose between different operating systems
either Linux or Windows based. For this tutorial you can choose either the
latest Ubuntu Linux or the latest Microsoft Windows Server distribution
(or any other OS if you feel comfortable to follow along!).

### Select your Instance type

You can choose between different service levels (combinations of CPU cores and
RAM memory). Some operating systems may be restricted from some choices because
of their own specifications.

Please note you will be able to modify this choice later on by scaling your
Instance up or down to another service level. If you wish you can read
[more about scaling here](/documentation/compute/instance-scaling/).

### Choose your Instance volume size

Similarly to service levels some disk sizes may be restricted by operating
system limitations.

Consider this a starting point for the size of your volume. You can adjust this
value later on from the Instance detail, scaling down and up to 400GB by steps
of 1 GB.
[More on volume scaling here](/documentation/compute/instance-scaling/).

### Keypair

As we said in the beginning we assume you will use an SSH Keypair if you choose
to start a Linux based Instance. Windows users can simply skip this point.

If instead you have chosen a Linux based Instance make sure you have loaded or
created an SSH key. If you haven't, please do so, you can read
[more about SSH Keypairs](/documentation/compute/ssh-keypairs/)
on this article.

Your machine will automatically be provisioned with the key you choose and you
will be able to access it via SSH straight away.

### Security Groups

Security Groups are firewalling rules. An empty security group called `default`
is already provided, but you may add more groups and even combine them.
A security group without defined rules behaves like this:

* All incoming traffic is blocked
* All outgoing traffic is allowed

At least one security group has to be associated with an Instance. The default
group is attributed in case you would choose no group. You can also add and
remove [Security Groups](/documentation/compute/security-groups/)
later on from the Instance details screen.

This being your first Instance simply choose the `default` security group, this
tutorial will assume you did so.

### Anti-Affinity Groups and User Data

Anti-Affinity Groups are a way to ensure your machines are created on different
hypervisors, and User Data is a way to automatically provision your machine
with additional software or settings (like installing Wordpress or set more
SSH keys).

Those may be considered advanced features, and you can skip them for your first
Instance creation. You may find more information on specific articles:

* [Anti-Affinity Groups](/documentation/compute/anti-affinity-groups/)
* [User Data and CloudInit](/documentation/compute/cloud-init/)

### Launch the Instance creation

You may now press the `CREATE` button. You will be redirected to the Instance
list where you'll see your new Instance listed. The VM will be marked as
`STOPPED` at first and you should see the label change to `STARTING` and
switch to `RUNNING` just after. Now you can log in to your new Compute Instance!

You can continue this guide depending on the OS you've chosen: log-in to a
Linux based Instance or log-in to a Windows based Instance.

## Log-in to a Linux-based Instance

If you followed along and you attributed the default empty Security Group to
your Instance, you should by now know that your new server can't be accessed
from outside. Having no rules defined, your security group denies all
incoming traffic.

You will access your machine via SSH. This means we have to make sure that
inbound traffic is allowed on port 22.

In the `COMPUTE` menu, choose the `FIREWALLING` sub-menu and then click on the
`default` Security Group. You will be presented with the group detail, where all
rules of the group are listed. You'll also see the list of Instances your group
is attached to.

Upper right you'll find commands to either choose a template (a shortcut to add
the most common rules) or add your own configured rule. We'll choose the SSH
template in this case, which will create a new rule with the following
underlying settings:

* Type: inbound
* Source Type: CIDR
* CIDR: 0.0.0.0/0
* Protocol: TCP
* Start Port: 22
* End Port: 22

If you followed all the steps you should now be able to simply log-in to your
Instance from a Linux command line with:

`ssh root@<instance IP>`

You can find your server's IP in the Instance list view and in the Instance
detail.

## Log-in to a Windows based Instance

Like we said before speaking about Security Groups, your Windows Instance will
not be accessible from outside until you define some rules for his security
group.

In the `COMPUTE` menu, choose the `FIREWALLING` sub-menu and then click on the
`default` Security Group. You will be presented with the group detail, where all
rules of the group are listed. You'll also see the list of Instances your group
is attached to.

Upper right you'll find commands to either choose a template (a shortcut to add
the most common rules) or add your own configured rule. We'll choose the
<abbr title="Remote Desktop Protocol">RDP</abbr> template in this case, which
will create a new rule with the following underlying settings:

* Type: inbound
* Source Type: CIDR
* CIDR: 0.0.0.0/0
* Protocol: TCP
* Start Port: 3389
* End Port: 3389

Once you have allowed traffic on port 3389, simply point your remote desktop
client to the public IP address of your Instance using the user name
"administrator". You will find your Instance password in the Instance detail
page.

**Please note the following**:

* If you don't see the password in the Instance detail, please allow a short
lapse of time for the password to bubble up to the interface (generally less
than 60 seconds).
* **The password will be available through the interface for a very short
period** (some hours), so please take good note of it. **You may in any case
reset it when you wish** from the same Instance detail.
