---
title: "Installing Wordpress with SlipStream"
slug: "installing-wordpress-with-slipstream"
date: 2016-01-18 09:28 +01:00
tags: "slipstream,user-data,wordpress"
---
___Originaly published on our blog___

Deploying WordPress using ExoScale and SlipStream requires only a few steps.


## Usage guide

### Configure the WordPress deployment on the ExoScale cloud service 

Launch the WordPress "Execute Deployment" dialog in SlipStream by simply clicking on the WordPress icon.

Then choose the max number of VMs to be deployed under "Multiplicity"

Under "Cloud service" choose "exoscale-ch-gva" for the target cloud service. Ensure that under your SlipStream profile you have entered your ExoScale API and Secret Key credentials.
These can be edited by selecting your user profile, then -> exoscale-ch-gva -> Edit

Choose an admin-email and blog title under "admin title" and "wordpress_title"

### Initiate provisioning process

To  deploy WordPress with the above configuration, simply click on the "Run" button in SlipStream.

Once WordPress has been deployed and is available, and "is READY" message appears at the top of your screen, and a large app link will appear at the top right of the screen.

### Start using your fresh  Wordpress

Simply click on the app link at the top of the right of your screen to run your WordPress instance.

The WordPress application is deployed with machine-generated maintenance passwords

