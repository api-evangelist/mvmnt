---
name: Onboard a carrier
description: Add a carrier to MVMNT with a contact and a payment method so it can be booked on loads.
api: openapi/mvmnt-openapi-original.yml
method: generated
source: openapi/mvmnt-openapi-original.yml
operations: [searchCarriers, createCarrier, createCarrierContact, createCarrierPaymentMethod]
---

# Onboard a carrier

Register a carrier in the MVMNT TMS with the contact and remittance details needed to book and pay it.

## Auth
- OAuth 2.0 client credentials (`POST https://api.mvmnt.io/oauth2/token`) → `Authorization: Bearer <token>`. Base URL `https://api.mvmnt.io/v1`.

## Steps
1. **De-dupe** — `searchCarriers` (`POST /carriers/search`) by name / MC / DOT to avoid creating a duplicate.
2. **Create the carrier** — `createCarrier` (`POST /carriers`). Capture the returned `id`.
3. **Add a contact** — `createCarrierContact` (`POST /carrier-contacts`) referencing the carrier.
4. **Add a payment method** — `createCarrierPaymentMethod` (`POST /carrier-payment-methods`) so bills can be paid.

## Rules
- Store your own system id as a client key so later calls can look the carrier up by your id (conventions: client_keys).
- 409 `DUPLICATE_KEY` means the carrier/contact already exists — search first.
- Watch `CARRIER_CREATED`, `CARRIER_ACTIVATED`, `CARRIER_PAYMENT_METHOD_CREATED` webhooks.
