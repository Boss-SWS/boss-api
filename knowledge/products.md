# 📦 Products & Related Entities

> Product ecosystem: Product (base) → ProductBranch (inventory), ProductSupplier (purchasing), ProductCompany (company settings), Category, Unit, Filter, Manufacturer


### What is the Product structure?

Product: UniqueId, Active, Name, UnitAmount, Unit, Category (array), Filter (array), Barcode, Manufacturer, ShortDescription, CostVatExc (cost excl VAT), VatFree, Price (array), ProductBranch (inventory), ExternalInfo, ImageURL.

**Endpoint:** `GET /api/Product/{id}`

---

### How do I create a new product?

Create product: POST /api/Product. Required: Name. Recommended: Active, Unit, Category, Price. Returns UniqueId of new product.

**Endpoint:** `POST /api/Product`

---

### How do I search for products?

Search products: GET /api/Product. Parameters: Phrase (text), StartPoint, MaxResults, TimeStampMin/Max (sync), ExectMatch (exact barcode match).

**Endpoint:** `GET /api/Product`

**Example:**
```json
GET /api/Product?Phrase=shirt&MaxResults=20
GET /api/Product?Phrase=7290001234567&ExectMatch=true
```

---

### How do I update a product?

Update product: PUT /api/Product/{UniqueId}. Send only fields to update.

**Endpoint:** `PUT /api/Product/{id}`

---

### How do I delete a product?

Delete product: DELETE /api/Product/{UniqueId}. Warning - cannot be undone!

**Endpoint:** `DELETE /api/Product/{id}`

---

### What is ProductBranch?

ProductBranch links a product to a branch - contains inventory info (StorageAmounts: Current, Min, Max) and whether product is active in branch. Each product can have multiple ProductBranch - one per branch.

**Endpoint:** `GET /api/ProductBranch/{id}`

---

### How do I get product inventory in a branch?

Get inventory: GET /api/ProductBranch/0?ProductUniqueId={productId}&BranchUniqueId={branchId}. Use id=0 with params to search.

**Endpoint:** `GET /api/ProductBranch/0`

---

### How do I update inventory?

Update inventory: 1) Get current ProductBranch, 2) PUT /api/ProductBranch/{id} with new StorageAmounts.Current.

**Endpoint:** `PUT /api/ProductBranch/{id}`

---

### How do I create a new ProductBranch?

Create product-branch link: POST /api/ProductBranch. Required when adding product to new branch or setting initial inventory.

**Endpoint:** `POST /api/ProductBranch`

---

### What is ProductSupplier?

ProductSupplier links a product to a supplier - defines from which supplier to buy, purchase price, supplier's SKU. Each product can have multiple suppliers.

**Endpoint:** `GET /api/ProductSupplier/{id}`

---

### How do I link a product to a supplier?

Link product-supplier: POST /api/ProductSupplier with ProductUniqueId and SupplierUniqueId.

**Endpoint:** `POST /api/ProductSupplier`

---

### How do I get all suppliers for a product?

Product suppliers: GET /api/ProductSupplier/0?ProductUniqueId={productId}. Returns all supplier links.

**Endpoint:** `GET /api/ProductSupplier/0`

---

### What is ProductCompany?

ProductCompany - product settings at company level. Read-only: GET /api/ProductCompany/0?ProductUniqueId={id}&CompanyUniqueId={id}.

**Endpoint:** `GET /api/ProductCompany/{id}`

---

### What is ProductUnit?

ProductUnit - units of measurement (piece, kg, liter, meter, etc.). Full CRUD: GET/POST/PUT/DELETE /api/ProductUnit.

**Endpoint:** `GET /api/ProductUnit`

---

### What is Category?

Category - product categories (or client/supplier categories by ActorType). Fields: UniqueId, Name, TableHandle, Region, Priority. CRUD: GET/POST/PUT/DELETE /api/Category.

**Endpoint:** `GET /api/Category`

---

### What is the Price array in Product?

Price is an array of prices - each has VatExc (excl VAT), VatInc (incl VAT), and TableHandle (price list). Allows multiple price lists per product (retail, wholesale, VIP, etc.).

**Endpoint:** `GET /api/Product/{id}`

---

### What is Filter in Product?

Filter - product attributes/properties (color, size, material, etc.). Each Filter has UniqueId, Name, Value. Enables filtering and searching by attributes.

**Endpoint:** `GET /api/Product/{id}`

---

### What is the product hierarchy in the system?

Hierarchy: Product (base) → ProductBranch (inventory per branch), ProductSupplier (link to suppliers), ProductCompany (company settings). Product contains: Unit, Category, Filter, Manufacturer, Price.

---

### What is Manufacturer?

Manufacturer - product manufacturer/brand. Fields: UniqueId, Name. Defined within the Product itself.

---

### What is ExternalInfo in Product?

ExternalInfo - info for external use (e-commerce). Fields: Status (site status), Name (display name), NoteUniqueId (notes).

---
