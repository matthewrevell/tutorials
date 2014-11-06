---
title: Key concepts
date: 2014-01-16 09:28 +01:00
category: Getting started
tags: concepts, cloud
---
exoscale is a public infrastructure-as-a-service provider and builds up on the
following key concepts.

## Security Groups

**Security groups** are groups of firewall rules managed at the hypervisor
level in order to restrict incoming and outgoing network trafic. When you
create an instance, you can choose which security groups to attach to the
instance. Firewall rules defined in your security groups take precedence over
the default rules, which are:

* All outgoing traffic is allowed
* All incoming traffic is forbidden

## Scaling

Scaling instances can be achieved simply using the "scale" button on an
instance detail view. Scaling up and down is allowed: a *small* instance can
become a *large* one for a few hours and then be adjusted to a *medium* one.

Note that you can only scale stopped instances.

## User Data

**User Data** is a commonly-used format to make an instance perform a set
of actions while it boots. It consist of a file that is served to a booting
instance and includes declarations and blobs of data which define actions
such as upgrading the instance on boot up, installing packages or
bootstrapping configuration management tools such as
[Puppet](http://puppetlabs.com/puppet/puppet-open-source) or
[Chef](http://www.opscode.com/chef/).

The [official documentation](https://cloudinit.readthedocs.org/en/latest/)
explains the syntax and lists some examples.

## API

The API, or Application Programmable Interface, is a standardized access to
our Cloud Platform. Without any development, it can be used to interact with
exoscale products in order to create, update or delete ressources.

For more information, visit the [API
reference](/documentation/open-cloud/reference/api_reference).

## Tags

Tags are key/values pairs associated with a ressource. They can be used to
mark instances belonging to the same group or assign parameters to the
instance before it is even created.

Tags can be queried by the instance itself in order to take these values into
account for its configuration.
