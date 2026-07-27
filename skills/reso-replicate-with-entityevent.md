---
name: Replicate a RESO feed with EntityEvent and webhooks
description: >-
  Keep a local copy of an MLS's listing data current using the ratified RESO change-tracking model -
  the EntityEvent append-only log (RCP-27) for pull, and Push Replication with Webhooks (RCP-28) for
  push - instead of paging the whole Property resource.
api: RESO EntityEvent Resource and Replication 2.0.2 (RCP-27) + Push Replication with Webhooks 1.0.1 (RCP-28)
contract: openapi/reso-dd-2.1-enum.xml
operations:
  - GET {service-root}/EntityEvent
  - GET {service-root}/{Resource}('{Key}')
  - POST {consumer-endpoint} (producer push, RCP-28)
mcp_tools: [authenticate, query]
generated: '2026-07-26'
method: generated
source:
  - https://github.com/RESOStandards/transport/blob/main/proposals/entity-events.md
  - https://github.com/RESOStandards/transport/blob/main/proposals/webhooks-push.md
---

# Replicate a RESO feed with EntityEvent and webhooks

## Why not just page the Property resource

`$top`/`$skip` paging over a full listing set is fragile: a compliant server is not required to
return consistent results between requests, and skip depth is capped at the server's discretion.
RESO's answer is an append-only change log — the **EntityEvent** resource — plus an optional push
channel over it.

## 1. Pull mode — poll the EntityEvent resource

`EntityEvent` is an ordinary Data Dictionary resource, so it answers to the same OData options as
any other:

```
GET {service-root}/EntityEvent?$filter=EntityEventSequence gt {last_seen}
                              &$orderby=EntityEventSequence
                              &$top=1000
```

Via MCP: `query({ url, resource: "EntityEvent", authToken, filter, orderby, top })`.

Each event identifies a change as the tuple:

- `EntityEventSequence` — monotonic sequence number; this is your cursor
- `ResourceName` — which Data Dictionary resource changed
- `ResourceRecordKey` — the key of the changed record

The event tells you *what* changed, not the new values. Fetch the record itself:

```
GET {service-root}/{ResourceName}('{ResourceRecordKey}')
```

**Persist the highest `EntityEventSequence` you have fully processed**, and resume strictly from it.
Never resume from a wall-clock time.

## 2. Push mode — receive webhooks (RCP-28)

The direction is inverted from most webhook APIs: **you (the consumer) implement the endpoint**, and
the MLS (the producer) POSTs to it. RESO sends nothing; it specifies the exchange.

```
POST https://your-api.example.com/webhooks
Authorization: Bearer <credentials the producer was given>
Content-Type: application/json

{
  "@reso.context": "urn:reso:metadata:2.0:resource:entityevent",
  "EntityEventSequence": 1101,
  ...
}
```

Requirements from the specification:

- Consumers **MUST** implement an endpoint that can receive EntityEvents.
- Producers **MUST** use HTTP POST with an EntityEvent payload and the given credentials.
- Payloads are **RESO Common Format** (RCP-25) and may be single-valued or batched.
- Producers **SHOULD** also host the EntityEvent resource so you can replay events you missed.

### Backpressure

Return `Retry-After` (a date or a number of seconds) and the producer waits that long before
resuming. If a producer fails after some number of retries it **MAY stop the flow entirely** — and
you may then have to POST to the producer's endpoint to restart the feed. Treat repeated 5xx from
your own endpoint as an outage that can silently cost you the feed.

**Accept and queue events; do not block while processing them.** This matters most when a feed is
first initialised and a large backlog arrives at once.

### Optional feed labelling

Producers may send an `EntityEventSource` header (String, 255) — a consumer-assigned label such as
`IDX Feed 123` — so you can tell which of your feeds a batch belongs to. It is the only
RESO-defined correlation header.

## 3. Reconcile

Because events carry keys rather than payloads, your pipeline is: read events in sequence order →
dedupe by `(ResourceName, ResourceRecordKey)` within the batch → fetch the current record for each →
upsert → advance the cursor. Deletes surface as events too; verify by re-reading the record and
treating a 404 as confirmation.

## 4. Prove it before you ship it

Run the ratified compliance suite against the server you are integrating with:

```
reso-cert entity-event --url https://api.example.com --auth-token TOKEN --mode observe
```

`observe` is read-only (9 scenarios); `full` adds canary writes (up to 12). See `cli/reso-cli.yml`.
To rehearse the whole loop locally, enable EntityEvent on the RESO reference server
(`sandbox/reso-sandbox.yml`).

## Coming

RCP-49 (EntityEvent Subscriptions and Filtering) is in review — it would let a consumer subscribe to
a filtered slice of the feed rather than the whole log. Not ratified; do not build against it yet.
