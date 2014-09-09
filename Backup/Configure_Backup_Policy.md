---
title: Configure Backup Policy
date: 2014-09-09 09:28 +01:00
category: Backup
tags: backup
---
## Configure Backup Policy

* Introduction

Avamar uses groups to implement various policies to automate backups and enforce 
consistent rules and system behavior across an entire segment, or group, of the user 
community.

* Create a backup policy

Click on Policy in the Avamar console

![Avamar Console Policy](ConfigureBackupPolicy1.png)


Select the Groups tab

Select the Avamar domain to which the group should belong and right click on it

Right click on New Group

![Avamar Policy](ConfigureBackupPolicy2.png)


Add the name of your group

Untick Disabled to immediately enable regularly scheduled client backups for the new group

Click on Next 

![New Group](ConfigureBackupPolicy3.png)


Select an existing dataset for the group

Click on Next 

![Existing Dataset](ConfigureBackupPolicy4.png)


Select an existing schedule for the group

Click on Next 

![Existing Schedule](ConfigureBackupPolicy5.png)


Select an existing retention policy for the group

Click on Next 

![Existing Policy](ConfigureBackupPolicy6.png)


Select clients for your group (Hold CTRL to select numerous one)

Click on Finish 

![Select Clients](ConfigureBackupPolicy7.png)


New group policy appears in your domain group

![New Group Policy](ConfigureBackupPolicy8.png)








































