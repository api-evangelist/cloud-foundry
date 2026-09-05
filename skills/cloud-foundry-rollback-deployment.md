---
name: cloud-foundry-rollback-deployment
description: Reverse a bad Cloud Foundry deploy — cancel a rollout that is still in flight, or roll an app back to a previous revision — and know which of the two windows you are still inside.
api: Cloud Foundry Cloud Controller API v3
generated: '2026-09-05'
method: generated
source: openapi/cloud-foundry-capi-v3-openapi.yaml, https://docs.cloudfoundry.org/devguide/revisions.html
operations:
  - listDeployments
  - getDeployment
  - cancelDeployment
  - listRevisionsForApp
  - listDeployedRevisionsForApp
  - getRevision
  - createDeployment
  - listAppDroplets
  - getJob
---

# Roll back a Cloud Foundry deployment

Cloud Foundry is unusually good at this: rolling back is the *same operation* as rolling forward,
so it uses the same well-tested path. There are two mechanisms and they apply at different moments.

## Which window are you in?

`getDeployment` (`GET /v3/deployments/{guid}`), or `listDeployments`
(`GET /v3/deployments?app_guids=<app-guid>&order_by=-created_at`) if you do not have the GUID.

- `status.value` is **`DEPLOYING`** → you can cancel. Go to §1.
- `status.value` is **`FINALIZED`** → cancelling is no longer possible. Go to §2.

## 1. Cancel an in-flight rollout

`cancelDeployment` (`POST /v3/deployments/{guid}/actions/cancel`). This reverts the app to the
droplet and process state it had *before* the rollout started. It is the cleanest reversal
available on this API — nothing new was ever fully in service.

## 2. Roll back to a previous revision

`listRevisionsForApp` (`GET /v3/apps/{guid}/revisions?order_by=-created_at`) to see what is
available, and `listDeployedRevisionsForApp` to see what is live now.

Then `createDeployment` with the revision instead of a droplet:

```json
{"revision": {"guid": "<revision-guid>"},
 "relationships": {"app": {"data": {"guid": "<app-guid>"}}}}
```

(`cf rollback APP-NAME --version N` is the CLI form of exactly this call.)

Poll `getDeployment` until `status.value` is `FINALIZED`.

## The trap: two different retention numbers

The documentation states two limits and they are **not** the same:

- "By default, CAPI retains a maximum of **100 revisions** per app."
- "By default, Cloud Foundry retains the **five most recent staged droplets** in its droplets
  bucket" (raisable by the operator via `system_blobstore_ccdroplet_max_staged_droplets_stored`).

So a revision can still appear in `listRevisionsForApp` while the droplet it points at has already
been pruned. **Do not treat "the revision is listed" as "the rollback will succeed."** Before
committing, cross-check with `listAppDroplets` (`GET /v3/apps/{guid}/droplets`) that the revision's
droplet is still present. On a default foundation only the five most recent are guaranteed
deployable.

## What cannot be rolled back

- **Deletes.** No trash, no soft delete, no restore operation, no retention window. `deleteApp`,
  `deleteSpace` and `deleteOrganization` cascade to everything beneath them. Escalate to a human.
- **Task side effects.** `cancelTask` stops the process; it does not undo what the task already did.
- **Service instance deletion.** Whether backing data survives is the *broker's* decision, not
  Cloud Foundry's.

## Rules

- Branch on the `title` field (`CF-…`) in the error envelope, never on `detail`.
- A `422 CF-UnprocessableEntity` here usually means the target revision's droplet is gone — check
  §"the trap" before reporting it as a platform failure.
- Read `X-RateLimit-Reset` as an absolute Unix timestamp. There is no `Retry-After` on this API.
