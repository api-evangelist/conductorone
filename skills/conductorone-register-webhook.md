---
name: Register and test a ConductorOne outbound webhook
description: Create, test, and verify an outbound webhook so approvals/provisioning/workflow events are delivered to your endpoint.
api: openapi/conductorone-openapi-original.yml
operations:
  - c1.api.webhooks.v1.WebhooksService.Create
  - c1.api.webhooks.v1.WebhooksService.Test
  - c1.api.webhooks.v1.WebhooksService.Get
  - c1.api.webhooks.v1.WebhooksService.List
---

# Register and test a ConductorOne outbound webhook

## Auth
OAuth2 client-credentials (`POST /auth/v1/token`, credentials in body) or bearer. Base URL `https://{tenantDomain}.conductor.one`.

## Steps
1. **Create the webhook** — `WebhooksService.Create` (`POST /api/v1/webhooks`) with `displayName` + `url` (both required).
2. **Send a test delivery** — `WebhooksService.Test` (`POST /api/v1/webhooks/{id}/test`) and confirm your endpoint receives it.
3. **Fetch state** — `WebhooksService.Get` (`GET /api/v1/webhooks/{id}`); list all with `WebhooksService.List` (`GET /api/v1/webhooks`).

## Delivery contract
- Each delivery carries a `Webhook-Event` header naming the payload type (`c1.webhooks.v1.PayloadPolicyApprovalStep`, `PayloadPolicyPostAction`, `PayloadProvisionStep`, `PayloadWorkflowStep`, `PayloadTest`).
- To respond asynchronously, return HTTP `202 Accepted` and POST your response body to the `Webhook-Callback-Url` header value.
- Listener authentication supports HMAC and JWT (JWKS). See `asyncapi/conductorone-webhooks.yml`.
