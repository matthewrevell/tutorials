---
title: Turning an exoscale Ubuntu template in a Lamp server
date: 2014-07-03 09:28 +01:00
category: Web hosting
tags: web, hosting, ubuntu, lamp, apache, mysql, php
---

This tutorial explains how to convert a standard Ubuntu 12.04 LTS 
or 14.04 TLS template from the official exoscale bare instance 
list to a full working LAMP server.

We are going to rely on the included Ubuntu way of doing this 
task using the Tast Selector that is usually run at server
installation time.

## Install Task Selector

install the selector

    sudo apt-get install tasksel

## Install LAMP

Now to install LAMP, type the taskel command in terminal

    sudo  tasksel

From the list select the task named `LAMP Server`

During the installation you will be asked to insert the mysql root password.

## Check your configuration

To check the configuration is running, we are going to
make a simple PHP page.

    sudo vi /var/www/phpinfo.php

and insert the following content:

    <?php
    
    phpinfo()
    
    ?>

Now restart the Apache server

    sudo service apache2 restart

and check by pointing your browser at `http://<ip>/phpinfo.php`

NOTE: you need to have your security group opened inbound 
for port 80 for this to work.

## Next steps

you are now ready to host your LAMP project. For additional
management ease, you might want to install a mysql
tool like phpmyadmin

    sudo apt-get install phpmyadmin

the service should be running at `http://<ip>/phpmyadmin`



