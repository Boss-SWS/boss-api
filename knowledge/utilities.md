# 🔧 Utilities


### What utilities are available in Boss API?

**Available Utilities:**

• **ActorTableSize** - Get count of records for any entity

Useful for planning sync operations and pagination.

---

### How do I get the count of records for an entity?

Use ActorTableSize to get total count of any entity:

GET /api/ActorTableSize?Actor={ActorType}

**Actor Types:**
• Client
• Product
• Supplier
• Employee
• Branch
• and more...

**Use cases:**
• Know total records before sync
• Plan pagination strategy
• Monitor data growth

**Endpoint:** `GET /api/ActorTableSize?Actor={ActorType}`

**Example:**
```json
GET /api/ActorTableSize?Actor=Client
Response: 1500

GET /api/ActorTableSize?Actor=Product
Response: 8420

GET /api/ActorTableSize?Actor=Supplier
Response: 85
```

---
