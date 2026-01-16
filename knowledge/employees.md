# 👔 Employees


### Tell me about Employees

**Employees (עובדים)**

Employee records in Boss system.

**Endpoints:**
• GET /api/Employee/{id} - Get employee by ID
• GET /api/Employee - Search employees

**Structure:**
• UniqueId - Employee ID
• Name - Employee name
• Branch - Associated branch
• Active - Is active

**Usage:**
• Required in EssentialInfo for movements
• Used for tracking who created documents

---

### How do I get an employee by ID?

Get employee: GET /api/Employee/{id}

**Endpoint:** `GET /api/Employee/{id}`

**Example:**
```json
GET /api/Employee/5
```

---
