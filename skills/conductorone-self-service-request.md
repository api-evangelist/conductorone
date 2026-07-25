---
name: Configure a self-service access request catalog
description: Build a request catalog and expose requestable entitlements for self-service access in ConductorOne.
api: openapi/conductorone-openapi-original.yml
operations:
  - c1.api.requestcatalog.v1.RequestCatalogManagementService.Create
  - c1.api.requestcatalog.v1.RequestCatalogManagementService.List
  - c1.api.requestcatalog.v1.RequestCatalogManagementService.AddAppEntitlements
  - c1.api.requestcatalog.v1.RequestCatalogManagementService.ListEntitlementsPerCatalog
---

# Configure a self-service access request catalog

## Auth
OAuth2 client-credentials (`POST /auth/v1/token`, credentials in body) or bearer. Base URL `https://{tenantDomain}.conductor.one`.

## Steps
1. **Create a catalog** — `RequestCatalogManagementService.Create` (`POST /api/v1/catalogs`).
2. **Add requestable entitlements** — `RequestCatalogManagementService.AddAppEntitlements` (`POST /api/v1/catalogs/{catalog_id}/requestable_entries`).
3. **Verify** — `RequestCatalogManagementService.ListEntitlementsPerCatalog` (`GET /api/v1/catalogs/{catalog_id}/requestable_entitlements`).
4. **List catalogs** — `RequestCatalogManagementService.List` (`GET /api/v1/catalogs`).

## Rules
- Cursor pagination (`page_size`/`page_token` → `nextPageToken`). See `conventions/conductorone-conventions.yml`.
- Updates use an `updateMask` (protobuf FieldMask) to scope changes.
