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
- **Format:** JSON
- **Acknowledgement:** any HTTP **2xx** response
- **Authentication:** configured in the UI
- **Payload:** typed business object (no generic envelope)

---

## Supported webhook event types

Configured per customer in the UI.

Common event types include:

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

There is **no additional wrapper object** and **no webhook-specific headers**.

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

## Webhook payloads

Each webhook delivers a **typed business object**.  
The payload structure reflects the Boss data model agreed for the customer integration.

### Customer Update

Sent when a customer is created or updated.

**Payload:** Customer object  
Includes (non-exhaustive):

- Customer identifiers
- Creation date
- Name and personal details
- Contact details (email, phones)
- Address
- Communication permissions
- Tags
- Loyalty / reward information
- Business identifier

Example (illustrative):
```json
{
  "UniqueId": 12345,
  "FirstName": "John",
  "LastName": "Doe",
  "Email": "john@doe.com",
  "Phones": [
    { "Number": "0501234567", "Description": "Mobile" }
  ],
  "AllowSms": true,
  "AllowEmail": true,
  "AllowMarketing": true,
  "BusinessId": "516389566"
}
```

---

### Customer MoveMent Update

Sent when a customer movement (transaction/activity) is created or updated.

**Payload:** MoveMent object  
Typically includes:
- Movement identifiers
- Customer identifiers
- Transactional data
- Monetary values
- Status and timing fields

The exact schema depends on the customer integration agreement.

---

### Delivery Status Update

Sent when a delivery status changes.

**Payload:** Delivery object  
Typically includes:
- Delivery identifier
- Related movement / order
- Current delivery status
- Relevant timestamps

---

## Response handling

- Any **2xx** HTTP response acknowledges successful delivery.
- Non-2xx responses indicate failure.

---

## Delivery semantics

- Webhook delivery is **best-effort**.
- Duplicate deliveries may occur.
- Ordering is **not guaranteed**.

Consumers should implement **idempotent processing** where applicable.

---

## Relationship to WebAPI

For some customers, this Custom API add-on is deployed as part of a broader **WebAPI enablement and integration package**.

In such setups:
- Webhooks are used for **outbound events**
- WebAPI endpoints may be used for **inbound or pull-based access**
- Authentication, routing, and integration logic may be aligned between both

Exact capabilities depend on the customer agreement.

---

## Important notes

- Payload schemas are **customer-specific**
- Fields may evolve over time
- Consumers must not assume schema stability unless explicitly agreed

---

## Summary

The Custom API Webhooks add-on provides a **direct, event-driven integration mechanism** that pushes real business events from Boss to customer systems using simple HTTP POST calls.
