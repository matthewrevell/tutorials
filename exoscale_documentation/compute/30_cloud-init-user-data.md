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

Let's install `screen` on an Ubuntu Instance using Apt.

Add the following to the `User Data` field to install the `screen` package:

```
apt-get --yes install screen
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
