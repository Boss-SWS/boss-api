# Add-On: Custom API Webhook (Outbound Events)

This document describes the **Custom API Webhook add-on** used by specific customers to receive outbound notifications from the system when business objects are **created** or **updated**.

This is **not** the public API. It is a **customer-specific add-on**.

---

## Overview

When enabled, the system sends HTTP POST requests to a customer-defined endpoint whenever configured events occur.

Supported entities typically include:
- Customers (create / update)
- Movements (create / update)
- Movement items / lines (optional, per customer)

Delivery is **at-least-once**.

---

## Configuration

Per customer configuration includes:
- Webhook URL
- Shared secret
- Enabled event types
- Optional authentication or custom headers

---

## HTTP Request

- Method: POST  
- Content-Type: application/json  
- Response: Any 2xx status acknowledges delivery

---

## Headers

- X-Webhook-Event
- X-Webhook-Id
- X-Webhook-Timestamp
- X-Webhook-Signature

---

## Event Names

Examples:
- customer.created
- customer.updated
- movement.created
- movement.updated

Exact events are customer-specific.

---

## Payload Envelope

```json
{
  "id": "evt_unique_id",
  "event": "customer.created",
  "timestampUtc": "2026-01-16T10:22:31Z",
  "data": {
    "entity": "customer",
    "operation": "created",
    "keys": {
      "customerUid": "CUST_123"
    },
    "record": {}
  }
}
```

---

## Security

Requests are signed using HMAC-SHA256 with the shared secret.
Consumers must validate the signature and timestamp.

---

## Idempotency

Duplicate deliveries are possible.
Consumers must deduplicate using the event id.

---

## Retries

Failures or non-2xx responses trigger retries with backoff.

---

## Notes

Payload fields and schemas may vary per customer and must not be assumed stable unless explicitly agreed.
