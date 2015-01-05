---
title: Adding ping connectivity to your instances
date: 2014-01-16 09:28 +01:00
category: Firewall
tags: ping, network, ICMP, Firewall
---

## Understanding ICMP

On top of TCP and UDP traffic, the Internet Protocol (IP) can also carry
ICMP messages. 

While all ICMP requests are closed by default on your instances, it is easy
to add ECHO REPLY ICMP traffic rule to your security group in order to be able
to ping your instance.
With Ping activated to an instance, it is then easier to enable it in your 
preferred monitoring tool or service.

## Adding ping to your instances

To add ingress ping connectivity to your instance follow this procedure:

* **Locate the security group**: navigate to the instance list and choose
  the security group enabled on your instance on which you want to add ping
  traffic. Please note that all instances belonging attached to this security
  group will gain ping access too.

* **Add a rule**: in the selected Security group add a INGRESS rule, restrict
  the source IP or Network range, then select the ICMP protocol.

* **Echo**: Select the type ECHO (type 8) and code NO CODE

* **Save**: Save your rule by pressing on Add and allow a few moments for 
  the rule to propagate on the exoscale cloud

![Addin ECHO ICMP Rule](adding-ping-2.png)

The resulting rules appears like this:

![Echo ICMP Rule](adding-ping-1.png)

## Reminder

* **Outbound traffic**: By default all outbound traffic is
  permitted, however as soon as you define an outbound rule, outbound traffic
  is only allowed for the defined outbound rules. See [managing outbound
  security rules](/tutorial/outbound-traffic-rules-accept-or-deny/)
  for more information.

* **Allow Outbound Reply**: to allow outbound reply if the outbound traffic 
  is restricted, add an outbound rule with ICMP Type ECHO REPLY (type 0) and 
  code NO CODE.

![Addin ECHO REPLY ICMP Rule](adding-ping-3.png)
