---
name: Quote to shipment
description: Create a freight quote in MVMNT and convert the won quote into a tracked shipment.
api: openapi/mvmnt-openapi-original.yml
method: generated
source: openapi/mvmnt-openapi-original.yml
operations: [createQuote, convertQuoteToShipment, getShipment, trackShipments]
---

# Quote to shipment

Turn a freight quote into a live, tracked shipment against the MVMNT TMS.

## Auth
- OAuth 2.0 client credentials. POST `https://api.mvmnt.io/oauth2/token` with your Client ID/Secret to get a bearer JWT (valid 3600s).
- Send `Authorization: Bearer <token>` on every request. Base URL `https://api.mvmnt.io/v1`.

## Steps
1. **Create the quote** — `createQuote` (`POST /quotes`). Provide customer, mode, and lane details. Capture the returned quote `id`.
2. **Convert to shipment** — once the quote is won, `convertQuoteToShipment` (`POST /quotes/{id}/convert-to-shipment`). This returns the new shipment.
3. **Read the shipment** — `getShipment` (`GET /shipments/{id}`) to confirm status and stops.
4. **Track** — `trackShipments` (`POST /shipments/track`) for current tracking status.

## Rules
- Writes use JSON; partial updates use PATCH (see conventions/mvmnt-conventions.yml).
- Errors return `{error, message, details[]}` (errors/mvmnt-problem-types.yml); parse `details[]` on 422.
- Honor `X-RateLimit-Reset` on 429; there is no request-side idempotency key, so avoid blind retries on writes.
- Subscribe to `QUOTE_WON`, `SHIPMENT_BOOKED`, `SHIPMENT_IN_TRANSIT`, `SHIPMENT_DELIVERED` webhooks instead of polling (asyncapi/mvmnt-webhooks.yml).
