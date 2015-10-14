---
title: "Snapshots"
slug: "snapshots"
meta_desc: "Find out how to create and delete snapshots to get point-in-time recovery for your Exoscale Compute Instances, straight from our intuitive web interface"
tags: "compute"
---

Snapshots provide a way to get point-in-time recovery for your Instance.

## Create a new snapshot
Select the `INSTANCES` sub-menu from the `COMPUTE` menu on the left. From the
Instances list screen, click on your Instance.
In the Instance details view, click on `Take snaphot` to create a new snapshot.

## Delete a snapshot
Select the `INSTANCES` sub-menu from the `COMPUTE` menu on the left. From the
Instances list screen, click on your Instance.
In the Instance details view, select the Snapshot you want to delete and
click on `Delete snaphot` to delete it.

## Example
When you need to apply patches to an application, you can create a new snapshot
before applying the patches.
Once the patches are installed, if your tests are not satisfying, you can revert
to the snapshot.

## Limits
* You are limited to 10 snapshots per Account. Please contact support should you
  need more.
* Only one action is permitted at a time. For example, when a snapshot is
  being destroyed, you cannot create a new snapshot.
* Snapshots cannot be named, they are identified by a UUID.
