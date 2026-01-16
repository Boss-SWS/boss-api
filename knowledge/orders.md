# 📋 Orders


### Tell me about Orders

**Client Orders (הזמנות)**

An Order is a reference to a future transaction - no financial obligation until converted to invoice.

**Endpoints:**
• POST /api/ClientOrder - Create order
• GET /api/ClientOrder/{id} - Get by ID
• GET /api/ClientOrder - Search orders

**Structure:** Same as Invoice
• EssentialInfo (Company, Branch, Client, Employee)
• ProductsInfo (products, prices, VAT)
• ExternalInfo (external reference number)
• SpecialOffer (optional - promotions)

**Workflow:** Order → Invoice → Receipt

**Endpoint:** `POST /api/ClientOrder`

---

### What is the difference between an order and an invoice?

An Order (ClientOrder) is a reference to a future transaction - it has no financial obligation until converted to an invoice. An Invoice (ClientInvoice) is a document with binding financial obligation.

**Endpoint:** `POST /api/ClientOrder`

---

### How do I create a client order?

Create order: POST /api/ClientOrder. Same structure as invoice - EssentialInfo and ProductsInfo. Used to connect ecommerce website with Boss.

**Endpoint:** `POST /api/ClientOrder`

**Example:**
```json
POST /api/ClientOrder
{
  "EssentialInfo": {
    "Company": {"UniqueId": 1},
    "Branch": {"UniqueId": 1},
    "Client": {"UniqueId": 42},
    "Employee": {"UniqueId": 2}
  },
  "ProductsInfo": {
    "CalcWithVat": true,
    "TotalVatInc": 120.0,
    "Product": [{"UniqueId": 1, "UnitValue": 60.0, "Amount": 1, "TotalValue": 60.0}]
  },
  "ExternalInfo": {"Number": 777}
}
```

---

### How do I get an order by ID?

Get order: GET /api/ClientOrder/{UniqueId}. Works exactly like Get Client By UniqueId.

**Endpoint:** `GET /api/ClientOrder/{UniqueId}`

**Example:**
```json
GET /api/ClientOrder/3
```

---

### What is ExternalInfo.Number?

ExternalInfo.Number is an external reference number (Int64) - used to store the order number from external system (like ecommerce site) for tracking and connecting systems.

**Endpoint:** `POST /api/ClientOrder`

---

### How do I report discounts/promotions in an order?

There are 5 methods to report discounts in orders:

1. Final price in ProductsInfo
2. Cart discount amount (DiscountAmount)
3. Discount + promotion ID (SpecialOffer)
4. Full promotion with MappedProducts
5. Boss promotion engine

Ask about "discount methods" or "SpecialOffer" for details.

---

### How do I add a promotion to an order?

Add SpecialOffer object to your order. Simple method (Boss splits discount):

**Endpoint:** `POST /api/ClientOrder`

**Example:**
```json
{
  "EssentialInfo": {...},
  "ProductsInfo": {
    "CalcWithVat": true,
    "TotalVatInc": 100.0,
    "Product": [...]
  },
  "SpecialOffer": {
    "SpecialOfferId": 17,
    "SpecialOfferName": "Summer Sale",
    "DiscountAmount": 10.00
  }
}
```

---
