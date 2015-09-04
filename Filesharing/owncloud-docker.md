---
title: Install Owncloud in 1 minute with Docker
date: 2015-09-01 19:28 +01:00
category: Filesharing
tags: owncloud, docker, coreos, filesharing
---

We have already written about Owncloud and a traditional
method to install it on a [Linux Ubuntu instance][previous]
This guide will show how to get a quickly running instance
by leveraging Docker.

## Start your Owncloud powered by Docker

1. Log on to [Exoscale Portal][portal]
2. Go to the [ADD][add] instance 1 page wizard
3. Input the name e.g. __owncloud__
4. Select the CoreOS template
5. Select the size: small and 10 GB are enough
6. Take a security group which has TCP port 80 and 443 open
7. Paste the following in the userdata box

    ```
    #cloud-config
    hostname: owncloud
    
    write_files:
      - path: /var/owncloud/config/empty
        permissions: 0644
        owner: root
      - path: /var/owncloud/data/empty
        permissions: 0644
        owner: root

    coreos:
      units:
        - name: docker.service
          command: start
        - name: ow.service
          command: start
          content: |
            [Unit]
            After=docker.service
            Requires=docker.service
            Description=starts Owncloud

            [Service]
            TimeoutStartSec=0
            ExecStartPre=/usr/bin/docker pull owncloud
            ExecStart=/usr/bin/docker run -d -p 80:80 p 443:443 -v /var/owncloud/config:/var/www/html/config -v /var/owncloud/data:/var/www/html/data owncloud
      update:
        reboot-strategy: reboot
    ```

8. Launch your instance
9. Wait 1 minute and then open the IP address in your browser

You are then faced with the usual configuration wizard from Owncloud
to create an admin user and finish setup.

You can add unlimited storage to this Owncloud installation
by linking to [Exoscale Object Storage][S3]

## What happens

Here we are creating a [CoreOS][coreos] compute instance and injecting in it
the above userdata. This userdata gets interpreted as a [Cloudinit][cloudinit]
template which give the instance:

* its name
* tells it to start Docker on boot
* Create a new service called `ow`
* Defines this service as being a Docker container
* Pulls the official Owncloud image from [Docker Hub]
* And runs it exposing port 80

[hub]: https://hub.docker.com/_/owncloud/
[previous]: /tutorial/install-owncloud-on-ubuntu-1404/
[coreos]: https://coreos.com/
[cloudinit]: https://coreos.com/os/docs/latest/cloud-config.html
[add]: https://portal.exoscale.ch/compute/instances/add
[portal]: https://portal.exoscale.ch
[S3]: /tutorial/extend-owncloud-with-s3-compatible-storage/
