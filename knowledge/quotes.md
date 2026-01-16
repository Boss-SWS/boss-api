# 📝 Quotes


### Tell me about Quotes

**Client Quote (הצעת מחיר)**

A Quote is a price proposal for a customer - non-binding until converted to an order or invoice.

**Endpoint:**
• GET /api/ClientQuote/{id} - Get quote by ID

**Parameters:**
• id (required) - Quote UniqueId
• ReportAsSource (optional) - Include source info

**Structure:**
• UniqueId, TaxNumber
• EssentialInfo (Company, Branch, Client, Employee)
• PrintInfo
• ProductsInfo (products, prices, VAT)
• ExternalInfo

**Note:** Quotes are created in Boss software, API is read-only.

**Endpoint:** `GET /api/ClientQuote/{id}`

---

### How do I get a quote by ID?

Get quote: GET /api/ClientQuote/{id}

**Parameters:**
• id - Quote UniqueId (required)
• ReportAsSource - Include source document info (optional, default false)

**Endpoint:** `GET /api/ClientQuote/{id}`

**Example:**
```json
GET /api/ClientQuote/55

GET /api/ClientQuote/55?ReportAsSource=true
```

---

### What is the quote object structure?

Client Quote structure:

**Endpoint:** `GET /api/ClientQuote/{id}`

**Object Structure:**
```
ClientQuote:
  UniqueId (long?) - Quote ID
  TaxNumber (long?) - Tax document number
  EssentialInfo - Company, Branch, Client, Employee
  PrintInfo - Print/display settings
  ProductsInfo - Products, prices, VAT
  ExternalInfo - External reference info
```

---

### What is the difference between quote and order?

**Quote vs Order:**

| Aspect | Quote | Order |
|--------|-------|-------|
| Binding | No | No (until invoice) |
| Purpose | Price proposal | Commitment to buy |
| API | Read-only (GET) | Read & Write |
| Next step | Order or Invoice | Invoice |

Quote is earlier in the sales funnel. Both have same data structure.

---
