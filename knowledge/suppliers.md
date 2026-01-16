# 🏭 Suppliers


### How do I create a new supplier?

Create supplier: POST /api/Supplier. Main fields: Name (required), Email, Phone (array), Address, Category (array).

**Endpoint:** `POST /api/Supplier`

**Example:**
```json
POST /api/Supplier
{
  "Name": "Electronics Supplier Ltd",
  "Email": "info@supplier.com",
  "Phone": [{"Number": "03-1234567", "Description": "Main"}],
  "Category": [{"UniqueId": 1}]
}
```

---

### What is the Supplier object structure?

Supplier structure: UniqueId, Name, Email, Phone (array), Address, HasMultipleAddresses, Category (array), Balance, Id (external IDs), NoteUniqueId, NumOfMoveMents.

**Endpoint:** `GET /api/Supplier/{id}`

---

### How do I search for suppliers?

Search suppliers: GET /api/Supplier. Parameters: Phrase (search text), StartPoint, MaxResults, TimeStampMin/Max (for sync).

**Endpoint:** `GET /api/Supplier`

**Example:**
```json
GET /api/Supplier?Phrase=electronics&MaxResults=20
```

---

### How do I update a supplier?

Update supplier: PUT /api/Supplier/{UniqueId}. Send only the fields you want to update.

**Endpoint:** `PUT /api/Supplier/{id}`

**Example:**
```json
PUT /api/Supplier/123
{
  "Email": "new-email@supplier.com"
}
```

---

### How do I delete a supplier?

Delete supplier: DELETE /api/Supplier/{UniqueId}.

**Endpoint:** `DELETE /api/Supplier/{id}`

**Example:**
```json
DELETE /api/Supplier/123
```

---

### How do I create a supplier invoice?

Create supplier invoice: POST /api/SupplierInvoice. Contains EssentialInfo (supplier, date, branch), ProductsInfo (purchased products), ExternalInfo.

**Endpoint:** `POST /api/SupplierInvoice`

---

### How do I create a supplier credit note?

Create supplier credit: POST /api/SupplierCrediting. Similar structure to supplier invoice - EssentialInfo with supplier details and products to return.

**Endpoint:** `POST /api/SupplierCrediting`

---

### How do I link a product to a supplier?

Link product-supplier: POST /api/ProductSupplier. Allows defining which supplier provides which product, including purchase price and catalog number.

**Endpoint:** `POST /api/ProductSupplier`

**Example:**
```json
POST /api/ProductSupplier
{
  "ProductUniqueId": 456,
  "SupplierUniqueId": 123
}
```

---

### How do I get all products from a supplier?

Get supplier's products: GET /api/ProductSupplier/0?SupplierUniqueId={supplierId}. Returns all products linked to the supplier.

**Endpoint:** `GET /api/ProductSupplier/0`

**Example:**
```json
GET /api/ProductSupplier/0?SupplierUniqueId=123&MaxResults=100
```

---
