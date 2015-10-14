---
title: "Instance Tags"
slug: "instance-tags"
meta_desc: "Associate key/value pairs to an Instance, either from the interface or the API. Query them and use them as metadata to help the management of your Instances"
tags: "compute"
---

Tags are key/value pairs associated with a ressource, in our case an Instance.

Tags can be queried by the Instance itself in order to take these values into
account for its configuration.

## Creating tags
Tags can only be created once the Instance is created.
After your Instance is created, go to the `COMPUTE` menu, choose the `Instances`
sub-menu, and click on your Instance to display the Instance details.
In the Instance Tags area of the Instance details page, you can add a key and
its value.

## Usage
Tags can be accessed directly from the Instance. You will need to access the
Exoscale API, querying the Instance Tags.
See the [API documentation](/api/compute/#tags)
