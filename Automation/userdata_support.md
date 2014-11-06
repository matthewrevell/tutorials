---
title: Bootstrap your instances with CloudInit and User Data 
date: 2014-01-16 09:28 +01:00
category: Automation
tags: linux, userdata
---
___Originaly published on our blog___

User-data is now a commonly used format to make an instance do stuff while it boots. It consist of a file that is served to a booting instance and includes actions and blobs of data. It can be passed to the instance in either raw text or separate, optionally compressed part files.

We have made it pretty easy to manage and edit your user-data on exoscale with a new tab in our instance creation box:

![user-data form](userdata_support.png)

User-data is tightly linked to cloud-init, a package which handles the post boot process of an instance. Exoscale already uses cloud-init in order to set up your SSH keys and to apply a matching hostname toe the instance.


### make sure your instance is up to date on boot up

Would it not be nice to make sure that the new instance you create is up to date with the very latest packages and fixes for the official repositories ? This is possible with cloud-init and a simple YAML configuration statement. Cloud-init understands several pragmas: upstart job, script, part handler, include file, and config. With the later option, you can access all cloud-init syntax by using a simple #cloud-config in your user-data file attached to the instance.
On an Ubuntu machine this will look like:

    #cloud-config
    apt_upgrade: true

When the machine boots, it will call cloud-init, which in turn will query exoscale CloudStack user-data and start reading the configuration. Because the "apt_upgrade" is defined the cloud-config handler, it will call the code attached and eventually make an "apt-get upgrade" call.

### run a script to create a user and group

As explain in the first usage example, a script handler also exist in cloud-init. Therefore, anything after the standard " #!" will be interpreted as a user  script.

    #!/bin/sh
    groupadd mygroup
    useradd –D user1 –G mygroup



__Note:__  _a cloud-config class exists for user and group management directly. Just use the groups and users definitions:_

    #cloud-config
    groups:
        - mygroup: [user1, user2]
    users:
        - name: user1
        - primary-group: user1
        - groups: mygroup

### bootstrap your Puppet configuration management system

If you dive in the cloud-config handler, you will also find necessary method for inserting your instance right into your configuration management system. One of the most popular ones being [Puppet](https://puppetlabs.com/community/overview/), a simple definition like below will configure your instance as a puppet agent and puppet will run against the master specified. Therefore, you can achieve packages installation, user creation and more in a reusable fashion.

    #cloud-config
    puppet: 
    conf:
        agent:
        server: puppetmaster.mydomain.org
        certname:Nodename.mydomain.org

### Conclusion

To get further, you should also leverage TAGS, but this will be another article.

There are almost endless possibilities and integration ideas with cloud init, and the [official documentation](https://cloudinit.readthedocs.org/en/latest/) is a good place to start. There are lots of [examples](https://cloudinit.readthedocs.org/en/latest/topics/examples.html) ranging from mount points to integration with [Chef](http://docs.opscode.com/index.html).

