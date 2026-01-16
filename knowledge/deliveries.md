# 📦 Deliveries


### Tell me about Deliveries

**Client Delivery (תעודת משלוח)**

A delivery note documenting goods shipped to customer. Comes AFTER order, BEFORE invoice.

**Workflow:**
Order → **Delivery** → Invoice/InvoiceReceipt

**Endpoint:**
• GET /api/ClientDelivery/{id} - Get delivery by ID (read-only)

**Key points:**
• Can be replaced by Invoice or InvoiceReceipt
• Can be canceled by another ClientDelivery
• API is read-only (created in Boss software)

**Structure:**
• UniqueId, TaxNumber
• EssentialInfo, PrintInfo
• DeliveringsInfo (delivery details)
• ProductsInfo (products delivered)
• RewardPointsInfo, ExternalInfo

**Endpoint:** `GET /api/ClientDelivery/{id}`

---

### How do I get a client delivery by ID?

Get delivery: GET /api/ClientDelivery/{id}

**Parameters:**
• id (required) - Delivery UniqueId
• ReportAsSource (optional) - Include source document info

**Endpoint:** `GET /api/ClientDelivery/{id}`

**Example:**
```json
GET /api/ClientDelivery/123

GET /api/ClientDelivery/123?ReportAsSource=true
```

---

### What is the client delivery structure?

Client Delivery object structure:

**Endpoint:** `GET /api/ClientDelivery/{id}`

**Object Structure:**
```
ClientDelivery:
  UniqueId (long?) - Delivery ID
  TaxNumber (long?) - Tax document number
  EssentialInfo - Company, Branch, Client, Employee
  PrintInfo - Print/display settings
  DeliveringsInfo - Delivery details (address, dates)
  ProductsInfo - Products delivered
  RewardPointsInfo - Loyalty points
  ExternalInfo - External reference
```

---

### What is the delivery workflow?

**Client Document Workflow:**

```
Quote (הצעת מחיר)
    ↓
Order (הזמנה)
    ↓
Delivery (תעודת משלוח)  ← You are here
    ↓
Invoice / InvoiceReceipt (חשבונית)
    ↓
Receipt (קבלה) - if separate
```

**Delivery can be:**
• Replaced by Invoice (converts delivery to invoice)
• Replaced by InvoiceReceipt (converts to combined doc)
• Canceled by another Delivery (negative/cancellation)

---

### How do I cancel a delivery?

A Client Delivery is canceled by creating another Client Delivery that references it as a cancellation.

**Note:** This is done in Boss software, not via API.

The API is read-only - you can only GET existing deliveries.

---
