---
title: "Moving an Instance Between Accounts and Organizations"
slug: "instance-move"
meta_desc: "Move up as a team: a personal project can grow up to an organization and you can move Instances to your organization's account, letting your team manage them."
tags: "compute,organizations"
---

Once you've joined an [Organization](/documentation/platform/organizations/), you have
the possibility to move an Instance from your account to this Organization.

## Requirements
Several requirements need to be met:

* your Organization should not be suspended
* it should have enough credit to run the Instance
* you should have the role Owner or Tech
* the Instance should be stopped

**Note that your instance will be assigned a new IP address.**

## Moving an Instance
From the `COMPUTE` menu, choose the `INSTANCES` sub-menu and click on your
Instance.
Make sure your Instance is in status `Stopped`. Stop it otherwise.
Click on the `MOVE` button on the top right corner of the page.
Choose the destination Organization.
You need to choose the Security Group(s) for the Instance. Security Groups
are defined per Organization, you cannot move the Security Groups from your
account to the Organization.
You can choose to start the Instance once it has been moved to the Organization.

To proceed with the move, click on the `MOVE`
