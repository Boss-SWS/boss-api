# 👤 Clients


### How do I search for clients?

Search clients: GET /api/client with parameters: Phrase (searches Last/First Name, Email, Phone, Club Member Numbers), StartPoint (offset, default 0), MaxResults (max results, default 10, max 50). Results ordered by ABC of names.

**Endpoint:** `GET /api/client`

**Example:**
```json
GET /api/client?Phrase=John&StartPoint=0&MaxResults=50

Response:
{
  "Results": [{client objects}],
  "MaxAvailableResults": 180,
  "HighestTimeStamp": 1000
}
```

---

### How do I get a client by ID?

Get client by UniqueId: GET /api/client/{UniqueId}. The ID is sent in the URL path.

**Endpoint:** `GET /api/client/{UniqueId}`

**Example:**
```json
GET /api/client/27

Returns full client object
```

---

### How do I create a new client?

Create client: POST /api/client. Required: AssociatedBranch.UniqueId, StoragePriceNum. Recommended: LastName, FirstName, Email, Phone, DateOfBirth (format: yyyy-mm-ddTHH:mm:ss), Category.

**Endpoint:** `POST /api/client`

**Example:**
```json
POST /api/client
{
  "LastName": "Daw",
  "FirstName": "John",
  "Email": "john@email.com",
  "AssociatedBranch": {"UniqueId": "1"},
  "StoragePriceNum": "3",
  "Phone": [{"Number": "052-1234567", "Description": "Mobile", "TableHandle": {"Operation": "NewFirstEmpty"}}]
}

Response: {"UniqueId": 123}
```

---

### How do I update a client?

Update client: PUT /api/client/{ClientUniqueId}. Send only fields you want to update. For phones and categories use TableHandle.

**Endpoint:** `PUT /api/client/{ClientUniqueId}`

**Example:**
```json
PUT /api/client/14
{
  "FirstName": "Johnny",
  "Phone": [{"Number": "056-5556667", "Description": "Office", "TableHandle": {"Index": "1", "Operation": "Update"}}]
}
```

---

### How does client authentication by email work?

Client authentication: GET /api/client?Email={email}&PassWord={password}&UniqueId={id}. Used to let clients access their profile on ecommerce sites. UniqueId is optional - used when multiple cards have same email.

**Endpoint:** `GET /api/client`

**Example:**
```json
GET /api/client?Email=john@email.com&PassWord=MyPass123
```

---

### What is the client object structure?

Client object: UniqueId, LastName (max 30), FirstName (max 20), Email (max 50), HasPassWord, PassWord (max 10), DateOfBirth, Phone[4], Address, HasMultipleAddresses, Category[5], AssociatedBranch, StoragePriceNum, Balance, SmsAllowed, NoteUniqueId, NumberOfMovements.

**Endpoint:** `GET /api/client/{id}`

**Object Structure:**
```
UniqueId: int64
LastName: string (max 30)
FirstName: string (max 20)
Email: string (max 50)
HasPassWord: boolean
PassWord: string (max 10)
DateOfBirth: string (ddMMyyyy)
Phone[4]: array of {Number, Description, TableHandle}
Address: object
HasMultipleAddresses: boolean
Category[5]: array
AssociatedBranch: {UniqueId, Name}
StoragePriceNum: string
Balance: number
SmsAllowed: boolean
```

---

### How do I get client movements?

Client movements: GET /api/ClientMoveMents?ClientUniqueId={id}&StartPoint=0&MaxResults=10. Returns array of movements (invoices, receipts, orders etc.) with CreationDateTime, ActorType, ActorUniqueId, TotalVatInc.

**Endpoint:** `GET /api/ClientMoveMents`

**Example:**
```json
GET /api/ClientMoveMents?ClientUniqueId=3&StartPoint=0&MaxResults=10
```

---
