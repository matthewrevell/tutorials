---
title: "Using the DNS through the API"
slug: "api-examples"
meta_desc: "Learn how to use the Exoscale DNS API to configure your DNS zone. Set up records pointing to your server, using curl to send JSON data via an HTTP request"
tags: "dns"
---
The key feature of Exoscale DNS resides in its simple
[JSON API](/documentation/dns/api), which allows you to program hosted zones
and records.

We assume that you've already chosen a zone bundle. If that's not the case,
[here's how to select one.](/documentation/dns/quick-start)

Authentication is done through the use of the `X-DNS-Token`. The `X-DNS-Token` header follows this simple layout: `API_KEY:API_SECRET`.

In the following examples, *<token>* should be replaced by your `API_KEY:API_SECRET`.

## Configuring a zone with the API

Create the `example.com` zone:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     -H 'Content-Type: application/json' \
     -d '{"domain":{"name":"example.com"}}' \
     -X POST \
     https://api.exoscale.ch/dns/v1/domains
```

View the zone:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     https://api.exoscale.ch/dns/v1/domains
```

## Configuring a machine record

To configure a record, we need to pass some JSON to our curl request.

We want to create an A record named 'web' pointing to the IP 1.2.3.4:

```json
{
  "record":
  {
    "name": "web",
    "record_type": "A",
    "content": "1.2.3.4",
    "ttl": 3600
  }
}
```

The request will be:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      -H 'Content-Type: application/json' \
      -X POST \
      -d '{"record":{"name": "web","record_type": "A","content": "1.2.3.4","ttl": 3600}}' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records
```

## Configuring a alias record
We want to create a CNAME record named 'www' pointing to 'web.example.com':

```json
{
  "record":
  {
    "name": "www",
    "record_type": "CNAME",
    "content": "web.example.com",
    "ttl": 3600
  }
}
```

The request will be:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      -H 'Content-Type: application/json' \
      -X POST \
      -d '{"record":{"name": "www","record_type": "CNAME","content": "web.example.com","ttl": 3600}}' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records
```

## Configuring a Mail Exchange record

We want to create an MX record pointing to 'mail.example.com':

```json
{
  "record":
  {
    "name": "",
    "record_type": "MX",
    "content": "mail.example.com",
    "ttl": 3600,
    "prio": 10
  }
}
```

The request will be:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      -H 'Content-Type: application/json' \
      -X POST \
      -d '{"record":{"name": "","record_type": "MX","content": "mail.example.com","ttl": 3600,"prio":10}}' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records
```

## Configure my domain to point to an IP

We want the domain 'example.com' to point to '1.2.3.4':

```json
{
  "record":
  {
    "name": "",
    "record_type": "A",
    "content": "1.2.3.4",
    "ttl": 3600
  }
}
```

The request will be:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      -H 'Content-Type: application/json' \
      -X POST \
      -d '{"record":{"name": "","record_type": "A","content": "1.2.3.4","ttl": 3600}}' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records
```
