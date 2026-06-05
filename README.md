# Cloud Foundry (cloud-foundry)

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
