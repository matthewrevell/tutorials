---
title: "Vagrant and Exoscale"
slug: "vagrant-exoscale"
meta-desc: "With Vagrant, you can easily deploy and provision virtual machines. Discover how to use Vagrant with Exoscale, and deploy multiple Instances in a few seconds."
date: 2015-11-06 12:00 +0100
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
the "works on my machine" excuse a relic of the past.
(_from_ [vagrantup.com](https://www.vagrantup.com/about.html))

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

Let's add an Ubuntu Linux box.
We will choose `Linux Ubuntu 15.04 64-bit 10G Disk` but you can choose any
Ubuntu box.

First, download the box: either click on the link from your browser, or copy
the link and use `wget` to download it.

*The link will change every time the box is updated.*

```
wget "https://github.com/exoscale/vagrant-exoscale-boxes/raw/master/exoscale-boxes/Linux-Ubuntu-15.04-64-bit-10G-Disk-(2015-04-22-c2595b).box"
```

Once the file is downloaded, you can 'add' it, to let Vagrant know it can use it
as box. As we're adding a box from a local file, we have to name it.
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

Installing Vagrant plugins is very easy: `vagrant plugin install <plugin name>`

Let's install our Cloudstack plugin:

```
$ vagrant plugin install vagrant-cloudstack --plugin-version 1.0.0
Installing the 'vagrant-cloudstack' plugin. This can take a few minutes...
Installed the plugin 'vagrant-cloudstack (1.0.0)'!
```

The plugin version here is 1.0.0 and might change as the plugin is being
developed. To install the latest version, simply omit the`--plugin-version`
parameter.

This plugin allows you to use Exoscale's boxes and deploy Instances in
Exoscale Compute.

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

Right now, all Vagrant knows is what box to use, but we need to specify the
provider we want to use.

Edit the Vagrantfile so it looks like this:

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
  config.vm.provider :cloudstack do |cloudstack, override|
    # provider's details
  end
end
```

Next, we want to authenticate so we will provide our API key and secret.

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
  config.vm.provider :cloudstack do |cloudstack, override|
    cloudstack.api_key    = "AAAA_YOUR_API_KEY"
    cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
  end
end
```

The box we chose only contains OS and Disk information. We need to add CPU
and memory. This is the Service Offering Name: Micro, Tiny, Small, Medium,
Large, Extra-large, Huge.

Choose one service offering name and update your Vagrantfile:

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
  config.vm.provider :cloudstack do |cloudstack, override|
    cloudstack.api_key    = "AAAA_YOUR_API_KEY"
    cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
    cloudstack.service_offering_name = "Micro"
  end
end
```

In 8 lines of configuration, we are telling Vagrant to deploy an Ubuntu Linux
Micro instance (1cpu with 512MB ram) with 10GB disk, on Exoscale Compute.
We're missing the Security Group(s) for this Instance and the SSH keypair we
want to use when connecting to the Instance. That's two more lines of configuration.

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
  config.vm.provider :cloudstack do |cloudstack, override|
    cloudstack.api_key    = "AAAA_YOUR_API_KEY"
    cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
    cloudstack.service_offering_name = "Micro"
    cloudstack.security_group_names = ['default']
    cloudstack.keypair = "name_of_your_keypair"
  end
end
```

As we've named the SSH keypair we want to use, we need to point Vagrant to our
private key.

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
  config.vm.provider :cloudstack do |cloudstack, override|
    cloudstack.api_key    = "AAAA_YOUR_API_KEY"
    cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
    cloudstack.service_offering_name = "Micro"
    cloudstack.security_group_names = ['default']
    cloudstack.keypair = "name_of_your_keypair"
    cloudstack.ssh_key = "~/.ssh/id_rsa"
  end
end
```

And finally, we will log in as 'root' so we need to tell this to Vagrant.

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
  config.vm.provider :cloudstack do |cloudstack, override|
    cloudstack.api_key    = "AAAA_YOUR_API_KEY"
    cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
    cloudstack.service_offering_name = "Micro"
    cloudstack.security_group_names = ['default']
    cloudstack.keypair = "name_of_your_keypair"
    cloudstack.ssh_key = "~/.ssh/id_rsa"
    cloudstack.ssh_user = "root"
  end
end
```

When deplying our Instance, Vagrant will "talk" to Exoscale Compute through the
vagrant-cloudstack plugin, start a new instance with the parameters we set,
and will attempt to connect via SSH... so we need to allow the incoming connections on port 22!
[Add a rule to the default Security Group](https://community.exoscale.ch/documentation/compute/security-groups/#a-simple-example).

Now let's **deploy** our new Instance:

```
$ vagrant up --provider=cloudstack
Bringing machine 'default' up with 'cloudstack' provider...
...
==> default: Machine is booted and ready for use!
...
```

When you get your prompt back, try `vagrant ssh` to connect to your machine.
Run a few command like `ifconfig` or `cat /proc/cpuinfo` to verify that you're
connected to an Exoscale Instance... Type `exit` to disconnect.

To avoid unnecessary costs, destroy the Instance. We can create it again when we
need it again.

```
$ vagrant destroy -f
==> default: Disabling Static NAT ...
==> default: Deleting the port forwarding rule ...
==> default: Deleting the firewall rule ...
==> default: Terminating the instance...
==> default: Waiting for instance to be deleted
```

In case you have an error message "No host IP was given to the Vagrant core NFS
helper. This is an internal error that should be reported as a bug.", add the
following line in the `config.vm.provider` block:
`override.nfs.functional = false`

## Provisioning

As we said we want our machine to act as a web server, we need to install
Apache or Nginx or any other web server.
Normally, we would connect to the machine, run `apt-get install nginx` and our
web server would be ready.

With Vagrant we will simply add the following line under the `config.vm.provider` block:
`config.vm.provision "shell", inline: "apt-get -y install nginx"`

You Vagrantfile looks like this:

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "exoscale-ubuntu-1504-10GB"
  config.vm.provider :cloudstack do |cloudstack, override|
    cloudstack.api_key    = "AAAA_YOUR_API_KEY"
    cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
    cloudstack.service_offering_name = "Micro"
    cloudstack.security_group_names = ['default']
    cloudstack.keypair = "name_of_your_keypair"
    cloudstack.ssh_key = "~/.ssh/id_rsa"
    cloudstack.ssh_user = "root"
  end
  config.vm.provision "shell", inline: "apt-get -y install nginx"
end
```

Add a rule for incoming connection to port 80 from any IP on your default
Security Group and then deploy your Instance

```bash
$ vagrant up --provider=cloudstack
```

Find the IP of your Instance, either by:

* Connecting to the web interface
* SSH'ing to your Instance: `vagrant ssh` and run `ifconfig`
* Run `ifconfig` over ssh: `vagrant ssh -c ifconfig`
* Displaying the SSH configuration: `vagrant ssh-config`

Go to this IP on your browser.
If you see "Welcome to nginx on Ubuntu!", congratulations!
Your (very basic) web server is up and running.


## Deploying multiple Instances

Let's make things a bit more interesting: from our Vagrantfile, start
1 Instance with Nginx, 1 Instance with Apache

We only need to replace the line
`config.vm.provision "shell", inline: "apt-get -y install nginx"` with the
definition of our Instances:

```bash
config.vm.define 'web-nginx' do |machine|
  machine.vm.provision :shell, inline: "apt-get -y install nginx"
end

config.vm.define 'web-apache' do |machine|
  machine.vm.provision :shell, inline: "apt-get -y install apache"
end
```

Run `vagrant up --provider=cloudstack` and you should see 2 Instances being
deployed.

This is not really ideal though, as each Instance will share the same provider's
configuration:
* Same box so same Operting System and same disk
* Same Service Offering, so same CPU and RAM
* Same security Group...

Let's tweak our Vagrantfile so we're able to choose different parameters for
each Instance.

```bash
Vagrant.configure(2) do |config|
  config.vm.define 'web-nginx' do |machine|
    machine.vm.box = "exoscale-ubuntu-1504-10GB"
    machine.vm.provider :cloudstack do |cloudstack, override|
      cloudstack.api_key    = "AAAA_YOUR_API_KEY"
      cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
      cloudstack.service_offering_name = "Micro"
      cloudstack.security_group_names = ['default']
      cloudstack.keypair = "name_of_your_keypair"
      cloudstack.ssh_key = "~/.ssh/id_rsa"
      cloudstack.ssh_user = "root"
    end

    machine.vm.provision :shell, inline: "apt-get -y install nginx"
  end
end
```

We can duplicate the block `config.vm.define` for each additional Instance and
change parameters according to your needs. Don't forget to `vagrant add` the
boxes you want to use.

Below is an example of an Micro Instance with ubuntu 15.04 starting Nginx, and
a Tiny Instance with Ubuntu 12.04 starting Apache 2...

```bash
Vagrant.configure(2) do |config|
  config.vm.define 'web-nginx-1504' do |machine|
    machine.vm.box = "exoscale-ubuntu-1504-10GB"
    machine.vm.provider :cloudstack do |cloudstack, override|
      cloudstack.api_key    = "AAAA_YOUR_API_KEY"
      cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
      cloudstack.service_offering_name = "Micro"
      cloudstack.security_group_names = ['default']
      cloudstack.keypair = "name_of_your_keypair"
      cloudstack.ssh_key = "~/.ssh/id_rsa"
      cloudstack.ssh_user = "root"
    end

    machine.vm.provision :shell, inline: "apt-get -y install nginx"
  end

  config.vm.define 'web-apache-1204' do |machine|
    machine.vm.box = "exoscale-ubuntu-1204-10GB"
    machine.vm.provider :cloudstack do |cloudstack, override|
      cloudstack.api_key    = "AAAA_YOUR_API_KEY"
      cloudstack.secret_key = "BBBB_YOUR_SECRET_KEY"
      cloudstack.service_offering_name = "Tiny"
      cloudstack.security_group_names = ['default']
      cloudstack.keypair = "name_of_your_keypair"
      cloudstack.ssh_key = "~/.ssh/id_rsa"
      cloudstack.ssh_user = "root"
    end

    machine.vm.provision :shell, inline: "apt-get -y install apache2"
  end
end
```

## Going further

**Create the security group and rules on-the-fly**:

Here's an example of Security Group that will be created when runing `vagrant up`

```ruby
cloudstack.security_groups = [
      {
        :name         => "Awesome_security_group",
        :description  => "Created from the Vagrantfile",
        :rules => [
          {:type => "ingress", :protocol => "TCP", :startport => 22, :endport => 22, :cidrlist => "0.0.0.0/0"},
          {:type => "ingress", :protocol => "TCP", :startport => 80, :endport => 80, :cidrlist => "0.0.0.0/0"}
        ]
      }
    ]
```

You need to remove the list of Security Groups already defined.
