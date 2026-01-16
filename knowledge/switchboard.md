# 📄 Switchboard/PBX Integration


### Switchboard integration requirements

Boss Switchboard Integration is a two-way integration:

**1. Switchboard → Boss (Push)**
When a call rings, the switchboard POSTs to Boss endpoint to trigger caller popup.

**2. Boss → Switchboard (Pull)**
Boss queries the switchboard's API to retrieve call history.

**What YOU (vendor) provide:**
• Call Logs API (Boss calls this)
• Ring Event notifications (you POST to Boss)

**Proven integrations:** Voicenter, OmniTelecom, 1Plus

---

### What API endpoints must the switchboard vendor provide?

The switchboard vendor must provide these endpoints:

**1. Health Check**
GET /health - Simple availability check

**2. Call Logs by Station**
GET /calls/station/{station_id} - Returns call history for a Boss station

**3. Call Logs by Phone Numbers**
GET /calls/entity?phones=... - Returns calls for specific phone numbers

**Technical Requirements:**
• HTTPS (TLS 1.2+)
• JSON format
• OAuth2 or API Key authentication
• Pagination support (offset-based)
• Newest calls first (LIFO)

**Endpoint:** `GET /health
GET /calls/station/{station_id}
GET /calls/entity?phones={phones}`

---

### What is the call record structure?

Each call record must include these fields:

**Example:**
```json
{
  "call_id": "CALL-789456",
  "line_id": "LINE-001",
  "timestamp": "2025-01-15T10:25:00Z",
  "direction": "inbound",
  "caller_number": "+972501234567",
  "called_number": "+97237654321",
  "duration_seconds": 180,
  "status": "answered",
  "recording_url": "https://domain.com/recordings/CALL-789456.mp3"
}
```

**Object Structure:**
```
call_id (string, required) - Unique call identifier
line_id (string, required) - Line that handled the call
timestamp (datetime, required) - Call start time (ISO 8601)
direction (string, required) - 'inbound' or 'outbound'
caller_number (string, required) - Caller's phone number
called_number (string, required) - Number that was dialed
duration_seconds (integer, required) - Duration in seconds (0 if not answered)
status (string, required) - answered, missed, no_answer, busy, voicemail, failed
recording_url (string, optional) - URL to call recording
```

---

### How do I send ring event notifications to Boss?

When a call starts ringing, POST to Boss endpoint to trigger the caller popup:

**Endpoint:** POST /api/ExtVendors/{VendorName}/CallEvent

**Authentication options:**
1. Query params: ?username={user}&password={pass}&boss_unique_id={id}
2. OAuth2 Bearer token in header

**Important:** Send the notification as SOON as the call rings, BEFORE it's answered.

**Response:** HTTP 200 on success

**Endpoint:** `POST https://api.boss-sws.com/api/ExtVendors/{VendorName}/CallEvent`

**Example:**
```json
{
  "event_type": "ring",
  "line_id": "LINE-001",
  "call_id": "CALL-789456",
  "timestamp": "2025-01-15T10:25:00Z",
  "direction": "inbound",
  "caller_number": "+972501234567",
  "called_number": "+97237654321"
}
```

---

### What is LineId vs StationId?

**LineId** - Your switchboard's identifier for a phone line (e.g., LINE-001, EXT-5001)

**StationId** - Boss workstation identifier. One station can have multiple lines (e.g., desk phone + mobile).

**BossUniqueId** - Unique identifier for a Boss customer account.

**Binding:** The mapping between LineIds and StationIds is configured in Boss during setup. This determines which station receives the popup when a specific line rings.

---

### What are the call status values?

Call status values:

• **answered** - Call was connected and answered
• **missed** - Inbound call was not answered
• **no_answer** - Outbound call was not answered
• **busy** - Line was busy
• **voicemail** - Call went to voicemail
• **failed** - Call failed to connect

---

### How does call logs pagination work?

Call logs API uses offset-based pagination:

**Parameters:**
• offset (default: 0) - Records to skip
• limit (default: 50, max: 100) - Records to return
• from_date (optional) - Filter from date
• to_date (optional) - Filter until date

**Response includes:**
• calls[] - Array of call records
• pagination.offset - Current offset
• pagination.limit - Current limit
• pagination.total - Total records
• pagination.has_more - Boolean for more pages

**Endpoint:** `GET /calls/station/{station_id}?offset=0&limit=50`

**Example:**
```json
{
  "calls": [...],
  "pagination": {
    "offset": 0,
    "limit": 50,
    "total": 150,
    "has_more": true
  }
}
```

---

### What optional features can switchboard vendors provide?

Optional features that enhance the integration:

• **Call Recordings** - Provide recording URLs in call records
• **Additional Events** - Beyond 'ring': answered, ended, transferred (Boss logs these but only 'ring' triggers popup)
• **Webhook Configuration** - API for Boss to register/unregister for notifications

---

### What is the switchboard integration setup process?

Integration setup process:

1. Contact Boss to receive VendorName and API credentials
2. Implement Call Logs API endpoints
3. Implement Ring Event notifications to Boss
4. Provide Boss with your API base URL and auth details
5. Configure line-to-station mapping in Boss
6. Test in sandbox environment
7. Go live

---
