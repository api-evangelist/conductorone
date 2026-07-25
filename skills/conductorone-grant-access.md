---
name: Grant and revoke access with ConductorOne
description: Discover apps and entitlements, then create grant/revoke tasks and approve them via the ConductorOne (C1) API.
api: openapi/conductorone-openapi-original.yml
operations:
  - c1.api.app.v1.Apps.List
  - c1.api.app.v1.AppEntitlements.List
  - c1.api.task.v1.TaskService.CreateGrantTask
  - c1.api.task.v1.TaskService.CreateRevokeTask
  - c1.api.task.v1.TaskActionsService.Approve
  - c1.api.task.v1.TaskService.Get
---

# Grant and revoke access with ConductorOne

Use this to move a user's access with an auditable task in ConductorOne.

## Auth
Get an OAuth2 client-credentials token from `POST /auth/v1/token` (credentials in the **body**, not headers), or use a bearer token. Base URL is `https://{tenantDomain}.conductor.one`. See `authentication/conductorone-authentication.yml`.

## Steps
1. **List apps** — `Apps.List` (`GET /api/v1/apps`). Paginate with `page_size` + `page_token`; follow `nextPageToken`.
2. **List entitlements for the app** — `AppEntitlements.List` (`GET /api/v1/apps/{app_id}/entitlements`) to find the `app_entitlement_id` to grant.
3. **Create a grant task** — `TaskService.CreateGrantTask` (`POST /api/v1/task/grant`) with the target user + entitlement. To remove access instead, use `TaskService.CreateRevokeTask` (`POST /api/v1/task/revoke`).
4. **Approve** — `TaskActionsService.Approve` (`POST /api/v1/tasks/{task_id}/action/approve`) when the policy requires approval.
5. **Confirm** — `TaskService.Get` (`GET /api/v1/tasks/{id}`) and check the task state.

## Rules
- Errors are `google.rpc.Status` (code/message/details); handle `permission_denied` and `resource_exhausted` (429, back off). See `errors/conductorone-error-codes.yml`.
- No idempotency key: do not blindly retry a create; check task state first.
