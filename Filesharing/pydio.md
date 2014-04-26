---
title: Installing Pydio filesharing web application
date: 2014-04-26 19:28 +01:00
category: Filesharing
tags: ubuntu, pydio, filesharing
---

Pydio allows you to instantly turn any server into a powerful file sharing 
platform. Formerly known as AjaXplorer.

With a simple we interface and powerful mobile clients for Android
and iOS, Pydio, can manage securely data and data sharing between
users in the same organization or with external users.

Pydio can leverage a Filesystem, FTP or S3 backend making
it extremely extendable.

This guide installs Pydio on exoscale.

## Manual installation on Ubuntu 14.04 LTS

Add a the official Pydio repository to your instance:

    sudo vi /etc/sources.list.d/pydio.list

And copy the following contents:

    deb http://dl.ajaxplorer.info/repos/apt stable main
    deb-src http://dl.ajaxplorer.info/repos/apt stable main

Add the repository key to your instance:

    wget -O - http://dl.ajaxplorer.info/repos/charles@ajaxplorer.info.gpg.key | sudo apt-key add -

Update and then install Pydio:

    sudo apt-get update
    sudo apt-get install pydio

Push a default Apache2 configuration

    sudo cp /usr/share/doc/pydio/apache2.sample.conf /etc/apache2/sites-enabled/pydio.conf

Install the mcrypt library required for all security features of Pydio

    sudo apt-get install php5-mcrypt php5-sqlite
    sudo php5enmod mcrypt

And restart Apache

    sudo service apache2 restart

## Configure your Pydio instance

Connect to your freshly installed Pydio with http://<instance IP>/pydio

Pydio will check all your installation prerequisites and probably
throw some warnings because our default configuration does not run
over SSL. You can skip this by clicking on `click here to continue to Pydio`.

Pydio will then launch the setup wizard and ask about language,
admin name, username and password. 

Use the with database mode and then select sqlite and finish the wizard.


## Additional steps and TODOs

* install SSL
* Optimize the performance
* Use S3 backend


