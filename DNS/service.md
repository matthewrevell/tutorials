---
title: Exoscale DNS
date: 2014-12-11 09:52 +01:00
category: DNS
tags: dns
---

Our integrated DNS service, built in cooperation with DNSimple provides a
simple way to host your zones.

## Subscribing to the service

If you wish to subscribe to exoscale DNS you only need to select the
zone bundle which best suits you from within our portal. The up-to-date list of
plans is available at https://exoscale.ch/add-on/dns

## Graphical interface

Once you have selected a bundle, you'll be presented with our interface
within the exoscale portal.

We're confident you'll our interface intuitive and won't get lost, if you
do be sure to contact us through our support interface!

## Registering domains

Exoscale provides no way to register domains and assumes you already have
bought domains from an existing registrar. Once you have configured your zone
- either through our API or interface - you'll need to point the adjust the
NS servers for your zone with your registrar.

## API access

The key feature of exoscale DNS resides in its simple JSON API, which
allows you to program hosted zones and records

### API authentication

There are two levels of authentication:

- A global authentication which allows listing registered zones
- A per-zone authentication realm

The API endpoint lives at https://api.exoscale.ch/dns and authentication
is done through the use of the `X-DNS-Token` for the global realm or `X-DNS-Domain-Token`
for the per-zone realm.

The `X-DNS-Token` follows this simple layout: `API_KEY:SHA1(API_SECRET)`, your standard
exoscale API Key, a colon and the hexadecimal representation of your API secret's SHA1.

The `X-DNS-Domain-Token` follows a similar layout: `API_KEY:ZONE_TOKEN`, instead of
a fixed token, you'll be expected to supply the per-zone token retrieved via the
`GET /domains` call.

### JSON API

#### Domain API

*GET /domains*

list all domains

example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     https://api.exoscale.ch/dns/v1/domains
```

example response:

```json
[
  {
    "domain": {
      "id": 228,
      "user_id": 19,
      "registrant_id": null,
      "name": "example.it",
      "unicode_name": "example.it",
      "token": "domain-token",
      "state": "hosted",
      "language": null,
      "lockable": true,
      "auto_renew": false,
      "whois_protected": false,
      "record_count": 5,
      "service_count": 0,
      "expires_on": null,
      "created_at": "2014-01-15T22:03:49Z",
      "updated_at": "2014-01-15T22:03:49Z"
    }
  },
  {
    "domain": {
      "id": 227,
      "user_id": 19,
      "registrant_id": 28,
      "name": "example.com",
      "unicode_name": "example.com",
      "token": "domain-token",
      "state": "registered",
      "language": null,
      "lockable": true,
      "auto_renew": true,
      "whois_protected": false,
      "record_count": 7,
      "service_count": 0,
      "expires_on": "2015-01-16",
      "created_at": "2014-01-15T22:01:55Z",
      "updated_at": "2014-01-16T22:56:22Z"
    }
  }
]
```

*POST /domains*

Create a domain, expected input params:

- `domain.name`: zone to host


example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     -H 'Content-Type: application/json' \
     -d '{"domain":{"name":"example.com"}}' \
     -X POST \
     https://api.exoscale.ch/dns/v1/domains
```

example response

```json
  {
    "domain": {
      "id": 227,
      "user_id": 19,
      "registrant_id": 28,
      "name": "example.com",
      "unicode_name": "example.com",
      "token": "domain-token",
      "state": "registered",
      "language": null,
      "lockable": true,
      "auto_renew": true,
      "whois_protected": false,
      "record_count": 7,
      "service_count": 0,
      "expires_on": "2015-01-16",
      "created_at": "2014-01-15T22:01:55Z",
      "updated_at": "2014-01-16T22:56:22Z"
    }
  }
```

*GET /domains/:domain*

Retrieve a domain's details, by ID or name

