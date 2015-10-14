---
title: "Scaling an Instance"
slug: "instance-scaling"
meta_desc: "Learn how to change the amount of CPU, memory or disk of your Instance. Exoscale Compute makes it very easy to scale up and down to best suit your needs"
tags: "compute"
---

Your application will grow in time, and so will your virtual machines! During
the life of your project you will be able to resize your instance up and down,
scaling its service level (the combination of CPU's cores and RAM) and its disk
size, optimizing it for your needs.

Select the instance you wish to operate on, and from the instance detail you
will be able to access the scaling interface.

**Please note that every scaling operation needs to be performed on a stopped
instance.**

## Scaling the Service Level
Scaling your CPU's cores and the RAM of your machine is pretty straightforward:
stop your instance, access the scaling view, choose your new Service Level and
restart when finished. Remember that some Service Levels may be unavailable due
to eventual restrictions of your selected operative system.

## Scaling the disk size
As for scaling the Service Level, scaling your disk size is easy. You can resize
your volume up and down by steps of 1 GB, with a higher limit of 400 GB per
instance and a lower limit dictated by your operative system.
