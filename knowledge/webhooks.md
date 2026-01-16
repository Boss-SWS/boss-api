# 🔔 WebHooks


### How do I create a delivery WebHook?

Create WebHook: POST /api/WebHook. Parameters: Type (UpdateDelivering), CallBackAddress (URL to receive notification), AppHandle (optional identifier). When new delivery created, system sends POST to your URL.

**Endpoint:** `POST /api/WebHook`

**Example:**
```json
POST /api/WebHook
{
  "Type": "UpdateDelivering",
  "CallBackAddress": "https://yoursite.com/webhook/delivery",
  "AppHandle": "1234"
}
```

---

### What is the WebHook notification structure?

WebHook notification contains: Type, UniqueId, AppHandle, Delivering (full object with UniqueId, Status, StorageStatus, DeliveringRoute, DueDateAndTime, Duration, Address, ActorType, ActorUniqueId).

**Endpoint:** `POST /api/WebHook`

---

### When should I use WebHooks?

WebHooks for real-time updates. Instead of polling (not allowed!), system notifies you of changes. Common use: connecting to external delivery system - when seller creates delivery, info sent automatically to delivery platform.

**Endpoint:** `POST /api/WebHook`

---
