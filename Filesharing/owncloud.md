---
title: Install Owncloud on Ubuntu 14.04
date: 2014-04-26 19:28 +01:00
category: Filesharing
tags: owncloud, ubuntu, filesharing
---

Owncloud is a PHP application that can enable you to launch 
your custom Dropbox like installation. It supports plugin for 
integration and extension as well as mobile application
and a desktop synchronisation client for keeping your
files in sync between smartphone, web and laptop.

This tutorial will guide you to a basic installation
of Owncloud on a linux Ubuntu 14.04 LTS intance on exoscale
cloud.

## First steps

launch an instance if you do not have one already. This instance 
should belong to a security group which has port 22 and 80 at minimum
opened. Port 443 should also be opened inbound for encryption support.

## Add Owncloud repository

update your repository lists:

    sudo sh -c "echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/xUbuntu_14.04/ /' >> /etc/apt/sources.list.d/owncloud.list"

and trust the repository key for this installation and future ones:

    wget http://download.opensuse.org/repositories/isv:ownCloud:community/xUbuntu_14.04/Release.key
    sudo apt-key add - < Release.key 

Update your package cache:

    sudo apt-get update

your are now ready to install the application

## Installation

    sudo apt-get install owncloud php5-sqlite
    sudo php5enmod mcrypt

then create an apache configuraiton file

    sudo vi /etc/apache2/sites-enabled/owncloud.conf

and copy the following base configuration

    alias   /owncloud       /var/www/owncloud
    <Directory "/var/www/owncloud">
        Options FollowSymLinks
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>

Finally restart apache2

    service apache2 restart

## Finalize the configuration online

connect with your browser on http://<instance IP>/owncloud

you will be able to create the adminstrator credentials and
finish the configuration.


