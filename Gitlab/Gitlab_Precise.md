---
title: Deploy Gitlab on Ubuntu 12.04 with Vagrant
date: 2014-05-18 20:00 +01:00
category: Gitlab
authors: nicolasbrechet
tags: ubuntu, gitlab, vagrant
---


## What is Gitlab

GitLab offers git repository management, code reviews, issue tracking, activity feeds and wikis. 
A single GitLab server can handle more than 25,000 users but it is also possible to create a high availability setup with multiple active servers.

GitLab Community Edition (CE) is open source software to collaborate on code. 
Create projects and repositories, manage access and do code reviews. 
GitLab CE is on-premises software that you can install and use on your server(s).


## What is Vagrant

Create and configure lightweight, reproducible, and portable development environments.

Vagrant provides easy to configure, reproducible, and portable work environments built on top of industry-standard technology and controlled by a single consistent workflow to help maximize the productivity and flexibility of you and your team.

To achieve its magic, Vagrant stands on the shoulders of giants. Machines are provisioned on top of VirtualBox, VMware, AWS, or any other provider. Then, industry-standard provisioning tools such as shell scripts, Chef, or Puppet, can be used to automatically install and configure software on the machine.


## What we'll do

We will show you how to configure a Vagrantfile to deploy an Ubuntu 12.04 instance, provision it with Puppet to install Gitlab and its dependencies. All what's left to you is to update your DNS records and you will be hosting a Github replacement.

This tutorial has been tested with Vagrant 1.6.2 and the vagrant-cloudstack plugin v0.6.0.


## Prerequisites

