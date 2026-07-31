---
name: Book a load with a carrier
description: Create a load in MVMNT, find a carrier, and assign it to the load.
api: openapi/mvmnt-openapi-original.yml
method: generated
source: openapi/mvmnt-openapi-original.yml
operations: [createLoad, searchCarriers, addCarrierToLoad, getLoad]
---

# Book a load with a carrier

Create a load and tender it to a carrier in the MVMNT TMS.

## Auth
- OAuth 2.0 client credentials → `Authorization: Bearer <token>`. Base URL `https://api.mvmnt.io/v1`.

## Steps
1. **Create the load** — `createLoad` (`POST /loads`). Capture the returned load `id`.
2. **Find a carrier** — `searchCarriers` (`POST /carriers/search`) to pick an eligible carrier.
3. **Assign the carrier** — `addCarrierToLoad` (`POST /loads/{id}/add-carrier`) with the carrier id and agreed rate.
4. **Confirm** — `getLoad` (`GET /loads/{id}`) to verify the carrier assignment and load status.

## Rules
- Partial edits to the load use PATCH `updateLoad` (`PATCH /loads/{id}`).
- Errors return `{error, message, details[]}`; 422 carries field-level `details[]`.
- Prefer `LOAD_TENDER` / `SHIPMENT_BOOKED` webhooks over polling.
