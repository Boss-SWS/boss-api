# Add-On: Custom API Webhook / Outbound Events (HTTP Push)

This document describes the **Custom API AddOn** capability that supports **outbound event delivery** (sometimes called “webhooks”) to customer endpoints when selected business objects are created/updated.

> **Important terminology note**
> - The UI may call these “Webhooks”.
> - Technically, this is **server-initiated HTTP push** implemented via a **customer-specific Custom AddOn proxy**, not a generic “webhook framework” (no standard envelope, no HMAC webhook headers, etc.).
> - Payloads are **typed business objects** serialized as JSON.

This is **not** the public Boss WebAPI by itself. It is a **customer-specific add-on** that can be used as part of a wider integration package (see: “Relationship to WebAPI enablement”).

---

## What this AddOn is used for

### 1) Outbound events (the “webhook” part)
When enabled, the Boss server sends **HTTP POST** requests to a customer-defined endpoint whenever configured events occur (e.g., customer updated, movement updated, delivery status updated).

### 2) WebAPI enablement / integration package role
This same AddOn is also used in some customer setups as a **foundation component** to enable/host additional integration capabilities (including WebAPI access patterns, routing, auth bridging, or customer-specific extensions).

In other words:
- **Outbound events** are one capability of the Custom AddOn.
- The Custom AddOn may also be deployed/used as part of the broader **WebAPI enablement** package for a customer integration.

(Implementation details and exact scope depend on the customer and deployment.)

---

## Overview (Outbound Events)

- **Direction:** Boss server ➜ Customer endpoint
- **Transport:** HTTP `POST`
- **Format:** JSON
- **Delivery acknowledgement:** any **2xx** response
- **Payloads:** typed business objects (no generic envelope)
- **Auth:** configured in UI (`None` / `Basic` / `Bearer` / `ApiKey`)

---

## Supported event types (common)

Configured per customer in the UI. Common types include:

- **Customer Update**
- **Customer MoveMent Update**
- **Delivery Status Update**
- **Product Update** (if enabled for the customer)

> Availability and exact naming may vary by customer and version.

---

## Configuration (UI)

Each outbound-event configuration includes:

- **Type** (event category)
  - Deliveries
  - Customer MoveMents
  - Customer Update
  - Product Update

- **BaseURL**
  - The customer’s endpoint URL to receive the POST call.
  - In practice, each Type can be configured with its own BaseURL if needed.

- **Authentication Type**
  - `None`
  - `Basic`
  - `Bearer`
  - `ApiKey`

- **Token / Secret**
  - Used according to the selected authentication type.

---

## HTTP request specification

- **Method:** `POST`
- **Content-Type:** `application/json`
- **URL:** `BaseURL` (from configuration)
- **Success condition:** any HTTP **2xx** response

### Headers
This AddOn **does not** use the legacy “webhook header set” such as:
- `X-Webhook-Event`
- `X-Webhook-Id`
- `X-Webhook-Timestamp`
- `X-Webhook-Signature`

If customers require custom headers beyond the standard auth mechanism, that is a customer-specific integration requirement and must be agreed explicitly.

---

## Authentication

Authentication is applied according to the UI setting.

### None
No authentication headers are added.

### Basic
Standard HTTP Basic authentication is applied (customer-specific token/secret mapping).

### Bearer
Adds:
```
Authorization: Bearer <token>
```

### ApiKey
Adds an API key header according to the customer integration contract (header name/value are customer-specific within the add-on configuration).

> Implementation note: the request is built using a shared helper that applies **both** auth and JSON body serialization.

---

## Payloads (Outbound Events)

The payload body is the **typed business object** serialized to JSON. There is **no wrapper envelope** like `{ id, event, timestamp, data: {...} }`.

Below are the common event payloads used by the server implementation.

---

### 1) Customer Update

**When:** customer is created/updated (as configured).  
**HTTP:** `POST <BaseURL-for-CustomerUpdate>`  
**Body type:** `ProxyGeneralCustomer`

