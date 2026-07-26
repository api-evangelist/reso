# RESO (Real Estate Standards Organization) (reso)

RESO, the Real Estate Standards Organization, is the United States industry body that writes and certifies the machine-readable contract for residential real estate data. It publishes the RESO Data Dictionary (the field, enumeration and lookup vocabulary) and the RESO Web API (an OData 4.0/4.01 profile, Web API Core 2.0.0 ratified Jan 2021 and 2.1.0 ratified Dec 2023), plus the RESO Common Format, EntityEvent replication, Push Replication with Webhooks, Validation Expressions and the URN-based Universal Parcel Identifier (UPI). NAR Policy Statement 7.90 requires MLSs owned and operated by associations of REALTORS to implement the Data Dictionary and the Web API and to adopt new releases within one year of ratification, which makes this the only mandated machine-readable API contract in the API Evangelist sector study that is imposed by an industry body rather than a regulator. RESO itself operates no production API and holds no listing data: it certifies other people's servers. Its specifications, reference OData EDMX metadata and Data Dictionary JSON are freely and anonymously downloadable from transport.reso.org and the RESOStandards GitHub organization (a EULA click-through wraps the reso.org copies), and the certification directory at reso.org/certificates and certification.reso.org is public without login. Reachability is the separate fact: a RESO-certified endpoint is run by a local MLS, and credentials for it are issued only after a data licence with that MLS is signed, so certification here means conformance, never public access.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/reso/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/reso/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United States
- RESO
- MLS
- Property Listings
- Data Standards
- OData
- Industry Body
- IDX
- PropTech

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### RESO Web API

The RESO Web API is the ratified transport standard for real estate data, defined as a profile of OData 4.0/4.01 (Web API Core 2.0.0 and 2.1.0). Servers MUST expose an OData XML metadata document at `/$metadata` relative to their service root, MUST return JSON for data requests, MUST use TLS 1.2 or above, and MUST authenticate with OAuth2 Bearer tokens or the Client Credentials grant (RCP-026). RESO publishes the specification, the reference Data Dictionary EDMX metadata and the RESO Commander testing tool, but RESO operates **no endpoint of its own** and no base URL is documented here for that reason — every conformant server is run by an individual MLS or data provider. Access is not self-serve: RESO states that "access to data from the Web API is gained through local MLSs" and that "RESO does not provide MLS real estate data. RESO creates data standards." The `api.reso.org` host that appears in the specification text is an illustrative example only and does not resolve.

- **Human URL:** [https://www.reso.org/reso-web-api/](https://www.reso.org/reso-web-api/)
- **Base URL:** none — RESO operates no endpoint

#### Tags

- Web API
- OData
- MLS
- Transport
- Certification

#### Properties

- [Specification — RESO Web API Core (RCP-37)](https://github.com/RESOStandards/transport/blob/main/proposals/web-api-core.md)
- [Documentation](https://www.reso.org/reso-web-api/)
- [Documentation — transport.reso.org](https://transport.reso.org/)
- [Metadata — Data Dictionary 2.1 EDMX (enumerations)](openapi/reso-dd-2.1-enum.xml)
- [Metadata — Data Dictionary 2.1 EDMX (Lookup Resource)](openapi/reso-dd-2.1-lookup-resource.xml)
- [Metadata — Data Dictionary 2.0 EDMX (enumerations)](openapi/reso-dd-2.0-enum.xml)
- [Metadata — Data Dictionary 2.0 EDMX (Lookup Resource)](openapi/reso-dd-2.0-lookup-resource.xml)
- [Metadata — Data Dictionary 1.7 EDMX (enumerations)](openapi/reso-dd-1.7-enum.xml)
- [Metadata — Data Dictionary 1.7 EDMX (Lookup Resource)](openapi/reso-dd-1.7-lookup-resource.xml)
- [Data Model — Data Dictionary 2.1 (JSON)](openapi/reso-dd-2.1.json)
- [Data Model — Data Dictionary 2.0 (JSON)](openapi/reso-dd-2.0.json)
- [Data Model — Data Dictionary 1.7 (JSON)](openapi/reso-dd-1.7.json)
- [Webhooks — Push Replication with EntityEvent and Webhooks (RCP-28)](https://github.com/RESOStandards/transport/blob/main/proposals/webhooks-push.md)
- [Specification — EntityEvent Resource and Replication (RCP-27)](https://github.com/RESOStandards/transport/blob/main/proposals/entity-events.md)
- [Specification — RESO Data Dictionary (RCP-36)](https://github.com/RESOStandards/transport/blob/main/proposals/data-dictionary.md)
- [Tooling — RESO Commander](https://github.com/RESOStandards/web-api-commander)
- [Reference Implementation — RESO Web API reference server](https://github.com/RESOStandards/reso-web-api-reference-server)
- [Certification — public directory of certified organizations](https://www.reso.org/certificates/)

## Common Properties

- [Website](https://www.reso.org/)
- [About](https://www.reso.org/about-reso/)
- [Documentation — transport.reso.org](https://transport.reso.org/)
- [Specification directory](https://www.reso.org/specs/)
- [Specification — RESO Common Format (RCP-25)](https://github.com/RESOStandards/transport/blob/main/proposals/reso-common-format.md)
- [Specification — Web API Validation Expressions (RCP-19)](https://github.com/RESOStandards/transport/blob/main/proposals/validation-expressions.md)
- [Data Dictionary](https://www.reso.org/data-dictionary/)
- [Data Dictionary Wiki](https://dd.reso.org/)
- [Universal Parcel Identifier (UPI) 2.0](https://www.reso.org/universal-parcel-identifier/)
- [UPI Builder](https://upi.reso.org/builder/)
- [Certification](https://www.reso.org/certification/)
- [RESO Analytics certification portal](https://certification.reso.org/)
- [Certification fee schedule](https://www.reso.org/certification-fee-schedule/)
- [Membership](https://www.reso.org/membership/)
- [EULA](https://www.reso.org/eula/)
- [GitHub Organization](https://github.com/RESOStandards)
- [Blog](https://www.reso.org/blog/)
- [Blog RSS](https://www.reso.org/feed/)
- [NAR Policy Statement 7.90](https://www.nar.realtor/handbook-on-multiple-listing-policy/operational-issues-section-12-real-estate-transaction-standards-rets-policy-statement-790)

## Access Posture

| Question | Answer |
| --- | --- |
| RESO certified? | No — RESO is the certifying body, not a certified party |
| Access gate | Membership required (to certify); the specifications themselves are free and anonymous |
| Self-serve developer portal | No — `developer.reso.org`, `developers.reso.org`, `api.reso.org` and `docs.reso.org` all fail DNS |
| Open data | No — open specifications, zero open data |
| Auth model | Standard mandates OAuth2 Bearer token or Client Credentials over TLS 1.2+; credentials are issued by each MLS, never by RESO |
| Machine-readable contract | OData CSDL/EDMX `$metadata`, not OpenAPI — nine artifacts harvested into `openapi/` |
| To call real listing data | Join or contract with a specific local MLS and sign its data use and licensing policy (IDX, VOW, broker sponsorship, or a reseller such as Bridge, Trestle, MLS Grid or Spark) |

Full evidence, probe results and HTTP statuses are recorded in [review.yml](review.yml).
