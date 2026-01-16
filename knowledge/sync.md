# 🔄 Sync & Pagination


### How does TimeStamp sync work?

TimeStamp enables efficient delta sync - fetch only changes since last sync.

**Parameters:**
• Phrase - search text (not for sync)
• StartPoint - offset, starts from 0
• MaxResults - max 50, default 10 (use 0 for count only)
• TimeStampMin - lower bound (last saved value)
• TimeStampMax - upper bound (freeze current loop)

**The Algorithm:**
1. First sync: TimeStampMin=empty, TimeStampMax=empty
2. Save HighestTimeStamp from response
3. Same loop (pagination): use TimeStampMax=saved value
4. Next sync: TimeStampMin=saved value, TimeStampMax=empty

**Endpoint:** `GET /api/client?StartPoint=0&MaxResults=50&TimeStampMin=&TimeStampMax=`

**Example:**
```json
=== FIRST SYNC ===
GET /api/client?StartPoint=0&MaxResults=50&TimeStampMin=&TimeStampMax=
Response: { Results: [...], MaxAvailableResults: 120, HighestTimeStamp: 1000 }
→ Save HighestTimeStamp: 1000

=== SAME SYNC - PAGE 2 ===
GET /api/client?StartPoint=50&MaxResults=50&TimeStampMin=&TimeStampMax=1000
(TimeStampMax freezes the window - no new changes included)

=== SAME SYNC - PAGE 3 ===
GET /api/client?StartPoint=100&MaxResults=50&TimeStampMin=&TimeStampMax=1000

=== NEXT SYNC (later) ===
GET /api/client?StartPoint=0&MaxResults=50&TimeStampMin=1000&TimeStampMax=
→ Returns only records changed since TimeStamp 1000
→ Save new HighestTimeStamp: 1050
```

---

### Why is TimeStamp important?

Boss serves companies with large databases that update constantly. Without TimeStamp, you'd need to fetch ALL records every time - slow and wasteful.

**With TimeStamp:**
• First sync: Get everything once
• Next syncs: Get only changes (delta)
• Much faster and efficient

⚠️ **Polling is NOT allowed** - always use TimeStamp for sync operations.

**Endpoint:** `GET /api/client or /api/product`

---

### What is StartPoint and MaxResults?

Pagination parameters for fetching large datasets:

**StartPoint** - Offset (starts from 0)
**MaxResults** - Records per page (max 50, default 10)

**Special:** MaxResults=0 returns only TotalResults count without data.

**Loop until:** StartPoint >= MaxAvailableResults

**Endpoint:** `GET /api/client?StartPoint=0&MaxResults=50`

**Example:**
```json
Total records: 180

Page 1: StartPoint=0, MaxResults=50 → returns 50
Page 2: StartPoint=50, MaxResults=50 → returns 50
Page 3: StartPoint=100, MaxResults=50 → returns 50
Page 4: StartPoint=150, MaxResults=50 → returns 30 (remaining)

Loop ends when StartPoint (200) >= MaxAvailableResults (180)
```

---

### What does the search response contain?

Every search/list response contains:

• **Results** - Array of records
• **MaxAvailableResults** - Total count matching query
• **HighestTimeStamp** - Save this for next sync!

**Endpoint:** `GET /api/client or /api/product`

**Example:**
```json
{
  "Results": [
    { "UniqueId": 123, "FirstName": "John", ... },
    { "UniqueId": 124, "FirstName": "Jane", ... }
  ],
  "MaxAvailableResults": 250,
  "HighestTimeStamp": 1050
}
```

---

### What are the field limits?

**Client limits:**
• Phones: max 4
• Categories: max 5
• Addresses: unlimited

**Product limits:**
• Categories: max 5
• Filters: max 10
• Price lists: max 5

**Field lengths:**
• FirstName: 20 chars
• LastName: 30 chars
• Email: 50 chars
• Product Name: 50 chars
• Barcode: 20 chars

**Object Structure:**
```
Client:
  Phones: max 4
  Categories: max 5
  Addresses: unlimited

Product:
  Categories: max 5
  Filters: max 10
  PriceLists: max 5

Field lengths:
  FirstName: 20
  LastName: 30
  Email: 50
  ProductName: 50
  Barcode: 20
```

---

### Show me a full sync example

Complete sync process with pagination and TimeStamp:

**Endpoint:** `GET /api/client`

**Example:**
```json
// === INITIAL SYNC ===
let savedTimeStamp = null;
let startPoint = 0;
let maxResults = 50;
let hasMore = true;

while (hasMore) {
  const url = `/api/client?StartPoint=${startPoint}&MaxResults=${maxResults}&TimeStampMin=&TimeStampMax=${savedTimeStamp || ''}`;
  const response = await fetch(url);
  const data = await response.json();
  
  // Process data.Results
  processRecords(data.Results);
  
  // Save TimeStamp from first page only
  if (startPoint === 0) {
    savedTimeStamp = data.HighestTimeStamp;
  }
  
  // Next page
  startPoint += maxResults;
  hasMore = startPoint < data.MaxAvailableResults;
}

// === NEXT SYNC (delta) ===
// Use savedTimeStamp as TimeStampMin
const url = `/api/client?StartPoint=0&MaxResults=50&TimeStampMin=${savedTimeStamp}&TimeStampMax=`;
// This returns only records changed since last sync
```

---
