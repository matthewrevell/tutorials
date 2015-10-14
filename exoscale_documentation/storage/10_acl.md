---
title: "Managing Simple Object Storage ACL"
slug: "acl"
meta_desc: "Access Control List (ACL) let you define specific permission for each object, from the interface or through the API, to fine-tune who can use your data."
tags: "storage"
---

Access Control List (ACL) allows you to define specific permission at the
bucket level and single object level, allowing a granular access policy.
ACLs are not inherited from parent object.

ACL let you manage access to buckets and objects, as for Read, Write, Read
ACP, Write ACP, and Full Control (Read + Write). Your objects may be accessible
by their public URL, your buckets writable by your team and so on.

When you add an object (bucket, file, folder), you're then owner and so, you
will always have FULL_CONTROL on the object.

## Add ACL

**From the Exoscale Console**

Click on the `STORAGE` menu to display the list of your buckets.
If you don't have any buckets, please follow the [Quick Start Guide](/documentation/storage/quick-start)
Click on the `Edit` icon for your object. You are presented with the ACLs for
your object.

You have 2 ways of adding ACLs:

* Using the `Quick ACL` menu: these are presets for commonly used ACLs, known as 'canned ACLs'. For example, setting a file available for Public Read. [See the list below](/documentation/storage/acl/#canned-acls)

* Editing ACLs manually: for each permission you can enter a user email.

Editing the ACLs manually will remove the Quick ACL previously set.

**From command line tool**

We assume you are using [s3cmd](http://s3tools.org/s3cmd).

The `setacl` command modifies the ACLs.

You can then use the following options:

* `--acl-public`
* `--acl-private`
* `--acl-grant=PERMISSION:EMAIL` or `USER_CANONICAL_ID`

```
$ s3cmd setacl --acl-public  s3://<bucket>/<object>
s3://<bucket>/<object>: ACL set to Public  [1 of 1]
```

## View ACL

**From the Exoscale Console**

To view the ACL associated with an Object, you need to edit the Object:
* Click on the `STORAGE` menu
* Navigate to your object
* Click on the `Edit` icon of the Object.

**From command line tool**

We assume you are using [s3cmd](http://s3tools.org/s3cmd).

The `info` command will return information on an object, including its ACLs.

```
$ s3cmd info s3://<bucket>/<object>/
s3://<bucket>/<object>/ (object):
   File size: 0
   Last mod:  Mon, 02 Nov 2015 13:37:00 UTC
   MIME type: application/binary
   MD5 sum:   d41d8cd98f00b204e9800998ecf8427e
   SSE:       none
   policy:    none
   cors:      <?xml version="1.0" encoding="UTF-8"?><CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"></CORSConfiguration>
   ACL:       owner@domain.tld: FULL_CONTROL
   ACL:       *anon*: READ
```
Here the object is READable by anyone, and the owner has FULL_CONTROL.


## Delete ACL

**From the Exoscale Console**

To delete the ACL associated with an Object, you need to edit the Object:
* Click on the `STORAGE` menu
* Navigate to the Object
* Click on the `Edit` icon of the Object.

Then click on the `Quick ACL` menu and choose `Private`. This will revert to
the default settings.

If you need to delete a specific entry, you have to

**From command line tool**

## Canned ACLs

**for Buckets**:

* private
* public-read
* public-read-write
* authenticated-read

**for Files and Folders**:

* private
* public-read
* public-read-write
* authenticated-read
* bucket-owner-read
* bucket-owner-full-control
