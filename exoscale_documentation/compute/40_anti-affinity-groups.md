---
title: "Anti-Affinity Groups"
slug: "anti-affinity-groups"
meta_desc: "Anti-Affinity Groups let you specify which Exoscale Instances should run on separate hypervisors. Learn how to manage these groups using our web interface"
category: ""
tags: "anti-affinity"
---

Anti-Affinity Groups let you specify which Instances should run on separate
hosts. In case of a HA cluster for example you may wish to keep your Instances
on separate hypervisors to ensure a more reliable fault tolerance.

## Manage groups
From the `COMPUTE` menu choose the `ANTI-AFFINITY` sub-menu. You can then
create, delete or view the details of your groups and in the detail of a group
you can see the Instances attributed to that group.

## Attributing Anti-Affinity Groups during Instance creation
When you create an Instance you will be offered the list of your
Anti-Affinity Groups. You can choose to attribute your Instance to none or
several groups.

Although there is no limit to how many groups you may have, **an Anti-Affinity
group cannot contain more than 8 Instances**. If a group is full it won't be
presented in the available options.

## Remove an Instance from an Anti-Affinity group
Select your Anti-Affinity Group from the group list view and you will be
presented a list of Instances attributed to that group. From this view you can
easily remove an instance from the selected group.

## How to use the Anti-Affinity Groups
**Two Instances on the same group will be spawned on different hypervisors.**

Once created, your Instance will stay on its hypervisor during its entire life.
You will probably want to define your groups in advance to define a proper
distribution strategy.

An example of how Anti-Affinity Groups may prove useful would be to attribute
your machines to different groups based on their role: as an example you may
define an Anti-Affinity Group for the database servers and another one for the
front servers. In this way any issue on one hypervisor will not affect your
entire infrastructure.

If you find yourself needing more than 8 Instances per group, a possible
strategy would be to split a group in two to grant the best spread possible.
