---
title: "Vagrant and Exoscale"
slug: "vagrant-exoscale"
date: 2015-11-03 20:00 +01:00
authors: nicolasbrechet
tags: "vagrant"
---

## What we'll cover

Through this tutorial, you will learn how to install and configure Vagrant.
Then you will deploy an Instance on Exoscale, from the comfort of your
terminal. We'll configure a web server from Vagrant.
Finally, you will deploy and provision several Instances, without added
difficulty.

As we will only deploy the smallest Instances possible, and we will destroy the
Instances when we're done, following this tutorial should not cost you more
than a few Swiss Francs.

## About Vagrant

Vagrant is a tool for building complete development environments. With an
easy-to-use workflow and focus on automation, Vagrant lowers development
environment setup time, increases development/production parity, and makes
the "works on my machine" excuse a relic of the past. (_from_ [vagrantup.com](https://www.vagrantup.com/about.html))

Vagrant uses a 'box' as a base for your servers. You can download an existing
box, and use it for several configurations. Or you can make your own, this is
an advanced topic not covered here.

These boxes are packaged Vagrant environments. Usually, they are to be used
with a desktop virtualization application such as VirtualBox, VMware Fusion or
VMware Workstation.

Through the use of plugins, it is possible to use Cloud Providers instead of
local hypervisors. This is what we'll do in this guide, using Exoscale to
deploy our servers.

**Why should I use Vagrant?**
Vagrant makes it very easy to deploy a server, provisioning it with a
configuration management tool like Puppet or Chef, or simply with scripts.
It makes it easy to share the configuration of a server, as you only share the
Vagrantfile.

**I don't have VirtualBox or VMware Fusion**
That's OK, you don't need it for this tutorial. Your server will be deployed
directly on Exoscale Compute.

## Installing Vagrant

Go to the [Vagrant download page](https://www.vagrantup.com/downloads.html) and
retrieve the package for your Operating System.

**Windows**

* Double-click on the .msi package to install

**Debian/Ubuntu**

* Use the `dpkg` command to install the package
* Example: `dpgk --install vagrant_1.7.4_x86_64.deb`

**CentOS/RHEL**

* Use the `rpm` command to install the package
* Example: `rpm -Uvh vagrant_1.7.4_x86_64.rpm`

**OS X**

* Open the DMG file
* Double-click on the .pkg installer


## Verify your installation

Go to your terminal and type `vagrant` to check your installation.
You should see the help, showing you a list of commands. If not, verify your
installation.

## Adding a box

As we said earlier, Vagrant needs boxes to know what kind of server it needs to
start. Lucky for us, Exoscale provides a set of boxes matching the
[templates](https://www.exoscale.ch/open-cloud/templates/) available on
Exoscale Open Compute.

We will work with Ubuntu Linux, and we don't need a lot of disk for this
tutorial. The minimum amount of disk is 10GB on Exoscale. So in the list of
template, we can find the link to an Ubuntu Linux box, with 10GB disk.

Let add the `Linux Ubuntu 15.04 64-bit 10G Disk` box to our installation of
Vagrant.

First, download the box: either click on the link from your browser, or copy
the link and use `wget` to download it.

*The link will change every time the box is updated.*

```
wget "https://github.com/exoscale/vagrant-exoscale-boxes/raw/master/exoscale-boxes/Linux-Ubuntu-15.04-64-bit-10G-Disk-(2015-04-22-c2595b).box"
```

Once the box is downloaded, you can 'add' it, to let Vagrant know it can use a
box. As we're adding a box from a local file, we have to name it.
The syntax is `vagrant box add <box location> --name <box name>`

```
vagrant box add "Linux-Ubuntu-15.04-64-bit-10G-Disk-(2015-04-22-c2595b).box" --name 'exoscale-ubuntu-1504-10GB'
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'exoscale-ubuntu-1504-10GB' (v0) for provider:
  box: Unpacking necessary files from: file:///Users/exoscale/Linux-Ubuntu-15.04-64-bit-10G-Disk-(2015-04-22-c2595b).box
==> box: Successfully added box 'exoscale-ubuntu-1504-10GB' (v0) for 'cloudstack'!
```

As you can see in the response, the box was successfully added and Vagrant
detected the provider: "Cloudstack".
This is normal, [Cloudstack](https://cloudstack.apache.org) is the technology
used by Exoscale to provide the Open Compute service.

We now have an Exoscale Vagrant box installed. Feel free to install more if you
want: simply repeat the steps from before, for each box you want to add. Don't
forget to change the name of the box!

## Adding the Cloudstack plugin

In order to "talk" to Exoscale Open Compute, Vagrant needs the
[vagrant-cloudstack plugin](https://github.com/schubergphilis/vagrant-cloudstack)

Installing Vagrant plugins is very easy: `vagrant plugin add <plugin name>`

Let's install our Cloudstack plugin

## Deploying an Instance

Deploying an Instance with Vagrant requires 3 steps:
1. Creating a configuration file for Vagrant: the Vagrantfile
2. Editing the Vagrantfile to fit our needs
3. Deploy the Instance

We will deploy a simple Web server.
Let's create a minimal Vagrantfile in a directory for this tutorial:

`vagrant init exoscale-ubuntu-1504-10GB --minimal`

The `--minimal` option removes all the comments usually present in a
Vagrantfile. We don't need those in this tutorial.

Our Vagrantfile looks like this:

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
end
```



## Deploying multiple Instances

## Going further
