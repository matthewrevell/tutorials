---
title: First Windows instance
date: 2014-01-16 09:28 +01:00
category: Getting started
tags: windows
---
### Creating your instance

* Go to the [instances view](/instances) and click the "add" button.
* Enter the hostname of the instance and set its various properties (*Type*,
  *Disk Size*).
* Select a windows instance type. Note that Windows instances require at least
  50GB of disk and 1GB of RAM.
* Click the "Create my instance" button.

### Logging in to your instance

First, make sure the default securitygroup allows <abbr title="Remote Desktop
Protocol">RDP</abbr> access: from the instance list or detail view, click
on the security group name ("default" if you left the default values in the
instance creation dialog).

On the security group view, click the "add" button to add a rule. Set the
following values:

* Type: inbound
* Source Type: cidr
* CIDR: 0.0.0.0/0
* Protocol: TCP
* Start Port: 3389
* End Port: 3389

Once you have allowed traffic on port 22, simply point your remote desktop
client to the public IP address of your instance using the username
"administrator".

To find your instance password, look at the [jobs status](/jobs): "instance
creation" jobs have an additional button next to their status. Click on the
button to open a popover showing the instance password.
