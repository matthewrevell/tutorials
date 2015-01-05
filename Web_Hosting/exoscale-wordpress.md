---
title: Installing Wordpress with CloudInit
date: 2014-01-16 09:28 +01:00
category: Web hosting
tags: linux, cloudinit, wordpress
---
___Originaly published on our blog___

Each programming language has its "Hello World" tutorial. In the cloud 
provider business, the benchmarking is often done on a simple application deployment. 
[Wordpress](http://wordpress.org/) is such an application used to guide users as a first hands on experience.
We have created our own Wordpress deployment example, but done so in a slightly more 
disruptive way. 

Whereas traditionnal Virtual Private Servers tutorials lists pages of 
scripts and console windows, ours is **just 15 lines**.
The trick is to let Puppet, a configuration management tool, do the job. This example will not 
deploy a clone of a Wordpress installation, but a fresh and latest copy of the famous blogging engine. 


## Principles

This cookbook will leverage userdata extensibility of our Apache CloudStack powered Open Cloud offering and the features of cloud-init to 
bootstrap a **fully fonctionnal Wordpress install which IS NOT a clone**.

* Userdata will be filled with a script
* Script will be called on first boot by cloud-init
* A puppet repository will be pulled on the instance
* The script will launch Puppet and apply the manifest and eventually after PHP, NGINX and MYSQL installation pull a fresh copy of Wordpress and install it.

Keep it mind that it is possible to go much further in automation deployment. 

Anyway, here is the usage guide from the README section of the project or see for yourself with this simple article: https://github.com/exoscale/exoscale-wordpress/ :


## Usage guide

### Start an instance

Launch a new Ubuntu 12.04 LTS instance with the service offering you wish. Insert it in a security group or with firewall rules which enable port 80/http.

### User Data with Cloud-init

In the User Data tab, input the script below:

    #!/bin/sh
    set -e -x

    apt-get --yes --quiet update
    apt-get --yes --quiet install git puppet-common

    #
    # Fetch puppet configuration from public git repository.
    #

    mv /etc/puppet /etc/puppet.orig
    git clone https://github.com/exoscale/exoscale-wordpress.git /etc/puppet

    #
    # Run puppet.
    #

    puppet apply /etc/puppet/manifests/init.pp

### Start using your fresh  Wordpress

Point your browser to your instance public IP address and you should be asked for an admin email and password. 