* You have an exoscale Open Cloud Compute account with some credit.
* Your SSH Keypair is setup. If not, read the [documentation](https://portal.exoscale.ch/documentation/open-cloud/tutorials/ssh-keypairs "keypairs documentation")
* You have installed Vagrant. If not, install it from the [Vagrant download page](http://www.vagrantup.com/downloads.html "Vagrant download")
* You have installed the vagrant-cloudstack plugin: `vagrant plugin install vagrant-cloudstack`

## First steps

1. Install the box
	* Download the box from [exoscale's Open Cloud Compute templates](https://www.exoscale.ch/open-cloud/templates/ "exoscale"): it should be Linux Ubuntu 14.04 LTS 64-bit 50GB Disk
	* `vagrant box add Linux-Ubuntu-12.04-LTS-64-bit-50GB-Disk Linux-Ubuntu-12.04-LTS-64-bit-50GB-Disk.box`
2. Create a directory for this tutorial and change directory
	* `mkdir -p ~/src/exoscale/gitlab-on-exo`
	* `cd ~/src/exoscale/gitlab-on-exo`
3. Run `vagrant init Linux-Ubuntu-12.04-LTS-64-bit-50GB-Disk`. This will create a Vagrantfile, initialized with the exoscale box. Inside the `Vagrant.configure` block, remove everything except the line starting with `config.vm.box`


```ruby
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.box = "Linux-Ubuntu-12.04-LTS-64-bit-50GB-Disk"
  
end
```

## SSH config

To access the instance once it's provisionned, you need to provide the username you're connecting with, and its private SSH key.

To do this, according to [Vagrant's documentation](http://docs.vagrantup.com/v2/ "Vagrant documentation"), all you need to do is add the following parameters to your Vagrantfile :

* `config.ssh.username`
* `config.ssh.private_key_path`

Exoscale's instances are created without any users, only root. So after `config.vm.box` you need to add:

* `config.ssh.username = "root"`

And, you probably guessed it, the private_key_path parameter is the path to your own private key, that you've set up in [exoscale's portal](https://portal.exoscale.ch "exoscale portal"). If not, it's time to go read the [documentation](https://portal.exoscale.ch/documentation/open-cloud/tutorials/ssh-keypairs "keypairs documentation").

So it should look something like:

* `config.ssh.private_key_path = "~/.ssh/id_rsa"`

## Provider config

By default, Vagrant uses the Virtualbox provider. So unless you specify the provider you're using, Vagrant will try to start your instance on Virtualbox. To avoid that, we use the vagrant-cloudstack plugin, and we tell Vagrant that the provider is Cloudstack. This is actually what exoscale Open Cloud uses. Add the following block to your Vagrantfile, after the `config.ssh` parameters:

* `config.vm.provider :cloudstack do |cloudstack, override|`
* `end`

That's basically telling Vagrant "now we're using Cloudstack" and inside the block, we can define our parameters.
We need to specify what's the name of the keypair to use in Cloudstack. That's how you named your key in [exoscale's portal](https://portal.exoscale.ch "exoscale portal").
Add this line inside the block:

* `cloudstack.keypair = "name_of_your_keypair"`

Now we need to provide some authentication, and for that, we provide our API Key and Secret Key.
Retrieve your API Key & Secret Key from [exoscale's portal](https://portal.exoscale.ch "exoscale portal") : click on your account email and choose `Account details`. Then click on `API Details`. 
Add the following lines inside the block:

* `cloudstack.api_key = "your_api_key"`
* `cloudstack.secret_key = "your_secret_key"`

So now, Cloudstack knows who we are and how to connect us to the instance. So let tell Cloudstack what kind of "offering" we need. Gitlab's requirement is 2 cores and 2GB, so we need a Small instance.
You can see all of exoscale's Service Offering by [querying the API from your browser](https://portal.exoscale.ch/api/serviceofferings "exoscale's service offerings"). It returns JSON formatted content. 

Grab the `id` of the Service Offering and add the following line inside the block:

* `cloudstack.service_offering_id = "21624abb-764e-4def-81d7-9fc54b5957fb"`

Almost done! We need to tell Cloudstack to use a "Basic" type of networking, because that's how exoscale Open Cloud is configured.
Add the following line inside the block:

* `cloudstack.network_type = "Basic"`

At this point, if you were to launch this instance, it would boot, but you wouldn't be able to connect to it from your machine: by default, all incoming traffic is prohibited, only outgoing traffic is allowed. We need to configure the Security Groups!

## Firewall configuration: security groups

Read the doc on security groups. Really, it takes 5 minutes.

Now what do we need to do?

Gitlab is accessed on port 80 (or if you're securing it with SSL, 443, but it's outside the scope of this tutorial)
We want to connect to the VM using SSH, so we need port 22 to accept incoming connections.

So we could go to [exoscale's portal](https://portal.exoscale.ch "exoscale portal"), in the Firewalling section, and add the Security Group we need. But it's simpler to let Vagrant do it !

To create a security group from Vagrant, you need to add the `cloudstack.security_groups` parameter, which is an array containing a hash with 3 key/value pairs:

* a name
* a description
* the rules we want, and that's an Array of Hash!

So you can start by adding the following inside the `config.vm.provider :cloudstack do |cloudstack, override|` block, under `cloudstack.network_type = "Basic"`:

```ruby
cloudstack.security_groups = [
    { 
      :name         => "gitlab_on_precise_from_vagrant",
      :description  => "Created from the Vagrantfile",
			:rules 	=> 
	  }
  ]
```

All we're missing is... the actual rules! It's a Ruby Array `[]`. It should contain one Hash `{}` per rule.

* SSH from anywhere: `{:type => "ingress", :protocol => "TCP", :startport => 22, :endport => 22, :cidrlist => "0.0.0.0/0"}`
* HTTP from anywhere: `{:type => "ingress", :protocol => "TCP", :startport => 80, :endport => 80, :cidrlist => "0.0.0.0/0"}`

So the `cloudstack.security_groups` looks like this:

```ruby
cloudstack.security_groups = [
    { 
      :name         => "gitlab_on_precise_from_vagrant",
      :description  => "Created from the Vagrantfile",
			:rules 				=> [
				{:type => "ingress", :protocol => "TCP", :startport => 22, :endport => 22, :cidrlist => "0.0.0.0/0"},
				{:type => "ingress", :protocol => "TCP", :startport => 80, :endport => 80, :cidrlist => "0.0.0.0/0"}
			]
	  }
  ]
```

Now you would be able to SSH into your instance, if you were to start it now. But it wouldn't run any service.

## Provisionning

Let's "define" our instance in the Vagrantfile. This will allow us to provision it using the methods we want.
After the `config.vm.provider...` add the following block:

`config.vm.define "precise_gitlab" do |gitlab|
end`

This does nothing, so we need to add some provisionning instructions inside this block.

### Bootstrapping Puppet

Using a script from [Hashicorp](https://github.com/hashicorp/puppet-bootstrap "Puppet Bootstrap"), we can easily install Puppet on our instance. 
Let's clone the repo! Save the changes in your Vagrantfile, exit your text editor, ...

* `git clone https://github.com/hashicorp/puppet-bootstrap`

Set up the repo to use a know working version/commit:

* `cd puppet-bootstrap && git checkout c65048b776866cd28fa327559e3f06239eb2745e`

In the Vagrantfile, we need to add the following line to our last block:

* `gitlab.vm.provision :shell, path: "./puppet-bootstrap/ubuntu.sh"`

It's simply telling Vagrant to use the "shell" provisionner to run the script at path "`./puppet-bootstrap/puppet-bootstrap.sh`"

Let's add an "inline" shell provisionning instruction, to be sure we have the "build-essential" package. 

* `gitlab.vm.provision :shell, inline: "apt-get -y install build-essential"`

You can save the Vagrantfile and exit your text editor for now.

### Setting up librarian puppet

The `vagrant-librarian-puppet` plugin lets us use a Puppetfile describing the Puppet Modules we need. It will download the Modules locally, and Vagrant will know how to use them. 

Installing the plugin is simple:

* `vagrant plugin install vagrant-librarian-puppet`

### Creating our Puppetfile

At the root of our tutorial directory, let create a Puppetfile.

* `touch ~/src/exoscale/gitlab-on-exo/Puppetfile`

The content should be:

```
forge 'http://forge.puppetlabs.com'

mod 'gitlab',
    :git  => 'git://github.com/sbadia/puppet-gitlab.git',
		:ref  => '0.1.3'
mod 'gitlab_requirements',
    :git  => 'git://github.com/sbadia/puppet-gitlab-requirements',
		:ref  => 'a70ef46203eca56beed1fb88ff186583cd1a50ca'
```

We won't go into details as to what the Puppetfile does. Librarian-Puppet will simply install the `gitlab` module, the `gitlab_requirements` module, from Github, and the dependencies of both modules. The `:ref` specifies which version/commit we want, and it's simply because we've tested it.
Puppet will be able to get the Puppet modules to install Gitlab.

### Creating our manifest

We will store our manifest in a "manifests" directory. 

* `mkdir -p ~/src/exoscale/gitlab-on-exo/manifests`

Let's create an "init.pp" file and describe the configuration we want.

* `touch ~/src/exoscale/gitlab-on-exo/manifests/init.pp`

The content of the file is the following:

```ruby
node 'gitlab.yourdomain.tld' {
	
	$gitlab_dbname  = 'gitlab_prod'
	$gitlab_dbuser  = 'gitlab_user'
	$gitlab_dbpwd   = 'str0ngp@ssw0rd'

	class { 'gitlab_requirements':
	  gitlab_dbname   => $gitlab_dbname,
	  gitlab_dbuser   => $gitlab_dbuser,
	  gitlab_dbpwd    => $gitlab_dbpwd,
	}

	class { 'gitlab':
	    git_email         => 'changeme@notadoma.in',
	    gitlab_domain     => 'gitlab.yourdomain.tld',
	    gitlab_dbtype     => 'mysql',
	    gitlab_dbname     => $gitlab_dbname,
	    gitlab_dbuser     => $gitlab_dbuser,
	    gitlab_dbpwd      => $gitlab_dbpwd,
	    ldap_enabled      => false,
	}

	Class['gitlab_requirements'] -> Class['gitlab'] ->
	  file { '/etc/nginx/conf.d/default.conf':
	    ensure => absent,
	  } ->
	  exec { '/usr/sbin/service nginx reload': }
}
```

You should, of course, update the `$gitlab_dbpwd`, the `git_email` and `gitlab_domain`.

You should change `node 'gitlab.yourdomain.tld'` by your node name.

### Configuring the Vagrantfile to provision using Puppet

So how does Vagrant know it should use Puppet ? How does it know where the Puppetfile is, and that it should use it ?

We simply need to add the following in our `config.vm.define "gitlab1204"...` block:

```ruby
gitlab.vm.provision "puppet" do |puppet|
  puppet.facter = {
    "fqdn"	=> "gitlab.yourdomain.tld"
  }
  puppet.manifests_path = "manifests"
  puppet.manifest_file  = "init.pp"
  puppet.module_path = 'modules'
end
```

And one last line to add, this time before the block:

* `config.librarian_puppet.puppetfile_dir = "."` 

This is telling vagrant to use the `librarian_puppet` plugin, and that the Puppetfile is located in the current directory.

Since the Puppet provisioner will fail if the path provided to "module_path" doesn't exist, we need to create it.

* `mkdir -p ~/src/exoscale/gitlab-on-exo/modules`

## Deployment time !

### Step 1

Run `vagrant up --provider=cloudstack`

### Step 2

Grab a coffee, you deserve a break !

## DNS

You need to retrieve the IP address of your new Gitlab instance, either by connecting to the instance with Vagrant (`vagrant ssh`) and running `ifconfig`, or you can connect to [exoscale's portal](https://portal.exoscale.ch "exoscale portal").
An easier way is to type the following command:

* `vagrant ssh-config`

The second line of the result should give you the IP address of your instance.

## Verify

On your browser, go to http://<your_ip> or http://<you_domain_name> and you should get a nice Gitlab login page. 

