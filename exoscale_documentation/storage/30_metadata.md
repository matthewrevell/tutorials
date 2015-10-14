---
title: "Managing Objects Metadata"
slug: "metadata"
meta_desc: "With Metadata support, Exoscale Storage lets you define key-value pairs from our web interface or the API, and separate file metadata from the actual data."
tags: "storage"
---

SOS allows you to separate file metadata from data.
Each object contained in a bucket can be tagged with key-value pairs.
This metadata is presented through HTTP headers on SOS requests and responses.

## Add metadata

**From the Exoscale Console**

Click on the `STORAGE` menu to display the list of your buckets.
If you don't have any buckets, please follow the [Quick Start Guide](/documentation/storage/quick-start)
Then select a bucket by clicking on its name.
Choose an object to work with: a file or a folder. Click on the `Edit` icon and
then on the `Metadata` tab.

You can add key-value pairs, assigned to the Object you chose.

**From command line tool**

We assume you are using [s3cmd](http://s3tools.org/s3cmd).

The `modify` command modifies the metadata.
You can then use the option `--add-header` to add an HTTP header.
The headers starting with `x-amz-meta` will be seen as metadata.

If you want to add a metadata "foo" with value "bar", the key will be named `x-amz-meta-foo`

```
$ s3cmd modify --add-header x-amz-meta-foo:bar s3://<bucket>/<object>
modify: 's3://<bucket>/<object>'
```

## View metadata

**From the Exoscale Console**

To view the metadata associated with an Object, you need to edit the Object:
* Click on the `STORAGE` menu
* Select your bucket
* Click on the `Edit` icon of the Object.
* Click on the `Metadata` tab

**From command line tool**

We assume you are using [s3cmd](http://s3tools.org/s3cmd).

The `info` command will return information on an object, including its metadata.

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
   ACL:       None: FULL_CONTROL
   x-amz-meta-test: true
   x-amz-meta-foo: bar
```

You can see 2 key-value pairs for this object:
* `test` was set to `true`
* `foo` was set to `bar`

## Delete metadata

***From the Exoscale Console***

To delete the metadata associated with an Object, you need to edit the Object:
* Click on the `STORAGE` menu
* Select your bucket
* Click on the `Edit` icon of the Object.
* Click on the `Metadata` tab

Then, for each key-value pair, you can click on the `Delete` icon.

**From command line tool**

We assume you are using [s3cmd](http://s3tools.org/s3cmd).

The `modify` command modifies the metadata.
You can then use the option `--remove-header` to delete an HTTP header.
As the metadata start with `x-amz-meta`, if you want to remove the metadata
"foo" with value "bar", you should actually remove `x-amz-meta-foo`

```
$ s3cmd modify --remove-header x-amz-meta-foo s3://<bucket>/<object>
modify: 's3://<bucket>/<object>'
```
