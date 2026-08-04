# Cloud Foundry (cloud-foundry)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Cloud Foundry is an open-source, multi-cloud Platform as a Service (PaaS) governed by the Cloud Foundry Foundation. It provides a developer-friendly application platform where operators push source code or container images and Cloud Foundry handles staging, routing, scaling, and lifecycle management. The CF API (api.cloudfoundry.org) is the primary control plane and is documented at v3.cloudfoundry.org/version/release-candidate. The ecosystem also includes the User Account and Authentication (UAA) OAuth 2.0 server, the Loggregator log and metric pipeline, the Diego container scheduler, the Open Service Broker API for marketplace services, and the Eirini Kubernetes-based scheduler.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/cloud-foundry/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/cloud-foundry/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Cloud Foundry Foundation
- Containers
- Multi-Cloud
- Open Source
- PaaS
- Platform

## Timestamps

- **Created:** 2024-01-01
- **Modified:** 2026-04-23

## APIs

### Cloud Foundry Cloud Controller API v3

The Cloud Controller API v3 is the primary REST control plane for Cloud Foundry. It manages organizations, spaces, applications, processes, builds, droplets, packages, routes, domains, service instances, service brokers, tasks, users, roles, and platform metadata. Authentication is delegated to UAA via OAuth 2.0 bearer tokens.

- **Human URL:** [https://v3-apidocs.cloudfoundry.org/](https://v3-apidocs.cloudfoundry.org/)

#### Tags

- Apps
- PaaS
- Platform
- REST

#### Properties

- [Documentation](https://v3-apidocs.cloudfoundry.org/)
- [Source  Code](https://github.com/cloudfoundry/cloud_controller_ng)
- [OpenAPI](openapi/cloud-foundry-cloud-controller-api-v3-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Cloud Foundry UAA

The User Account and Authentication (UAA) server is Cloud Foundry's identity provider and OAuth 2.0 authorization server. It issues tokens consumed by the Cloud Controller, brokers, and operator tooling, and supports SAML and LDAP federation, multifactor authentication, and SCIM-style user and group management.

- **Human URL:** [https://docs.cloudfoundry.org/uaa/](https://docs.cloudfoundry.org/uaa/)

#### Tags

- Authentication
- Identity
- OAuth 2.0
- Security

#### Properties

- [Documentation](https://docs.cloudfoundry.org/uaa/)
- [Source  Code](https://github.com/cloudfoundry/uaa)
- [Reference](https://docs.cloudfoundry.org/api/uaa/)
- [Postman Collection](collections/cloud-foundry.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cloud-foundry.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Cloud Foundry Loggregator

Loggregator is Cloud Foundry's distributed log and metric pipeline that aggregates application logs, platform component logs, and metrics for streaming consumption by users and external sinks. It exposes a WebSocket Firehose, a v2 gRPC egress API, and the Reverse Log Proxy.

- **Human URL:** [https://docs.cloudfoundry.org/loggregator/](https://docs.cloudfoundry.org/loggregator/)

#### Tags

- Logs
- Metrics
- Observability
- Streaming

#### Properties

- [Documentation](https://docs.cloudfoundry.org/loggregator/)
- [Source  Code](https://github.com/cloudfoundry/loggregator-release)
- [AsyncAPI](asyncapi/cloud-foundry-loggregator-asyncapi.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/latest)
- [Postman Collection](collections/cloud-foundry.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cloud-foundry.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Open Service Broker API

The Open Service Broker API is the open specification originally developed by Cloud Foundry and now used by Kubernetes and other platforms to provision and manage backing services through a common HTTP interface. Brokers expose a catalog and lifecycle operations for service instances and bindings.

- **Human URL:** [https://www.openservicebrokerapi.org/](https://www.openservicebrokerapi.org/)

#### Tags

- Marketplace
- Open Standard
- Service Broker

#### Properties

- [Documentation](https://www.openservicebrokerapi.org/)
- [Specification](https://github.com/openservicebrokerapi/servicebroker)
- [Postman Collection](collections/cloud-foundry.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cloud-foundry.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### BOSH Director API

BOSH is Cloud Foundry's release engineering tool for packaging, deploying, and managing distributed software. The BOSH Director API exposes deployment, stemcell, release, task, and VM lifecycle operations used by operators and CI tooling to manage Cloud Foundry itself and the workloads it depends on.

- **Human URL:** [https://bosh.io/docs/director-api-v1/](https://bosh.io/docs/director-api-v1/)

#### Tags

- Deployment
- Infrastructure
- Lifecycle
- Releases

#### Properties

- [Documentation](https://bosh.io/docs/director-api-v1/)
- [Source  Code](https://github.com/cloudfoundry/bosh)
- [Postman Collection](collections/cloud-foundry.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cloud-foundry.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/cloud-foundry)
- [Website](https://www.cloudfoundry.org/)
- [Documentation](https://docs.cloudfoundry.org/)
- [Git Hub](https://github.com/cloudfoundry)
- [Foundation](https://www.cloudfoundry.org/foundation/)
- [Community](https://www.cloudfoundry.org/community/)
- [Slack](https://slack.cloudfoundry.org/)
- [Blog](https://www.cloudfoundry.org/blog/)
- [Events](https://www.cloudfoundry.org/events/)
- [Privacy Policy](https://www.cloudfoundry.org/privacy-policy/)
- [Trademark](https://www.cloudfoundry.org/trademark-policy/)
- [JSON-LD](json-ld/cloud-foundry-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [Spectral Rules](rules/cloud-foundry-rules.yml) — [Spectral](https://docs.stoplight.io/docs/spectral)

## Maintainers

**FN:** Kin Lane
**Email:** kinlane@gmail.com
