# 🏢 Branches & Company

> Branch and Company are read-only entities. Hierarchy: Company → Branches


### What is the Branch structure?

Branch: UniqueId, Name, Company (parent company), Address, Phone (array), BookKeepingCode, NoteUniqueId, NumOfMoveMents.

**Endpoint:** `GET /api/Branch/{id}`

---

### How do I get a list of branches?

List branches: GET /api/Branch. Parameters: Phrase, StartPoint, MaxResults, TimeStampMin/Max. Note: Read-only - cannot create/update branches via API.

**Endpoint:** `GET /api/Branch`

---

### How do I get a branch by ID?

Get branch: GET /api/Branch/{UniqueId}.

**Endpoint:** `GET /api/Branch/{id}`

---

### What is the Company structure?

Company: UniqueId, BossUniqueId (Boss system ID), Name, Id (external IDs like business registration), Address, HasMultipleAddresses, NoteUniqueId.

**Endpoint:** `GET /api/Company/{id}`

---

### How do I get a list of companies?

List companies: GET /api/Company. Note: Read-only - cannot create/update companies via API.

**Endpoint:** `GET /api/Company`

---

### What is the Company-Branch hierarchy?

Hierarchy: Company → Branches. Each company can have multiple branches. Each branch belongs to one company.

---

### Why can't I create a branch/company via API?

Branches and companies are administrative entities created in the Boss system itself. The API provides read-only access because creation requires many settings (licensing, accounting, VAT settings, etc.).

---
