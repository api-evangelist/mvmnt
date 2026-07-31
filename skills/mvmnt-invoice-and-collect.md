---
name: Invoice a shipment and collect
description: Generate a customer invoice from a delivered shipment, send it, and check outstanding balances.
api: openapi/mvmnt-openapi-original.yml
method: generated
source: openapi/mvmnt-openapi-original.yml
operations: [markShipmentReadyToInvoice, generateShipmentInvoice, sendInvoiceEmail, getCustomerOutstandingInvoices]
---

# Invoice a shipment and collect

Run the accounts-receivable side of a completed shipment in the MVMNT TMS.

## Auth
- OAuth 2.0 client credentials → `Authorization: Bearer <token>`. Base URL `https://api.mvmnt.io/v1`.

## Steps
1. **Mark ready to invoice** — `markShipmentReadyToInvoice` (`POST /shipments/{id}/ready-to-invoice`).
2. **Generate the invoice** — `generateShipmentInvoice` (`POST /shipments/{id}/invoice/generate`). Capture the invoice `id`.
3. **Send it** — `sendInvoiceEmail` (`POST /invoices/{id}/send`) to email the customer.
4. **Track collection** — `getCustomerOutstandingInvoices` (`GET /customers/{id}/outstanding-invoices`) to see what is still owed.

## Rules
- Only invoice shipments in a ready-to-invoice state; use `markShipmentNotReadyToInvoice` to reverse.
- Errors return `{error, message, details[]}`; watch `CUSTOMER_INVOICE_CREATED` and `CUSTOMER_PAYMENT_CREATED` webhooks to reconcile.
