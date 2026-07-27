---
name: Certify a server against RESO standards
description: >-
  Take a RESO Web API implementation through the four compliance suites RESO tests - Data Dictionary,
  Web API Core, Add/Edit and EntityEvent - using the first-party reso-cert CLI or the MCP
  run-compliance tool, and understand what the resulting endorsement does and does not mean.
api: RESO Certification (Data Dictionary 2.0, Web API Core 2.0.0/2.1.0, Add/Edit RCP-010, EntityEvent RCP-027)
contract: openapi/reso-dd-2.1.json
operations:
  - reso-cert dd
  - reso-cert core
  - reso-cert add-edit
  - reso-cert entity-event
  - GET {service-root}/$metadata
mcp_tools: [run-compliance, metadata-report, authenticate]
generated: '2026-07-26'
method: generated
source:
  - https://github.com/RESOStandards/reso-tools/blob/main/reso-certification/README.md
  - https://transport.reso.org/policy-changes/
  - https://www.reso.org/certification/
---

# Certify a server against RESO standards

## What certification is, and is not

A RESO endorsement proves your server **conforms** to an OData contract. It says nothing about
whether anyone can reach your data — access is governed by the data licence you sign with each
consumer. Do not describe a certified endpoint as "open".

NAR Policy Statement 7.90 makes this a clock, not a badge: MLSs owned and operated by associations
of REALTORS must implement the Data Dictionary and Web API and adopt new releases **within one year
of ratification**. As of the December 2025 RESO Board policy, **endorsements expire after two
years**, and RESO always certifies on the latest minor version in a major branch.

## 1. Test locally first

Stand up the reference server and run the suites against it so you learn the tooling on a server you
control:

```bash
cd reso-reference-server
docker compose up -d
docker compose --profile seed up seed
docker compose --profile compliance-core up --build --exit-code-from compliance-core
```

Details: `sandbox/reso-sandbox.yml`.

## 2. Point the CLI at your server

```bash
npm install -g @reso-standards/reso-certification

reso-cert dd           --url https://api.yourmls.com --auth-token TOKEN
reso-cert core         --url https://api.yourmls.com --auth-token TOKEN
reso-cert add-edit     --url https://api.yourmls.com --auth-token TOKEN
reso-cert entity-event --url https://api.yourmls.com --auth-token TOKEN --mode observe
```

Auth resolves in order: CLI flags → `--config` file → `.env` → environment variables
(`RESO_AUTH_TOKEN`, or `RESO_CLIENT_ID` / `RESO_CLIENT_SECRET` / `RESO_TOKEN_URI`).

Exit codes: `0` all scenarios passed, `1` one or more failed, `2` runtime error — so this drops
straight into CI. Use `--verbose` for CI logs and `--output json --output-dir ./results` for
machine-readable reports.

Via MCP: `run-compliance({ endorsement, url, authToken, resource?, version?, mode?, resources? })`
and `metadata-report({ url, authToken })`. The CLI, the desktop client and the MCP server all call
the same SDK functions, so results are identical whichever surface you use.

## 3. What each suite checks

| Suite | Endorsement | What it validates |
|---|---|---|
| `dd` | Data Dictionary 2.0 | Server metadata and data availability against the Data Dictionary; merges Lookup Resource data, checks variations, replicates data using multiple strategies. `--strict` tightens it. |
| `core` | Web API Core 2.0.0 / 2.1.0 | `$filter` across all data types, `$select`, `$orderby`, `$top`, `$skip`, `$count`, enumerations, error codes. 2.1.0 adds `$expand`, server-driven paging, string-based enum comparison. |
| `add-edit` | RCP-010 | Create, update and delete with `Prefer: return=representation` / `return=minimal` and error handling. 8 scenarios. |
| `entity-event` | RCP-027 | Change tracking in `observe` (read-only) or `full` (canary writes) mode. 9–12 scenarios. |

## 4. Fix the metadata before the data

Most failures are metadata failures. Generate the report first:

```bash
reso-cert metadata-report adapt --in metadata-report.json --out adapted.json --pretty
```

Common causes, straight from the standard:

- **Field name case** — names are case sensitive and must match your own `$metadata`.
- **Enumeration style** — `Edm.EnumType` still passes but is deprecated; migrate to `Edm.String` /
  `Collection(Edm.String)` with the Lookup Resource (RCP-032). `IsFlags=true` is capped at 64 values
  and is on the way out.
- **Every EntityType MUST define a Key.**
- **Missing `$metadata`** — servers MUST expose the EDMX document at the service root.
- **Version negotiation** — an older unsupported or newer-than-supported version MUST return 400.

## 5. Know what counts as breaking

Before you change a field, check `https://transport.reso.org/versioning/`. RESO treats these as
MAJOR (breaking) in the Data Dictionary: a standard field type change; adding a disallowed synonym;
closing an existing enumeration or removing items from a closed one; adding duplicate or replacement
data elements; adding new testing rules to existing data elements. Full model:
`lifecycle/reso-lifecycle.yml`.

## 6. Submit

Certification is run by RESO, complimentary for members (non-members: the published fee schedule).
You submit access credentials for the system under test. Results land in the public directory at
`https://www.reso.org/certificates/` and the RESO Analytics portal at
`https://certification.reso.org/`.