example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     https://api.exoscale.ch/dns/v1/domains/227
```
example response:

```json
  {
    "domain": {
      "id": 227,
      "user_id": 19,
      "registrant_id": 28,
      "name": "example.com",
      "unicode_name": "example.com",
      "token": "domain-token",
      "state": "registered",
      "language": null,
      "lockable": true,
      "auto_renew": true,
      "whois_protected": false,
      "record_count": 7,
      "service_count": 0,
      "expires_on": "2015-01-16",
      "created_at": "2014-01-15T22:01:55Z",
      "updated_at": "2014-01-16T22:56:22Z"
    }
  }
```

*DELETE /domains/:domain*

Delete a domain by ID or name.

example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     -X DELETE \
     https://api.exoscale.ch/dns/v1/domains/227
```

*POST /domains/:domain/token*

Resets a domain's token

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     -X POST \
     https://api.exoscale.ch/dns/v1/domains/227/token
```

example response:

```json
  {
    "domain": {
      "id": 227,
      "user_id": 19,
      "registrant_id": 28,
      "name": "example.com",
      "unicode_name": "example.com",
      "token": "domain-token",
      "state": "registered",
      "language": null,
      "lockable": true,
      "auto_renew": true,
      "whois_protected": false,
      "record_count": 7,
      "service_count": 0,
      "expires_on": "2015-01-16",
      "created_at": "2014-01-15T22:01:55Z",
      "updated_at": "2014-01-16T22:56:22Z"
    }
  }
```

*GET /domains/:domain/records*

List records for a domain.

Accepts the following params:

- `domain`: Domain name or id
- `name`: The name to search for
- `type`: The record type to search for

example query:

~~~
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records
~~~

~~~json
[
  {
    "record": {
      "name": "",
      "ttl": 3600,
      "created_at": "2010-07-04T04:41:31Z",
      "updated_at": "2010-10-21T15:47:47Z",
      "domain_id": 1,
      "id": 31,
      "content": "1.2.3.4",
      "record_type": "A",
      "prio": null
    }
  }
]
~~~

*POST /domains/:domain/records*

Create a new record.
Accepts the following params:

- `domain`: Domain name or id

example query:

    curl  -H 'X-DNS-Token: <token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X POST \
          -d '<json>' \
          https://api.exoscale.ch/dns/v1/domains/example.com/records


example response:

```json
  {
    "record": {
      "name": "",
      "ttl": 3600,
      "created_at": "2010-07-04T04:41:31Z",
      "updated_at": "2010-10-21T15:47:47Z",
      "domain_id": 1,
      "id": 31,
      "content": "1.2.3.4",
      "record_type": "A",
      "prio": null
    }
  }
```

*GET /domains/:domain/records/:id*

Retrieve record details.
Accepts the following params:

- `domain`: Domain name or ID
- `id`: Record ID

example query:

    curl  -H 'X-DNS-Token: <token>' \
          -H 'Accept: application/json' \
          https://api.exoscale.ch/dns/v1/domains/example.com/records/1
          
example response:

```json
  {
    "record": {
      "name": "",
      "ttl": 3600,
      "created_at": "2010-07-04T04:41:31Z",
      "updated_at": "2010-10-21T15:47:47Z",
      "domain_id": 1,
      "id": 31,
      "content": "1.2.3.4",
      "record_type": "A",
      "prio": null
    }
  }
```

*PUT /domains/:domain/records/:id*

Modify a record's content.
Accepts the following params:

- `record.name`: record name
- `record.content`: record content
- `record.ttl`: optional record TTL
- `record.prio`: record priority when applicable


example query:

    curl  -H 'X-DNS-Token: <token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X PUT \
          -d '<json>' \
          https://api.exoscale.ch/dns/v1/domains/example.com/records/1

example response:

```json
  {
    "record": {
      "name": "",
      "ttl": 3600,
      "created_at": "2010-07-04T04:41:31Z",
      "updated_at": "2010-10-21T15:47:47Z",
      "domain_id": 1,
      "id": 31,
      "content": "1.2.3.4",
      "record_type": "A",
      "prio": null
    }
  }
```

*DELETE /domain/:domain/records/:id*

Delete a record

example query:

    curl  -H 'X-DNS-Token: <token>' \
          -H 'Accept: application/json' \
          -X DELETE \
          https://api.exoscale.ch/dns/v1/domains/example.com/records/1

