# 📊 Inventory & Stock


### How do I update inventory?

Multi-branch system - inventory managed per branch via ProductBranch. Steps: 1) GET /api/ProductBranch/0?ProductUniqueId={id}&BranchUniqueId={id} 2) Update StorageAmounts.Current 3) PUT /api/ProductBranch/{id}

**Endpoint:** `PUT /api/ProductBranch/{id}`

**Example:**
```json
GET /api/ProductBranch/0?ProductUniqueId=123&BranchUniqueId=1

Then PUT with updated StorageAmounts.Current
```

---

### What is the ProductBranch structure?

ProductBranch links a product to a branch and contains inventory info. Structure: UniqueId, Active (boolean), ProductInfo (product UniqueId), BranchInfo (UniqueId, Name, StorageAmounts).

**Endpoint:** `GET /api/ProductBranch/{id}`

**Object Structure:**
```
ProductBranch:
  UniqueId: int64
  Active: boolean
  ProductInfo:
    UniqueId: int64 (product ID)
  BranchInfo:
    UniqueId: int64 (branch ID)
    Name: string
    StorageAmounts:
      Current: integer
      Min: integer
      Max: integer
```

---

### How do I get inventory for a specific product?

Get inventory: GET /api/ProductBranch/0?ProductUniqueId={productId}&BranchUniqueId={branchId}. Use id=0 with product and branch params to get the specific link.

**Endpoint:** `GET /api/ProductBranch/0`

**Example:**
```json
GET /api/ProductBranch/0?ProductUniqueId=123&BranchUniqueId=1
```

---

### What is StorageAmounts?

StorageAmounts contains 3 fields: Current (current stock quantity), Min (minimum - alert threshold), Max (maximum - capacity limit).

**Endpoint:** `GET /api/ProductBranch/{id}`

---

### How do I create a new product-branch link?

Create ProductBranch: POST /api/ProductBranch with ProductInfo.UniqueId and BranchInfo.UniqueId.

**Endpoint:** `POST /api/ProductBranch`

---
