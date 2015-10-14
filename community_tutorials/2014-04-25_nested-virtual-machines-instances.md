---
title: "Install KVM on a virtual machine to get nested instances"
slug: "install-kvm-on-a-virtual-machine-to-get-nested-instances"
date: 2014-04-25 09:28 +01:00
tags: "kvm"
---

It might be usefull sometimes to run virtual machines on
top of a virtual machine.

## Install KVM in your instance

    apt-get install qemu-kvm virt-top libvirt-bin 

Prepare your future host for virtual machines by first creating a 
storage path for the virtual machines:

    mkdir -p /var/kvm/images

And installing the installer:

    apt-get install virtinst

Install a first guest instance:

    virt-install --name ubuntu-trusty --ram 256 --disk path=/var/kvm/images/trusty.img,size=3 --vcpus 1 --os-type linux --os-variant ubuntutrusty  --graphics none --console pty,target_type=serial --location 'http://fr.archive.ubuntu.com/ubuntu/dists/trusty/main/installer-amd64/' --extra-args 'console=ttyS0,115200n8 serial'

Follow the setup wizard for a default Ubuntu install. At the end of
the process the guest will reboot and eventually stop (because QEMU is
not configured to restart on guests reboots).

The guest is now ready but will not benefit from all features
of the host instance.


## Enter nested KVM

Nested KVM is not installed by default on the host instance, check with:

    cat /sys/module/kvm_intel/parameters/nested
    N

The N stands for no. Activate the option in the kernel, via a kernel module option:

    echo “options kvm-intel nested=1″ | sudo tee /etc/modprobe.d/kvm-intel.conf

and reboot you instance. After reboot, check that the feature is activated:

    cat /sys/module/kvm_intel/parameters/nested
    Y

Verify the cpu vendor end model on the host instance:

    virsh capabilities | egrep "/model|/vendor"

and note the values:

      <model>cpu64-rhel6</model>
      <vendor>Intel</vendor>
      <model>apparmor</model>
      <model>dac</model>

Then edit the guest instance configuration to activate the access to VMX extensions
exposed by the nested KVM feature:

    virsh edit ubuntu-trusty

Add the following in the `domain`xml brackets:

    <cpu mode='custom' match='exact'>
      <model fallback='allow'>cpu64-rhel6</model>
      <vendor>Intel</vendor>
      <feature policy='require' name='vmx'/>
    </cpu>

Then save and start the instance:

    virsh start ubuntu-trusty

and check the instance is running:

    virsh list

    Id    Name                           State
    ----------------------------------------------------
    8     ubuntu-trusty                  running

And connect to your guest instance:

    virsh console ubuntu-trusty

Note: default escape character is CTRL + ]


## References:

* https://github.com/torvalds/linux/blob/master/Documentation/virtual/kvm/nested-vmx.txt


