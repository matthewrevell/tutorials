---
title: "Simple Object Storage CORS"
slug: "cors"
meta_desc: "Use Cross-Origin Resource Sharing (CORS) to access your buckets from your browser-based applications and websites. Configure CORS from the interface or API"
tags: "storage"
---

SOS's CORS (Cross-Origin Resource Sharing) support lets you configure access
to your buckets from browser-based applications and websites. CORS rules are
applied to entire buckets and all the objects they contain.

## CORS configuration format

A CORS configuration is an XML document containing a set of *rules*. Each rule defines a set of allowed HTTP origins, methods, and headers. Here is an example CORS configuration:

	<CORSConfiguration>
	  <CORSRule>
	    <AllowedOrigin>https://example.com</AllowedOrigin>
	    <AllowedMethod>*</AllowedMethod>
	    <AllowedHeader>Content-*</AllowedHeader>
	  </CORSRule>
	  <CORSRule>
	    <AllowedOrigin>*</AllowedOrigin>
	    <AllowedMethod>HEAD</AllowedMethod>
	    <AllowedMethod>GET</AllowedMethod>
	  </CORSRule>
	</CORSConfiguration>

This configuration allows all HTTP methods from the `https://example.com`
origin and all headers starting with `Content-`. For all other origins, the
`GET` method is allowed.

You can use wildcards (`*`) in allowed **origins**, **methods** and
**headers**. Wildcards in allowed **headers** are limited to prefix matches.

A single rule can contain mulitple `<AllowedOrigin>`, `<AllowedMethod>` and
`<AllowedHeader>` elements.

### AllowedMethod

`<AllowedMethod>` supports the following values:

* HEAD
* GET
* POST
* PUT
* DELETE

### AllowedOrigin

`<AllowedOrigin>` specifies the origins from which you want to allow
cross-domain requests. You can choose an exact match (e.g.
`https://example.com`) or a match with the wildcard character (e.g.
`https://*.example.com`). Simply setting `<AllowedOrigin>` to `*` allows all
origins.

### Additional elements

Each `<CORSRule>` element additionally supports the following elements:

* `<MaxAgeSeconds>`: this controls the browser's cache for the OPTIONS
  response. Caching response helps the browser to avoid making repeated
  OPTIONS calls if the original request is being repeated. To set a 5-minute
  cache, set `<MaxAgeSeconds>` to `300`.

* `<ExposeHeader>`: comma-separated list of headers that SOS can send back in
  its responses, making them available to your JavaScript application code.

## Managing CORS configuration in the Exoscale Console

CORS configuration can be edited in the Exoscale console. Simply navigate to
the bucket list and click the "Edit" button next to your bucket. In the "CORS"
tab, paste your XML document.

## Managing CORS configuration within the SOS API

The API endpoint for CORS management is
`https://<bucket-name>.sos.exo.io/?cors`. See the Pithos documentation for
details:

* [GET Bucket CORS](http://pithos.io/api.html#get-bucket-cors)
* [PUT Bucket CORS](http://pithos.io/api.html#put-bucket-cors)
* [DELETE Bucket CORS](http://pithos.io/api.html#delete-bucket-cors)
