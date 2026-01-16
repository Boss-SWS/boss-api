# 🔗 API Endpoints Reference


### What are all client endpoints?

Client endpoints: GET /api/Client (search), POST /api/Client (create), GET /api/Client/{id} (get by ID), PUT /api/Client/{id} (update), DELETE /api/Client/{id} (delete).

**Object Structure:**
```
GET /api/Client - Search
POST /api/Client - Create
GET /api/Client/{id} - Get
PUT /api/Client/{id} - Update
DELETE /api/Client/{id} - Delete
```

---

### What are all product endpoints?

Product endpoints: GET /api/Product (search), POST /api/Product (create), GET /api/Product/{id} (get), PUT /api/Product/{id} (update), DELETE /api/Product/{id} (delete). ProductBranch for inventory.

**Object Structure:**
```
Product:
GET/POST/PUT/DELETE /api/Product

ProductBranch (Inventory):
GET/POST/PUT/DELETE /api/ProductBranch
```

---

### What are all document endpoints?

Document endpoints: ClientInvoice, ClientReceipt, ClientInvoiceReceipt, ClientOrder, ClientQuote, ClientCrediting, SupplierInvoice, SupplierCrediting. All support GET by ID, most support POST for creation.

---

### What are all address-related endpoints?

Address: GET/POST/PUT/DELETE /api/Address. City: GET/POST/PUT/DELETE /api/City. Street: GET/POST/PUT/DELETE /api/Street. Zone, Neighborhood for areas.

---

### What are all task endpoints?

Task: GET/POST/PUT/DELETE /api/Task. TaskNote for notes, TaskAttachment for files, TaskLog for history.

---

### What are employee, supplier, branch endpoints?

Employee: full CRUD at /api/Employee. Supplier: full CRUD at /api/Supplier. Branch: read-only GET /api/Branch. Company: read-only GET /api/Company.

---

### What is the API base URL?

Base URL: https://portal.boss-sws.com:30034 (or your portal address). All endpoints start from this, e.g.: https://portal.boss-sws.com:30034/api/Client

---
