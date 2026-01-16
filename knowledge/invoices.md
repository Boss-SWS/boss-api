# 🧾 Invoices & Documents


### How do I create a client invoice?

Create invoice: POST /api/clientinvoice. Required: EssentialInfo (Company, Branch, Client, Employee UniqueIds) and ProductsInfo (CalcWithVat, VatPercentage, TotalVatInc, Product array).

**Endpoint:** `POST /api/clientinvoice`

**Example:**
```json
POST /api/clientinvoice
{
  "EssentialInfo": {
    "Company": {"UniqueId": 1},
    "Branch": {"UniqueId": 1},
    "Client": {"UniqueId": 42},
    "Employee": {"UniqueId": 2}
  },
  "ProductsInfo": {
    "CalcWithVat": true,
    "VatPercentage": 17.0,
    "TotalVatInc": 120.0,
    "Product": [{"UniqueId": 1, "UnitValue": 60.0, "Amount": 1, "TotalValue": 60.0}]
  }
}
```

---

### How do I get an invoice by ID?

Get invoice: GET /api/clientinvoice/{UniqueId}. The ID is sent in the URL path.

**Endpoint:** `GET /api/clientinvoice/{UniqueId}`

**Example:**
```json
GET /api/clientinvoice/18
```

---

### How do I create a receipt?

Create receipt: POST /api/ClientReceipt. Includes EssentialInfo and PaymentsInfo (Total, Payment array with Type, Value, CreditUniqueId for credit card).

**Note:** Receipt has no products, so SpecialOffer is not supported.

**Endpoint:** `POST /api/ClientReceipt`

**Example:**
```json
POST /api/ClientReceipt
{
  "EssentialInfo": {...},
  "PaymentsInfo": {
    "Total": 433.0,
    "Payment": [{"Type": "CreditCard", "Value": 433.0, "CreditUniqueId": 31723}]
  }
}
```

---

### How do I create an invoice/receipt?

Create invoice/receipt: POST /api/clientinvoicereceipt. Combines invoice and receipt in one document. Supports SpecialOffer since it contains products.

**Endpoint:** `POST /api/clientinvoicereceipt`

---

### How do I create a client credit?

Create credit: POST /api/clientcrediting. Same structure as invoice, but outcome is credit instead of invoice.

**Endpoint:** `POST /api/clientcrediting`

---

### How do I create a supplier invoice?

Create supplier invoice: POST /api/SupplierInvoice. Same structure as client invoice.

**Note:** Supplier invoices do not support SpecialOffer.

**Endpoint:** `POST /api/SupplierInvoice`

---

### What are the required fields for creating an invoice?

Required fields: EssentialInfo: Company.UniqueId, Branch.UniqueId, Client.UniqueId, Employee.UniqueId. ProductsInfo: CalcWithVat (boolean), TotalVatInc. Product: UniqueId, UnitValue, Amount, TotalValue.

**Endpoint:** `POST /api/clientinvoice`

---

### How do I report discounts/promotions in an invoice?

There are 5 methods to report discounts in invoices:

1. Final price in ProductsInfo
2. Cart discount amount (DiscountAmount)
3. Discount + promotion ID (SpecialOffer)
4. Full promotion with MappedProducts
5. Boss promotion engine

Ask about "discount methods" or "SpecialOffer" for details.

---

### How do I add a promotion to an invoice?

Add SpecialOffer object to your invoice. Simple method (Boss splits discount):

**Endpoint:** `POST /api/clientinvoice`

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
