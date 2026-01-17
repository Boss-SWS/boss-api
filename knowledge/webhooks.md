# Add-On: Custom API Webhooks (Outbound Events)

This document describes the **Custom API Webhooks add-on**, used by specific customers to receive **HTTP POST notifications** from Boss when selected business objects are created or updated.

This is **not** the public Boss WebAPI.  
It is a **customer-specific add-on**, configured and enabled per customer.

---

## What this add-on provides

The Custom API Webhooks add-on enables **server-initiated outbound HTTP calls** (“webhooks”) from Boss to a customer endpoint.

It is commonly used to:
- Push real-time business events to external systems
- Integrate Boss with third-party platforms
- Serve as part of a broader **WebAPI enablement / integration package** for advanced customers

---

## Overview

- **Direction:** Boss ➜ Customer endpoint
- **Method:** HTTP `POST`
- **Format:** JSON (`application/json`)
- **Acknowledgement:** any HTTP **2xx** response
- **Authentication:** configured in the UI
- **Payload:** typed business object (no generic event envelope)

---

## Supported webhook event types

Configured per customer in the UI. Common event types include:

- **Customer Update**
- **Customer MoveMent Update**
- **Delivery Status Update**
- **Product Update** (optional)

> The exact list of enabled events depends on the customer configuration.

---

## Configuration (UI)

Each webhook configuration includes:

- **Type**  
  The business event category (Customer, MoveMent, Delivery, Product)

- **Webhook URL (BaseURL)**  
  The full HTTP endpoint that will receive the POST request

- **Authentication Type**
  - `None`
  - `Basic`
  - `Bearer`
  - `ApiKey`

- **Token / Secret**  
  Used according to the selected authentication type

---

## HTTP request

For each event occurrence, Boss sends:

- **Method:** `POST`
- **URL:** configured Webhook URL
- **Content-Type:** `application/json`
- **Body:** the relevant business entity serialized to JSON

### Important differences from the legacy “webhook framework” style
- There is **no wrapper** like `{ id, event, timestamp, data: {...} }`.
- There are **no standard webhook headers** like `X-Webhook-*`.
- The body is the **business object itself**.

---

## Authentication

Authentication is applied according to the UI configuration:

### None
No authentication headers are sent.

### Basic
Standard HTTP Basic Authentication.

### Bearer
```
Authorization: Bearer <token>
```

### ApiKey
An API key header, as agreed with the customer integration.

---

## Webhook payloads (what your endpoint receives)

Each webhook delivers a **typed business object**.  
The exact schema is **customer-specific**, agreed as part of the integration.

This section documents the **expected object category** for each event type, and provides examples:
- **Customer Update**: full example (schema known)
- **MoveMent** and **Delivery**: examples are **template examples** because the exact schema varies by customer/version. Templates show how you should design your receiver (parse JSON, extract identifiers/status, store raw payload, etc.) without assuming specific field names beyond what your contract defines.

> If you need the exact JSON schema for MoveMent or Delivery in your customer deployment, request the agreed payload sample (or model file) from Boss support for that customer.

---

## 1) Customer Update

Sent when a customer is created or updated.

**Payload:** `ProxyGeneralCustomer` (serialized as JSON)

### What it typically includes (high-level)
- Identity: `UniqueId`, `BusinessId`
- Creation time: `CreationDateAndTime`
- Person: `FirstName`, `LastName`, `Gender`, `DateOfBirth`
- Contact: `Email`, `Phones[]`
- Address: `Address` (City/Street + house/apartment/floor/entrance)
- Permissions: `AllowSms`, `AllowEmail`, `AllowWhatsApp`, `AllowMoveMentByEmail`, `AllowMarketing`
- Tags: `Tags[]`
- Loyalty: `AccumulatedRewardPoints`
- Commercial: `PriceListId`
- Prognosis: `PrognosisNextOrderDayDiffVal` (if enabled)