#### Model: `ProxyGeneralCustomer` (high-level)
The customer object includes:
- Identity: `UniqueId`, `BusinessId`
- Creation: `CreationDateAndTime`
- Person: `FirstName`, `LastName`, `Gender`, `DateOfBirth`
- Contact: `Email`, `Phones[]`
- Address: `Address` (City/Street + house/apartment/floor/entrance)
- Permissions: `AllowSms`, `AllowEmail`, `AllowWhatsApp`, `AllowMoveMentByEmail`, `AllowMarketing`
- Tags: `Tags[]`
- Loyalty: `AccumulatedRewardPoints`
- Commercial: `PriceListId`
- Prognosis: `PrognosisNextOrderDayDiffVal` (if enabled)

#### Example (illustrative)
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

### 2) Customer MoveMent Update

**When:** movement is created/updated (as configured).  
**HTTP:** `POST <BaseURL-for-CustomerMoveMent>`  
**Body type:** `ProxyGeneralCustomerMoveMent`

#### Enrichment behavior (server-side)
Before sending the movement, the server enriches the payload with customer contact info:
- `MoveMent.Essential.CustomerEmail` is filled from `Customer.Email`
- `MoveMent.Essential.CustomerPhones` is filled from `Customer.Phones[]` (trimmed and filtered)

This means the movement payload may contain customer email/phones even if the core movement entity does not originally include them.

#### Notes
- The **exact movement schema** depends on the customer integration contract and the add-on version.
- Consumers should treat the JSON schema as agreed between parties and not assume it is identical to public WebAPI movement models.

---

### 3) Delivery Status Update

**When:** delivery status changes / delivery update event occurs (as configured).  
**HTTP:** `POST <BaseURL-for-DeliveryStatus>`  
**Body type:** `ProxyGeneralDelivery`

#### Notes
- The delivery payload is a typed object serialized as JSON.
- Exact fields depend on the customer integration contract and the add-on version.

---

## Server implementation notes (for maintainers & integrators)

### How the request is sent (behavioral contract)
In server code, each event is sent by creating a REST client and posting the payload to `BaseURL`:

- `BossCustomProxy_UpdateCustomer(BaseURL, Token, ProxyGeneralCustomer customer)`
- `BossCustomProxy_UpdateCustomerMoveMent(BaseURL, Token, ..., ProxyGeneralCustomerMoveMent moveMent, ProxyGeneralCustomer customer, ProxyGeneralBranch branch)`
- `BossCustomProxy_AddCustomerMoveMentDeliveryStatus(BaseURL, Token, ProxyGeneralDelivery del)`

Common behavior:
- `request = new RestRequest(BaseURL, Method.Post)`
- `AddAuthAndBody(request, Token, payload)`
- `client.Execute(request)`
- If `response.IsSuccessful` ➜ success (return `0`)
- Else ➜ log status code and return it
- Exceptions ➜ return `666` (generic failure)

### Logging
If API logging is enabled, the request/response can be logged (customer-specific operational setting).

---

## Delivery semantics, retries, and idempotency

### Delivery semantics
This add-on treats delivery as successful if the customer endpoint returns **any 2xx** response.

### Retries
A formal retry/backoff contract is **not guaranteed** at the documentation level for all deployments.
If retries are enabled/implemented for a customer, that behavior is customer- and deployment-specific.

### Idempotency (recommended)
Even without a formal retry contract, consumers should implement idempotent handling:
- Treat POST deliveries as potentially repeated (network timeouts, transient errors, manual resends)
- Deduplicate based on the object identity and/or last update timestamp (where applicable)

---

## Customer integration contract (important)

Because this is a **customer-specific add-on**:
- Payload fields and schemas may vary by customer.
- Authentication details (especially for ApiKey header naming) may vary by customer.
- Event types enabled may vary by customer.

Consumers must not assume schema stability unless explicitly agreed.

---

## Quick checklist for customer endpoint implementers

1. Expose a public HTTPS endpoint to receive `POST` JSON.
2. Implement the auth method selected in the UI (None/Basic/Bearer/ApiKey).
3. Return **2xx** quickly to acknowledge receipt.
4. Log payloads safely (do not log secrets).
5. Handle duplicates safely (idempotency).
6. Agree on payload schema per event type and version.

---
