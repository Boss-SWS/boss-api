# 📄 Movements Overview


### What are the movement types in the system?

The system supports 3 movement types: 1) Client Movements - sales to customers (invoices, receipts, orders), 2) Supplier Movements - purchases from vendors (supplier invoices, credits), 3) Branch Movements - transfers between branches.

**Object Structure:**
```
Movement Types:

1. CLIENT MOVEMENTS (Sales):
   - ClientInvoice
   - ClientReceipt
   - ClientInvoiceReceipt
   - ClientCrediting
   - ClientOrder
   - ClientQuote
   - ClientDelivery
   - ClientSumInvoice

2. SUPPLIER MOVEMENTS (Purchases):
   - SupplierInvoice
   - SupplierCrediting

3. BRANCH MOVEMENTS (Transfers):
   - BranchDelivery
   - BranchReceiveDelivery
```

---

### What's the difference between ClientInvoice and SupplierInvoice?

ClientInvoice - sales document to customer (you sell). SupplierInvoice - purchase document from supplier (you buy). Both use EssentialInfo and ProductsInfo but with different fields (Client vs Supplier).

---

### How do I transfer inventory between branches?

Branch transfer requires 2 movements: 1) BranchDelivery - outgoing shipment from source branch, 2) BranchReceiveDelivery - receipt at target branch. Both linked via ChainInfo.

**Endpoint:** `POST /api/BranchDelivery, POST /api/BranchReceiveDelivery`

---

### What is ChainInfo?

ChainInfo links related movements. For example: Order → Delivery Note → Invoice. Or: Branch Shipment → Branch Receipt. Allows tracking the document chain.

---

### How do I get all movements for a client?

Get client movements: GET /api/ClientMoveMents?ClientUniqueId={id}. Returns list of REFERENCES (ActorType + UniqueId) - NOT the actual documents! You need to fetch each document separately.

**Endpoint:** `GET /api/ClientMoveMents`

**Example:**
```json
Step 1 - Get references:
GET /api/ClientMoveMents?ClientUniqueId=123

Response:
[
  {"ActorType": "ClientInvoice", "UniqueId": 456},
  {"ActorType": "ClientReceipt", "UniqueId": 789}
]

Step 2 - Fetch each:
GET /api/ClientInvoice/456
GET /api/ClientReceipt/789
```

---

### What is ActorType in movements?

ActorType indicates the document type. Possible values: ClientInvoice, ClientReceipt, ClientInvoiceReceipt, ClientCrediting, ClientOrder, ClientQuote, ClientDelivery, ClientSumInvoice. Used to build the correct endpoint.

**Example:**
```json
ActorType → Endpoint:
ClientInvoice → GET /api/ClientInvoice/{id}
ClientReceipt → GET /api/ClientReceipt/{id}
ClientOrder → GET /api/ClientOrder/{id}
```

---

### What are all movement endpoints?

Client: ClientInvoice, ClientReceipt, ClientInvoiceReceipt, ClientCrediting, ClientOrder, ClientQuote, ClientDelivery, ClientSumInvoice. Supplier: SupplierInvoice, SupplierCrediting. Branch: BranchDelivery, BranchReceiveDelivery.

---

### What is ClientSumInvoice?

ClientSumInvoice - consolidated invoice. Combines multiple delivery notes into one invoice. Read-only: GET /api/ClientSumInvoice/{id}.

**Endpoint:** `GET /api/ClientSumInvoice/{id}`

---

### What is ExternalMoveMent?

ExternalMoveMent - movements created from external sources (Wix, WooCommerce, Wolt, etc.). Read: GET /api/ExternalMoveMent/{id}.

**Endpoint:** `GET /api/ExternalMoveMent/{id}`

---

### What is the full sales flow for a client?

Sales flow: Quote → Order → Delivery → Invoice → Receipt. Or: InvoiceReceipt (combined). Refund: Crediting. Documents linked via ChainInfo.

**Object Structure:**
```
SALES FLOW:

Quote → Order → Delivery → Invoice → Receipt
                              ↓
                      OR InvoiceReceipt

Refund: Crediting
```

---

### Which documents can I create (POST) vs read-only (GET)?

POST (create): ClientOrder, ClientInvoice, ClientReceipt, ClientInvoiceReceipt, ClientCrediting, SupplierInvoice, SupplierCrediting, BranchDelivery, BranchReceiveDelivery. GET only: ClientQuote, ClientDelivery, ClientSumInvoice.

---
