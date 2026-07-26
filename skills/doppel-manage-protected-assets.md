---
name: Manage Doppel brands and protected assets
description: List brands, then list, create, and delete the protected assets Doppel monitors for unauthorized use.
api: openapi/doppel-openapi-original.yml
operations: [list-brands, list-protected-assets, create-protected-asset, delete-protected-asset]
---

# Manage Doppel brands and protected assets

Protected assets are URLs, handles, or identifiers your org wants monitored; they also suppress alerts for legitimate owned properties.

## Auth
`x-api-key` + `x-user-api-key` headers.

## Steps
1. **Find your brands** — `list-brands` (`GET /brands`). Filter by `brand_type` (`organization_brand` or `personal_brand`) and/or `name`; paginated; only active brands returned.
2. **List current assets** — `list-protected-assets` (`GET /protected-assets`). Filter by `platform` and/or brand ids/names; paginated.
3. **Add an asset** — `create-protected-asset` (`POST /protected-asset`) for one or more brands that belong to your org. Must be unique per org (409 on duplicate; 422 on invalid params). Returns HTTP 201.
4. **Remove an asset** — `delete-protected-asset` (`DELETE /protected-asset/{id}`). Stops suppression for matching content.

## Notes
- Pagination is zero-indexed `page`/`page_size` (default 30, max 200) across list endpoints — see `conventions/doppel-conventions.yml`.
- `delete-brand` (`DELETE /brands/{brand_id}`) soft-deletes (archives) a brand and disables its related sourcing queries, trademarks, and takedown assets — use with care (403 if not permitted).
