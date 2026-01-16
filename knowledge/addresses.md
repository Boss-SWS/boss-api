# 📍 Addresses & Locations


### What are Addresses in the system?

**Addresses & Locations (כתובות)**

Addresses can be attached to any entity (Client, Supplier, Product, Category).

**Endpoints:**
• GET /api/address - Search by ActorType & ActorUniqueId
• POST /api/address - Create address
• PUT /api/address/{id} - Update address
• DELETE /api/address/{id} - Delete address

**Related APIs:**
• /api/city - City search/CRUD
• /api/street - Street search/CRUD (requires CityUniqueId)

**Key fields:** City, Street, HouseNum, ApartmentNum, FloorNum, Entrance, ZipCode

---

### How do I search for an address by type and ID?

Search address: GET /api/address?ActorType={type}&ActorUniqueId={id}. ActorType can be Client, Category, Product, Supplier, etc.

**Endpoint:** `GET /api/address`

**Example:**
```json
GET /api/address?ActorType=Client&ActorUniqueId=3
```

---

### How do I create a new address?

Create address: POST /api/address. Required: ActorType, ActorUniqueId. Optional: City.UniqueId, Street.UniqueId, HouseNum (max 5), ApartmentNum, FloorNum, Entrance, ZipCode (max 10).

**Endpoint:** `POST /api/address`

**Example:**
```json
POST /api/address
{
  "ActorType": "Client",
  "ActorUniqueId": "3",
  "City": {"UniqueId": "427"},
  "Street": {"UniqueId": "44239"}
}
```

---

### How do I update an address?

Update address: PUT /api/address/{UniqueId}. Send the fields to update.

**Endpoint:** `PUT /api/address/{UniqueId}`

**Example:**
```json
PUT /api/address/345
{
  "ZipCode": "12345"
}
```

---

### How do I delete an address?

Delete address: DELETE /api/address/{Id}. The ID is sent in the URL path.

**Endpoint:** `DELETE /api/address/{Id}`

**Example:**
```json
DELETE /api/address/54
```

---

### How do I search for cities?

Search cities: GET /api/city?Phrase={search}&StartPoint=0&MaxResults=10. Create city: POST /api/city with Name. Update: PUT /api/city/{UniqueId}. Delete: DELETE /api/city/{UniqueId}.

**Endpoint:** `GET /api/city`

**Example:**
```json
GET /api/city?Phrase=Tel-Aviv&MaxResults=10
```

---

### How do I search for streets?

Search streets: GET /api/street?CityUniqueId={cityId}&Phrase={search}. CityUniqueId is required. Create street: POST /api/street with City.UniqueId and Name.

**Endpoint:** `GET /api/street`

**Example:**
```json
GET /api/street?CityUniqueId=3&Phrase=Herzl&MaxResults=10
```

---
