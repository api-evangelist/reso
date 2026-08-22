# RESO (Real Estate Standards Organization) (reso)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
