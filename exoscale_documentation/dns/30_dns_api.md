---
title: "DNS API documentation"
slug: "api"
meta_desc: "Documentation for the Exoscale DNS API. Learn how to authenticate, get domain information, configure your zone, add, update and delete records from the API."
category: "dns"
tags: "dns"
---

The key feature of Exoscale DNS resides in its simple JSON API, which
allows you to program hosted zones and records.

### API authentication

There are two levels of authentication:

- A global authentication which allows listing registered zones
- A per-zone authentication realm

The API endpoint lives at https://api.exoscale.ch/dns and authentication
is done through the use of the `X-DNS-Token` header for the global realm or
`X-DNS-Domain-Token` header for the per-zone realm.

The `X-DNS-Token` header follows this simple layout: `API_KEY:API_SECRET`.
Your standard Exoscale API Key and secret, separated with a colon.

The `X-DNS-Domain-Token` follows a similar layout: `API_KEY:ZONE_TOKEN`.
Instead of a fixed token, you'll be expected to supply the per-zone token
retrieved via the `GET /domains` call.

### JSON API

#### Domain API

*GET /domains*

List all domains

Example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     https://api.exoscale.ch/dns/v1/domains
```

Example response:

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

Create a domain. Expected input params:

- `domain.name`: zone to host

Example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     -H 'Content-Type: application/json' \
     -d '{"domain":{"name":"example.com"}}' \
     -X POST \
     https://api.exoscale.ch/dns/v1/domains
```

Example response:

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

Retrieve a domain's details, by ID or name.

Example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     https://api.exoscale.ch/dns/v1/domains/227
```

Example response:

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

Example query:

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     -X DELETE \
     https://api.exoscale.ch/dns/v1/domains/227
```

*POST /domains/:domain/token*

Resets a domain's token.

```bash
curl -H 'X-DNS-Token: <token>' \
     -H 'Accept: application/json' \
     -X POST \
     https://api.exoscale.ch/dns/v1/domains/227/token
```

Example response:

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

Example query:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records
```

Example response:

```json
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
```

*POST /domains/:domain/records*

Create a new record. Accepts the following params:

- `domain`: Domain name or id

The JSON input accepts the following params:

- `record.name`: record name, **required*
- `record.record_type`: record type (A, CNAME, MX, ...), **required**
- `record.content`: record content **required**
- `record.ttl`: optional record TTL
- `record.prio`: record priority when applicable

Example query:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      -H 'Content-Type: application/json' \
      -X POST \
      -d '<json>' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records
```

```json
{
  "record": {
    "name": "",
    "record_type": "MX",
    "content": "mail.example.com",
    "ttl": 3600,
    "prio": 10
  }
}
```


Example response:

```json
  {
    "record": {
      "name": "",
      "ttl": 3600,
      "created_at": "2010-07-04T04:41:31Z",
      "updated_at": "2010-10-21T15:47:47Z",
      "domain_id": 1,
      "id": 31,
      "content": "mail.example.com",
      "record_type": "MX",
      "prio": 10
    }
  }
```

*GET /domains/:domain/records/:id*

Retrieve record details. Accepts the following params:

- `domain`: Domain name or ID
- `id`: Record ID

Example query:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records/1
```

Example response:

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

Modify a record's content. Accepts the following params:

- `record.name`: record name
- `record.content`: record content
- `record.ttl`: optional record TTL
- `record.prio`: record priority when applicable

Example query:

```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      -H 'Content-Type: application/json' \
      -X PUT \
      -d '<json>' \
      https://api.exoscale.ch/dns/v1/domains/example.com/records/1
```

Example response:

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

Delete a record.

Example query:
```bash
curl  -H 'X-DNS-Token: <token>' \
      -H 'Accept: application/json' \
      -X DELETE \
      https://api.exoscale.ch/dns/v1/domains/example.com/records/1
```
