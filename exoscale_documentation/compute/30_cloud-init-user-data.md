---
title: "Cloud-Init and User-Data"
slug: "cloud-init"
meta_desc: "Cloud-Init handles early initialization of your Exoscale Instance, letting you configure it on first boot using User-Data or scripts directly from our interface"
tags: "compute"
---

User Data is a way to automatically provision your machine with additional
software or settings.
You can provide a set of commands (ie. a script) or enter cloud-config
information as YAML.
It is important to understand that the User Data provided will be used only
when the machine is created.

## Adding User Data
When you're creating a new Instance, the wizard lets you enter User Data
at the bottom of the form.

You can find everything that can be done with User Data and Cloud-Init in the
[documentation](https://cloudinit.readthedocs.org/en/latest/index.html)

## Example: script

Let's install `screen` and `htop` on an Ubuntu Instance using Apt after having 
upgraded all packages on an Debian/Ubuntu system.   

Add the following to the `User Data` field to install the `screen` package:

```
#cloud-config
runcmd:
    - apt-get --yes upgrade
    - apt-get --yes install screen htop
```
*Note:* Cloud-Init has pre-built directives for many items. Therefore,
the previous scripts launched as command lines could be achieved with:

```
#cloud-config
package_upgrade: true
packages:
  - screen
  - htop
```

## Example: cloud-config YAML

Let's set our locale to en_US.UTF-8 using cloud-config YAML.
Add the following YAML in the User Data:

```
#cloud-config
locale: "en_US.UTF-8"
```

## Example: installing Wordpress

See our [Wordpress tutorial](/tutorial/installing-wordpress-with-cloudinit/)

## Querying the user data and meta data from the instance

`User Data` and `Meta Data` can be retrieved from an instance to 
integrate in scripts for example or configuration management tools.

First identify the IP address of the meta data server:

* for Debian/Ubuntu:
`cat /var/lib/dhcp/dhclient.eth0.leases | grep dhcp-server-identifier | tail -1`

* for CentOS based:
`cat /var/lib/dhclient/dhclient--eth0.lease | grep dhcp-server-identifier | tail -1`

Then you can access:

* User Data (the cloud-config contents)
`curl http://<IP ADDRESS>/latest/userdata`

* Meta Data, such as instance size or IP address
```
curl http://<IP ADDRESS>/latest/metadata
curl http://<IP ADDRESS>/latest/metadata/public-ipv4
```
