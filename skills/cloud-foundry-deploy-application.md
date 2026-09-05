---
name: cloud-foundry-deploy-application
description: Deploy new application source or a container image to a Cloud Foundry foundation using the Cloud Controller API v3, from creating the app through to a running rolling deployment.
api: Cloud Foundry Cloud Controller API v3
generated: '2026-09-05'
method: generated
source: openapi/cloud-foundry-capi-v3-openapi.yaml
operations:
  - listSpaces
  - listApps
  - createApp
  - createPackage
  - uploadPackageBits
  - getPackage
  - createBuild
  - getBuild
  - setCurrentDropletForApp
  - createDeployment
  - getDeployment
  - getJob
  - listFeatureFlags
---

# Deploy an application to Cloud Foundry

Cloud Foundry has no single API host. Before anything else, establish which foundation you are
targeting: the base URL is `https://api.<system-domain>`, and the bearer token must come from
*that* foundation's UAA. `cf oauth-token` prints a valid token if a cf CLI session already exists.

## 0. Check the foundation before assuming a capability

Call `listFeatureFlags` (`GET /v3/feature_flags`). Feature flags are per-deployment; an operation
that works on one Cloud Foundry returns `403 CF-FeatureDisabled` on another. This is the cheapest
call on the API and it prevents the most confusing class of failure.

## 1. Resolve the target space

`listSpaces` (`GET /v3/spaces?names=<space-name>`) — every app belongs to exactly one space, and
the space GUID is required to create one. Authorization is two-layered: a `cloud_controller.write`
scope is not enough without a role on this space.

## 2. Find or create the app

`listApps` (`GET /v3/apps?names=<app-name>&space_guids=<space-guid>`) first. If it exists, reuse it.

If it does not, `createApp` (`POST /v3/apps`) with the space in the relationships envelope:

```json
{"name": "my-app",
 "relationships": {"space": {"data": {"guid": "<space-guid>"}}},
 "lifecycle": {"type": "buildpack", "data": {"buildpacks": [], "stack": "cflinuxfs4"}}}
```

Do **not** blind-retry `createApp` after a timeout. There is no idempotency key on this API; the
protection is a uniqueness constraint, so a duplicate create returns `422 CF-UniquenessError`.
That is a safe outcome — treat it as "already exists" and go back to `listApps`.

## 3. Create a package and upload the bits

`createPackage` (`POST /v3/packages`) with `type: bits` (source) or `type: docker` (image),
related to the app. For `bits`, follow with `uploadPackageBits`
(`POST /v3/packages/{guid}/upload`, multipart). Poll `getPackage` until `state` is `READY`.

## 4. Stage a droplet

`createBuild` (`POST /v3/builds`) referencing the package GUID. Poll `getBuild` until `state` is
`STAGED` (the droplet GUID appears in the response) or `FAILED`. Staging is the slow step —
back off between polls rather than tightening the loop, because polling burns rate-limit budget.

## 5. Ship it

Two paths, and the difference matters:

- **First deploy / downtime acceptable** — `setCurrentDropletForApp`
  (`PATCH /v3/apps/{guid}/relationships/current_droplet`) then `startApp`.
- **Zero-downtime update** — `createDeployment` (`POST /v3/deployments`) with the droplet:

  ```json
  {"droplet": {"guid": "<droplet-guid>"},
   "relationships": {"app": {"data": {"guid": "<app-guid>"}}}}
  ```

  Poll `getDeployment` until `status.value` is `FINALIZED`.

## 6. Handle the async envelope

Operations that return `202` carry a `Location` header pointing at `/v3/jobs/{guid}`. Poll `getJob`
until `state` is `COMPLETE` or `FAILED`. A failed job carries the same error envelope as a
synchronous failure: `{"errors":[{"code":…,"title":"CF-…","detail":"…"}]}`. Branch on `title`,
never on `detail`.

## Rules

- **Rate limits are the operator's, not ours.** Read `X-RateLimit-Reset` — it is an absolute Unix
  timestamp, not a number of seconds. There is no `Retry-After`.
- **Watch `X-Cf-Warnings`** on successful responses; deprecation and quota advisories arrive there,
  not in the body.
- **Retry only reads freely.** `500`/`502`/`503` on a mutation means "state unknown" — re-read
  before retrying.
- **If this deploy goes wrong**, see `cloud-foundry-rollback-deployment`. Do not attempt to fix a
  bad rollout with `deleteApp`; deletes are irreversible and cascade.
