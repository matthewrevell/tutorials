---
title: "Make your own dynamic DNS service"
meta_desc: "How to make your own dynamic DNS with Exoscale's DNS Service."
date: "2016-07-11 00:00 +01:00"
authors: "JOduMonT"
category: "dns"
slug:
tags: "dns, dyndns, api, script, tutorials"
---

Dynamic DNS can be very useful when you want to give a publicly addressable presence to machines that have an unstable IP address. 

In this tutorial we'll see how to use Exoscale's DNS service to assign sub-domains to a machine whose IP address is liable to change.


## Prerequisites

First up, you'll need:

1. A DNS zone hosted with Exoscale.
2. The sub-domain you want to point to your dynamic IP address.

## In the Exoscale console

If you haven't already, you'll need to [add a DNS subscription to your Exoscale account](https://portal.exoscale.ch/dns) and then add a domain name.

Next, of course, you'll need to point your domain name to the Exoscale DNS servers:

 * ns1.exoscale.io
 * ns1.exoscale.com
 * ns1.exoscale.ch
 * ns1.exoscale.net

As with any DNS change, it can take a while for the change in nameservers to propagate.

Now, add an *A record* for the sub-domain you're going to use for your dynamic IP address. 

For example: **ddns**.EXEMPLE.COM, with a TTL of one hour.

For now we can enter a placeholder IP address for the A-record, as we'll be updating that dynamically.

That TTL is important as it means that DNS servers will cache the A-record for just one hour, so changes should take no more than one hour to propagate.

![](../img/2016-07-11-ddns-api-recipe/add_dns_record.png)

## On the target machine

Now we need to do some work on the machine that want to reference with our sub-domain.

### Find the record id

Each of your domain's records has a unique ID. We'll need that ID to set the sub-domain's IP address using the Exoscale API.

Here's a simple script that pulls down the domain's record information:

	#!/bin/bash
	#The best I could it's this ugly script
	API="___ YOUR API KEY ___"
	Secret="___ YOUR SECRET KEY ___"
	DOMAIN_NAME="___ YOUR DOMAIN NAME ___"

	curl -H "X-DNS-Token: $API:$Secret" -H "Accept: application/json" "https://api.exoscale.ch/dns/v1/domains/$DOMAIN_NAME/records"

Find your API Key and Secret [in your Exoscale console](https://portal.exoscale.ch/account/profile/api).

Make it executable:

	$ chmod 700 /root/bin/getid_exodns.sh

Then run it:

	$ /root/bin/getid_exodns.sh

You'll get a response back that looks something like this:

	{"record":{"id":**12345678**,"domain_id":123456,"parent_id":null,"name":"**ddns**","content":"1.2.3.4","ttl":600,"prio":null,"record_type":"A","system_record":false,"created_at":"2016-04-27T22:46:38.990Z","updated_at":"2016-04-28T06:20:19.453Z"}}

Make a note of the id as we'll need it next.

## Creating a script to update the sub-domain's IP

We're going to create a bash script that uses the Exoscale API. In the example here we're using the root user but you might prefer to create a dedicated user, with the correct permissions, to run this script.

1. Create a script

Here we're going to put our script at `/root/bin/update_exodns.sh` and it'll look something like this:

	#!/bin/bash
	API="___ YOUR API KEY ___"
	Secret="___ YOUR SECRET KEY ___"
	DOMAIN_NAME="___ YOUR DOMAIN NAME___"
	IP=`dig +short myip.opendns.com @resolver1.opendns.com`
	RECORD_ID="___ THE RECORD ID WE FOUND EARLIER ___"
	RECORD_NAME="___ YOUR SUBDOMAIN TO UPDATE (in our example, it's ddns) ___"
	RECORD_TYPE="A"

	curl -H "X-DNS-Token: $API:$Secret" -H "Accept: application/json" -H "Content-Type: application/json" -X "PUT" -i "https://api.exoscale.ch/dns/v1/domains/$DOMAIN_NAME/records/$RECORD_ID" -d "{\"record\":{\"content\":\"$IP\"}}"

2. Make the script executable

		$ chmod 700 /root/bin/update_exodns.sh

3. Create a **CRON** (as root)

	$ crontab -e

Add this line:

	@hourly /root/bin/update_exodns.sh > /dev/null 2>&1

Now the script will run each hour and update the sub-domain with whatever IP address is assigned to the machine at that time.