### Example payload
```json
{
  "UniqueId": 12345,
  "CreationDateAndTime": "2026-01-16T10:22:31Z",
  "FirstName": "John",
  "LastName": "Doe",
  "Email": "john@doe.com",
  "Phones": [
    { "Number": "0501234567", "Description": "Mobile" }
  ],
  "Address": {
    "City": { "UniqueId": 10, "Name": "Tel Aviv" },
    "Street": { "UniqueId": 20, "Name": "Ibn Gabirol" },
    "HouseNum": "12",
    "ApartmentNum": "3",
    "FloorNum": "1",
    "Entrance": "A"
  },
  "AssociatedBranch": { "UniqueId": 7, "Name": "Main Branch" },
  "AllowSms": true,
  "AllowEmail": true,
  "AllowWhatsApp": false,
  "AllowMoveMentByEmail": true,
  "AllowMarketing": true,
  "Tags": [{ "UniqueId": 1, "Name": "VIP" }],
  "AccumulatedRewardPoints": 120,
  "PriceListId": 2,
  "DateOfBirth": "1980-01-01T00:00:00Z",
  "BusinessId": "516389566",
  "PrognosisNextOrderDayDiffVal": 14
}
```

---

## 2) Customer MoveMent Update

Sent when a customer movement (transaction/activity) is created or updated.

**Payload:** MoveMent object (serialized as JSON)

### What it represents
A MoveMent payload represents an operational transaction/activity in Boss (e.g., sale, invoice/receipt activity, etc. depending on your integration).

### What your receiver should do
Because schema varies per customer/version, your receiver should:
1. Parse JSON safely.
2. Extract the **primary identifiers** agreed in your integration contract (e.g., movement UID, customer UID).
3. Optionally extract status/amount/time fields agreed in your contract.
4. Store the raw payload (recommended) for debugging/auditing.

### Template example (schema placeholders)
> Replace the placeholder field names below with the ones defined for your customer deployment.
```json
{
  "<movementUniqueIdField>": "<MOVEMENT_UID>",
  "<customerUniqueIdField>": "<CUSTOMER_UID>",
  "<businessIdField>": "<BUSINESS_ID>",
  "<createdUtcField>": "2026-01-16T10:22:31Z",
  "<typeField>": "<MOVEMENT_TYPE>",
  "<amountField>": 213.0,
  "<itemsField>": [
    { "<itemIdField>": "<ITEM_UID>", "<qtyField>": 1, "<priceField>": 213.0 }
  ]
}
```

---

## 3) Delivery Status Update

Sent when delivery status changes / delivery updates occur.

**Payload:** Delivery object (serialized as JSON)

### What it represents
A Delivery payload represents a delivery/shipping entity or a delivery status update tied to a transaction/order, depending on the customer integration.

### What your receiver should do
1. Parse JSON safely.
2. Extract:
   - Delivery identifier agreed in your contract
   - Related transaction/movement/order identifier (if present in your contract)
   - The delivery status value / stage
   - Any timestamps (status time, updated time)
3. Store the raw payload (recommended).

### Template example (schema placeholders)
> Replace the placeholder field names below with the ones defined for your customer deployment.
```json
{
  "<deliveryUniqueIdField>": "<DELIVERY_UID>",
  "<relatedMovementOrOrderField>": "<RELATED_UID>",
  "<statusField>": "<DELIVERY_STATUS>",
  "<statusUpdatedUtcField>": "2026-01-16T11:05:00Z",
  "<notesField>": "<OPTIONAL_NOTES>"
}
```

---

## Response handling

- Any **2xx** HTTP response acknowledges successful delivery.
- Non-2xx responses indicate failure.

Recommended: respond quickly with `200 OK` after validating auth and basic JSON parsing, then process asynchronously on your side if needed.

---

## Delivery semantics

- Webhook delivery is **best-effort**.
- Duplicate deliveries may occur.
- Ordering is **not guaranteed**.

Consumers should implement **idempotent processing** (deduplicate using identifiers agreed in the contract).

---

## Relationship to WebAPI

For some customers, this Custom API add-on is deployed as part of a broader **WebAPI enablement and integration package**.

In such setups:
- Webhooks are used for **outbound events**
- WebAPI endpoints may be used for **inbound or pull-based access**
- Authentication/routing/integration logic may be aligned between both

Exact capabilities depend on the customer agreement.

---

## Important notes

- Payload schemas are **customer-specific**
- Fields may evolve over time
- Consumers must not assume schema stability unless explicitly agreed

---

## Summary

The Custom API Webhooks add-on provides a **direct, event-driven integration mechanism** that pushes real business events from Boss to customer systems using simple HTTP POST calls.
