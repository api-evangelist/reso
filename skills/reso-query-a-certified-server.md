---
name: Query a RESO-certified Web API server
description: >-
  Authenticate to any RESO-certified MLS OData endpoint and read listing, member or media records
  correctly - discovering the schema from $metadata first, then paging with OData query options.
api: RESO Web API Core 2.1.0 (RCP-37)
contract: openapi/reso-dd-2.1-enum.xml
operations:
  - GET {service-root}/$metadata
  - GET {service-root}/{Resource}
  - POST {token-url} (OAuth2 client_credentials)
mcp_tools: [authenticate, metadata, query, parse-filter]
generated: '2026-07-26'
method: generated
source:
  - https://github.com/RESOStandards/transport/blob/main/proposals/web-api-core.md
  - https://github.com/RESOStandards/reso-tools/blob/main/reso-mcp-server/doc/GUIDE.md
---

# Query a RESO-certified Web API server

## Before you start — the access reality

RESO operates no listing endpoint and issues no Web API credentials. Every RESO-certified server is
run by an individual MLS or data provider, and credentials are issued only after a data licence with
that MLS is signed. `api.reso.org` appears in the specification as an illustrative example host and
does not resolve. If you do not already have a service root and credentials, this skill cannot get
them for you — say so plainly instead of guessing a hostname.

For development, stand up the RESO reference server locally instead (`sandbox/reso-sandbox.yml`).

## 1. Authenticate

The standard permits exactly two mechanisms (Web API Core, RCP-026), over TLS 1.2 or above:

- **OAuth2 Bearer token** — you were issued a token; send `Authorization: Bearer <token>`.
- **OAuth2 Client Credentials** — POST `client_id` + `client_secret` to the MLS's token URL and use
  the returned bearer token.

OpenID Connect is *not* supported — it was removed in Web API 1.0.2.

Via MCP: `authenticate({ clientId, clientSecret, tokenUrl })`, or pass `authToken` directly on any
tool call.

A failed auth mechanism returns **403 Forbidden**, not 401.

## 2. Read the schema before you read data

Never assume field names. Servers MUST expose an OData XML metadata (EDMX) document:

```
GET {service-root}/$metadata
```

Via MCP: `metadata({ url, authToken, resource? })`.

Field names are **case sensitive** in `$select`, `$filter` and `$orderby`, and you must use the case
declared in that server's metadata. Local (non-standard) fields are permitted, so the server's
metadata — not the Data Dictionary — is the authority on what exists on *this* server.

The standard resource set is in `data-model/reso-data-model.yml` (44 resources in DD 2.1). At least
one of Property, Member, Office or Media is present on any certified server.

## 3. Query

```
GET {service-root}/Property?$select=ListingKey,ListPrice,City,StandardStatus
                           &$filter=ListPrice ge 200000 and StandardStatus eq 'Active'
                           &$orderby=ModificationTimestamp asc
                           &$top=5&$count=true
```

Via MCP: `query({ url, resource, authToken, filter, select, orderby, top, skip, count, expand })`.

Servers MUST support `$select`, `$filter`, `$orderby`, `$top`, `$skip` and `$count`. `$expand` is a
Web API Core **2.1.0** addition — check the server's declared version before relying on it.

Response shape:

```json
{ "@odata.context": "...", "@odata.count": 1234, "value": [ { "ListingKey": "..." } ] }
```

Before sending a complex filter, sanity-check it with `parse-filter({ filter })` — it returns the
AST, so a malformed expression is caught locally instead of as a 400.

## 4. Page correctly

Combine `$top` and `$skip`. Two rules that bite:

- Each server decides how far it will let you skip; an unsupported `$skip` comes back as a 501 in
  the OData error envelope with `target: "$skip"`.
- A compliant server is **not** required to return consistent results between requests. Always add
  `$orderby` (conventionally `ModificationTimestamp asc`) to keep paging stable.

For anything larger than a browse, do not page with `$skip` at all — use a timestamp window on
`ModificationTimestamp`, or the EntityEvent feed (see the replication skill).

## 5. Handle enumerations

Two lookup styles exist in the wild:

- `Edm.EnumType` — legacy, requires knowing the vendor's namespace. Being deprecated.
- `Edm.String` / `Collection(Edm.String)` with the Lookup Resource (RCP-032) — current, uses
  human-friendly values. Prefer this for new work.

Read `$metadata` to see which style this server uses; `query` the `Lookup` resource to enumerate
allowed values rather than hardcoding them.

## 6. Errors

Errors are the **OData JSON error envelope**, not RFC 9457 problem+json:

```json
{ "error": { "code": "501", "message": "Unsupported functionality", "target": "query",
             "details": [ { "code": "501", "target": "$skip", "message": "..." } ] } }
```

| Status | Meaning | Do |
|---|---|---|
| 400 | Query failed validation | Read `details[]`, fix the offending option, retry |
| 403 | Auth mechanism not successful | Re-issue the token / re-run client credentials |
| 404 | Resource or collection not found | Confirm the name against `$metadata`; do not retry |
| 413 | `$filter`/`$orderby` too complex | Simplify or split the query |
| 429 | Licensee usage exceeded | Back off; limits live in the MLS data licence, not the standard |
| 501 | Method not available | The server does not implement that capability |

Full catalogue: `errors/reso-problem-types.yml`. Conventions: `conventions/reso-conventions.yml`.
