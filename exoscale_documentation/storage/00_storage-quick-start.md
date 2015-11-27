---
title: "Storage Quick Start"
slug: "quick-start"
meta_desc: "Exoscale's Simple Object Storage (SOS) allows you to store, distribute and manage large quantities of files through our web interface and via our API."
tags: "storage,quick-start"
---

Through this guide you will learn the basics of Exoscale's Simple Object Storage (SOS).

[Object Storage](https://en.wikipedia.org/wiki/Object_storage) allows you to
store, distribute and manage large quantities of files (objects). Examples
applications span from simple backup to complex media delivery. You can store
almost anything in SOS: assets, backups, email attachments, videos, pictures
and anything else you may imagine.

Your data is kept in Switzerland, under [Swiss privacy regulation](https://www.admin.ch/opc/en/classified-compilation/19920153/index.html).
It is replicated in at least three physical distinct copies to ensure
maximum safety.

**All the actions we will perform through the command-line can also be performed through the interface**: you can do almost everything in the Exoscale Console as well, although the real power of SOS is in the tools you may script and in the clients you may use in your application.

## S3 Compatibility

SOS is S3-compatible: this means it works with most object storage clients and
provides library access from a host of languages. You can drop-in from other
services and you don't have to fear to be locked in. Most of the time a simple change of API credentials and endpoint allows to switch to our service.

S3-compatibility is achieved through Pithos, an open-source frontend for storing files in a Cassandra cluster.

Here are a few clients and tools that interact well:

* [s3cmd](http://s3tools.org/s3cmd)
* [cyberduck](http://cyberduck.io)
* [owncloud](http://owncloud.org/)
* [boto](http://docs.pythonboto.org/en/latest/)

We will use `s3cmd` in this guide.

## Setup

This guide assumes you never used s3cmd. If this isn't the case, simply take a look at the `.s3cfg` example here below, drop in your Exoscale's API Key and API Secret and you are good to go.

First of all install s3cmd with your favorite package-manager:

* Debian/Ubuntu Linux: `apt-get update && apt-get install s3cmd`
* OS X: `brew install s3cmd`

Then create a file named `.s3cfg` in your home directory and copy/paste the following configuration template:

	[default]
	host_base = sos.exo.io
	host_bucket = %(bucket)s.sos.exo.io
	access_key = $EXO_SOS_KEY
	secret_key = $EXO_SOS_SECRET
	use_https = True
	signature_v2 = True

Change `$EXO_SOS_KEY` and `$EXO_SOS_SECRET` with your API Key and API Secret:
you can find them under the *Account* menu in the Exoscale Console.

## Your first Bucket

First we need to create a container for all of our objects, which is called a
*Bucket*.

	s3cmd mb s3://my-new-bucket

This creates a bucket called 'my-new-bucket'.

**Bucket names are unique across our entire platform.** You may encounter an
error creating `my-new-bucket`. If this name is already taken you need to
choose another name.

The reason for this is that buckets and their content can be accessed under
`https://sos.exo.io/my-new-bucket/` or `https://my-new-bucket.sos.exo.io`.
Buckets become subdomains and prefixed URLs under the `sos.exo.io` domain, and
hence names have to be unique. This does not mean objects are publicly
accessible: you can define precise access control with ACLs, and uploaded
objects are private by default.

You can check your bucket is here by listing your buckets:

	s3cmd ls

## Uploading and downloading assets

Now grab a file you have on your computer and upload it to your bucket:

	s3cmd put picture.jpg s3://my-new-bucket/picture.jpg

Check your file was uploaded successfully:

	s3cmd ls s3://my-new-bucket

You can also verify the presence of your file having a peek in the interface:
select the *Storage* service from the main menu and you should see your
`my-new-bucket` listed here. Select the Bucket and you will find your file!

You can try to upload more files in the Console, with a simple drag and drop.
And of course you can get back your files:

	s3cmd get s3://my-new-bucket/picture.jpg picture_download.jpg

## Advanced Options

Buckets and objects support advanced options: [ACL for buckets and objects](/documentation/storage/acl),
[CORS for buckets](/documentation/storage/cors) and [Metadata for objects](/documentation/storage/metadata).

Those options are available through the API but also through the interface:
editing a bucket or an object in the Console opens a drawer with the
configurable options.

### ACL

Access Control List (ACL) allows you to define specific permission at the
bucket level and single object level, allowing a granular access policy.
ACLs are not inherited from parent object.

ACL let you manage access to buckets and objects, as for Read, Write, Read
ACP, Write ACP, and Full Control (Read + Write). Your objects may be accessible by their public URL, your buckets writable by your team and so on.

For in-depth details about ACL's we invite you to read the [SOS ACL documentation](/documentation/storage/acl)

### CORS

CORS (Cross-Origin Resource Sharing) allows your browser-based applications to
perform actions on buckets and objects contained in it. CORS is available only
at the bucket level and its configuration is applied to all the objects
contained.

The [SOS CORS documentation](/documentation/storage/cors) explains it and provides some examples.

### Metadata

Each object contained in a bucket can be tagged with key-value pairs. This metadata is presented through HTTP headers on SOS requests and responses.

Have a look on the [SOS Metadata documentation](/documentation/storage/metadata) for more information and examples.
