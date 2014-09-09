---
title: Avamar Bare Metal Recovery
date: 2014-09-09 09:28 +01:00
category: Backup
tags: backup
---
## Bare Metal Recovery for Managed Cloud



* Windows BMR

Windows BMR capabilities and limitations:

This solution provides several Avamar features to simplify BMR of Windows Server 
2008, Windows Server 2012, Windows 7, or Windows 8 client machines

The WinPE ISO is available with 64-bit and 32-bit version.

Avamar provides BMR from physical machine to physical machine, as well as virtual 
machine to virtual machine.

N.B. : 

1) Avamar Windows Disaster Recovery is not supported for volumes encrypted with 
BitLocker Drive Encryption.

2) Recovery will fail to initialize and format the disk when the restore system’s 
disk size is different than the initial backup system disk size, even though the restore 
system’s disk size is sufficient for the disaster recovery backup data. 
All target disks on the restore system need to be as large as the original disks. After disaster recovery, 
there may be some unallocated space. This extra space can be used by extending the 
partition after the DR.

We highly recommend to choose exactly the same disk size as the initial one.


* Download the WinPE ISO

Open a web browser and type the following URL:

[http://exosafe.exoscale.ch]


Select the version required for Windows x86 (32-bit or 64-bit) and downloaded "AvamarWindowsSystemRecover_x86"

![Download ISO](BMR1.png)


* Create an empty VM in VMware vCloud Director

![Create VM vCloud Director](BMR-NewVirtualMachine.png)


* Configure your Virtual Machine

![Configure VM vCloud Director](BMR-CreateEmptyVM.png) 

Hard disk size must be at least the same as the source (we recommend you to put the same size in order to avoid errors)


* Connect VM to network

![Connect VM to Organization Network](BMR-ConnectEmptyVMToNetwork.png)

VM must be connected to network and also to internet in order to permit the restore
 


* Import AvamarWindowsSystemRecover_x86 ISO file to your catalogue

Go to your Organization catalog and upload the ISO.

PS : It must be on the same Virtual DataCenter as the target VM 

![Add ISO to catalogue](BMR-UploadToCatalog.png)


* Mount the ISO

![Mount ISO on VM](BMR-VMMountIso.png)

* Power on your VM and open the console

![Avamar System Recovery Wizard](BMR4.png)


Click on Next button


* Select your Network Interface (select default Intel PRO/1000)

![Select NIC](BMR5.png)


Click on Next button


* Fill in fields with your parameters

Select "Use the following IP Address"

Host Name : Name of the target VM

DNS Domain : Name of your domain (if exist)

IP Address : VM address IP

Subnet Mask : VM subnet mask

Default Gateway : VM default Gateway

Select "Use the following DNS server addresses"

DNS Server Preferred : IP of your DNS Server (if exist)

DNS Server Alternate : IP of your DNS Server (if exist)

Example: 
![Configure Network NIC](BMR6.png)


Click on Next button


* Check if the target VM own at least the same disk size as the source

![Available Disks](BMR7.png)


Click on Next button



* Fill in fields with your exoscale backup credentials (provided by email)

Avamar Server : exosafe.exoscale.ch

Avamar User Name : ManagedCloud_xxx_xxx_Admin

Avamar Password : xxxxxxxxxxxxxxxxxx

Avamar Domain : ManagedCloud_xxx_xxx

Avamar Account : /ManagedCloud_xxx_xxx/YourAvamarClient


- Replace x with your informations
- YourAvamarClient correspond to your VM name registered on your Avamar Domain

![Select Server](BMR8.png)


Click on Next button. 


* Select a backup in the list


![Select Backup](BMR9.png)


Click on Next button


* Check the volume summary

PS: it's recommended to perform a quick format

![Critical Volumes](BMR10.png)


Click on Next button


* System Recovery Summary

![Avamar Alarm](BMR11.png)


Click on Restore button and tick the case to confirm then click OK.


* Recovery is being performed

![Status](BMR12.png)


* Success Recovery

![Sucess Recovery](BMR13.png)


Reboot to finalise the restore. Your new system will boot automatically.


* Miscellaneous

If you get this error. 


![Get Critical Volume Results Error](BMR14.png)



That means, disaster recovery have failed to initialize and format the disk.

It happen if the target system disk size is less than the source backup system disk size. 



[http://exosafe.exoscale.ch]: http://exosafe.exoscale.ch

























