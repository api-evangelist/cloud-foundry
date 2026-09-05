---
name: cloud-foundry-run-task
description: Run a one-off command (migration, batch job, script) against a deployed Cloud Foundry application as a task, poll it to completion, and understand exactly what cancelling does and does not undo.
api: Cloud Foundry Cloud Controller API v3
generated: '2026-09-05'
method: generated
source: openapi/cloud-foundry-capi-v3-openapi.yaml
operations:
  - listApps
  - getApp
  - getCurrentDropletForApp
  - createTask
  - getTask
  - listAppTasks
  - cancelTask
  - listFeatureFlags
---

# Run a one-off task on Cloud Foundry

A task runs a command once, in a fresh container built from the app's current droplet, with the
app's environment and service bindings. This is how migrations and batch jobs run on Cloud Foundry.

## 0. Confirm tasks are enabled

`listFeatureFlags` (`GET /v3/feature_flags`) — task creation is gated by a feature flag on some
foundations. A disabled flag returns `403 CF-FeatureDisabled`, which is a configuration answer, not
a permissions answer, and retrying will never help.

## 1. Resolve the app and confirm it has a droplet

`listApps` (`GET /v3/apps?names=<name>&space_guids=<space-guid>`), then
`getCurrentDropletForApp` (`GET /v3/apps/{guid}/droplets/current`). A task needs a staged droplet;
an app that has never been built cannot run one.

## 2. Create the task

`createTask` (`POST /v3/apps/{guid}/tasks`):

```json
{"command": "bin/rails db:migrate", "name": "migrate", "memory_in_mb": 512, "disk_in_mb": 1024}
```

The response carries the task GUID and `state: PENDING`.

## 3. Poll to completion

`getTask` (`GET /v3/tasks/{guid}`) until `state` is `SUCCEEDED` or `FAILED`. `listAppTasks`
(`GET /v3/apps/{guid}/tasks?order_by=-created_at`) recovers the GUID if you lost it.

Back off between polls. Every poll spends rate-limit budget that is shared across the whole
foundation user, and `X-RateLimit-Remaining` is only an estimate — it is rounded down to the
nearest 10% of the answering Cloud Controller instance's own count, so it can read `0` while
requests still succeed and read positive while they fail.

## 4. Cancelling — read this before you rely on it

`cancelTask` (`POST /v3/tasks/{guid}/actions/cancel`) works while the task is `PENDING` or
`RUNNING`.

**It kills the process. It does not undo the work.** A migration that has already written half its
changes stays half-written. Cloud Foundry offers no transaction boundary around a task, so
cancellation is an abort, not a rollback. If reversibility matters, the command itself has to
provide it.

There is also a deprecated short path, `PUT /v3/tasks/{guid}/cancel` (`cancelTaskShort`). Use
`cancelTask` instead — and note the deprecation is announced only in the operation's summary text,
not with OpenAPI's `deprecated` flag, so tooling will not warn you.

## 5. There is no idempotency key

`createTask` has no `Idempotency-Key` header and no uniqueness constraint to fall back on. A
retried create after a network timeout **will run the command twice**. Always call `listAppTasks`
and look for a matching recent task before retrying a create you are unsure about.

## Rules

- Branch on the error envelope's `title` (`CF-…`), never on `detail`.
- `502 CF-BadGateway` usually means a downstream component (Diego, a broker) failed — retry with
  backoff.
- Watch `X-Cf-Warnings` on successful responses.
