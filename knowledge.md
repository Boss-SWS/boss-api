# Boss API Assistant - Knowledge Base

## Instructions for AI

You are the **Boss API Assistant** - an expert on Boss Software Solutions WebAPI.

### Your Role:
- Help users integrate with Boss ERP/CRM/POS system via API
- Answer questions about endpoints, parameters, and data structures
- Provide code examples when helpful
- Support both **Hebrew** and **English** questions

### Rules:
1. Answer ONLY based on this documentation
2. If something is not documented here, say "This is not covered in the current documentation"
3. Always mention the HTTP method and endpoint path
4. Show JSON examples for complex requests
5. Be concise but complete

---

# API Documentation

## Authentication

Boss API uses **HTTP Basic Authentication**.

**How to authenticate:**
- Include `Authorization` header in every request
- Format: `Basic base64(username:password)`

**Example:**
```
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

**Getting credentials:** Contact Boss support for API credentials. Each user has specific permissions.

**Note:** All endpoints require authentication.

**401 Unauthorized** means:
- Wrong username or password
- Missing Authorization header
- Incorrect Base64 encoding
- User account disabled
- Missing permissions for endpoint

---

## Clients (לקוחות)

Customer records in Boss system.

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Client/{id} | Get client by ID |
| GET | /api/Client | Search clients |
| POST | /api/Client | Create client |
| PUT | /api/Client/{id} | Update client |
| DELETE | /api/Client/{id} | Delete client |

### Search Parameters:
- `StartPoint` - Pagination start (default: 0)
- `MaxResults` - Max results to return
- `TimeStamp` - For sync (returns records changed after this value)
- `Phrase` - Search text
- `Phone` - Search by phone number
- `Email` - Search by email

### Client Object Fields:
- `UniqueId` (long) - Client ID
- `Name` (string) - Client name
- `Email` (string) - Email address
- `Phone` (array) - **ARRAY of phone objects - UNLIMITED**
- `PriceListUniqueId` (long) - Assigned price list
- `Active` (bool) - Is active
- `NoteUniqueId` (long) - Linked note
- `AddressUniqueId` (long) - Primary address
- `HasMultipleAddresses` (bool) - Has additional addresses
- `Category` (array) - **ARRAY - multiple categories allowed**
- `RewardPoints` (object) - Loyalty points info

### Phone Array Structure:
```json
"Phone": [
  { "UniqueId": 1, "Number": "03-1234567", "Type": "Office" },
  { "UniqueId": 2, "Number": "050-1234567", "Type": "Mobile" },
  { "UniqueId": 3, "Number": "052-9876543", "Type": "Home" }
]
```

### Example - Create Client:
```json
POST /api/Client
{
  "Name": "John Doe",
  "Email": "john@example.com",
  "Phone": [
    { "Number": "03-1234567", "Type": "Office" },
    { "Number": "050-1234567", "Type": "Mobile" }
  ],
  "Active": true,
  "Category": [
    { "UniqueId": 1, "Name": "VIP" },
    { "UniqueId": 2, "Name": "Retail" }
  ]
}
```

---

## Employees (עובדים)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Employee/{id} | Get employee by ID |
| GET | /api/Employee | Search employees |
| POST | /api/Employee | Create employee |
| PUT | /api/Employee/{id} | Update employee |
| DELETE | /api/Employee/{id} | Delete employee |

### Employee Object Fields:
- `UniqueId` (long) - Employee ID
- `Name` (string) - Employee name
- `Branch` (object) - Associated branch
- `Phone` (array) - **ARRAY - UNLIMITED phones**
- `Active` (bool) - Is active
- `Permissions` (object) - Access permissions
- `Role` (object) - Employee role

### Related Endpoints:
- `/api/EmployeeAttendance` - Track attendance

**Usage:** Required in `EssentialInfo` for all movements (invoices, orders, etc.)

---

## Branches (סניפים)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Branch/{id} | Get branch by ID |
| GET | /api/Branch | List branches |

### Branch Object Fields:
- `UniqueId` (long) - Branch ID
- `Name` (string) - Branch name
- `Active` (bool) - Is active
- `Address` (object) - Branch address

---

## Companies (חברות)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Company/{id} | Get company by ID |
| GET | /api/Company | List companies |

---

## Suppliers (ספקים)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Supplier/{id} | Get supplier by ID |
| GET | /api/Supplier | Search suppliers |
| POST | /api/Supplier | Create supplier |
| PUT | /api/Supplier/{id} | Update supplier |
| DELETE | /api/Supplier/{id} | Delete supplier |

### Supplier Object Fields:
- `UniqueId` (long) - Supplier ID
- `Name` (string) - Supplier name
- `Email` (string) - Email
- `Phone` (array) - **ARRAY - UNLIMITED**
- `Active` (bool) - Is active

### Supplier Movements:
| Type | Hebrew | Endpoint |
|------|--------|----------|
| Supplier Invoice | חשבונית ספק | /api/SupplierInvoice |
| Supplier Credit | זיכוי ספק | /api/SupplierCrediting |

---

## Products (מוצרים)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Product/{id} | Get product by ID |
| GET | /api/Product | Search products |
| POST | /api/Product | Create product |
| PUT | /api/Product/{id} | Update product |
| DELETE | /api/Product/{id} | Delete product |

### Search Parameters:
- `StartPoint`, `MaxResults` - Pagination
- `TimeStamp` - Sync
- `Phrase` - Search text
- `Barcode` - Search by barcode
- `CategoryUniqueId` - Filter by category

### Product Object Structure:
```json
{
  "UniqueId": 100,
  "Active": true,
  "Name": "חולצה כחולה XL",
  "UnitAmount": 1,
  "Unit": { "UniqueId": 1, "Name": "יחידה" },
  "Category": [
    { "UniqueId": 1, "Name": "ביגוד", "Priority": 1 },
    { "UniqueId": 2, "Name": "גברים", "Priority": 2 }
  ],
  "Filter": [
    { "UniqueId": 1, "Name": "צבע", "Value": { "UniqueId": 1, "Name": "כחול" } },
    { "UniqueId": 2, "Name": "מידה", "Value": { "UniqueId": 4, "Name": "XL" } },
    { "UniqueId": 3, "Name": "משקל", "Value": { "UniqueId": 10, "Name": "200 גרם" } }
  ],
  "Barcode": "7290000000001",
  "Manufacturer": { "UniqueId": 1, "Name": "קסטרו" },
  "ShortDescription": "חולצת כותנה 100%",
  "CostVatExc": 50.00,
  "VatFree": false,
  "Price": [
    { "VatExc": 85.47, "VatInc": 100.00 },
    { "VatExc": 76.92, "VatInc": 90.00 }
  ]
}
```

### Key Points:
- **Category** - ARRAY, up to 5 tags (flat, no hierarchy)
- **Filter** - ARRAY, up to 10 filters (key:value attributes for variants like color, size)
- **Price** - ARRAY, multiple price lists supported

### Related Endpoints:
- `/api/ProductBranch` - Branch-specific product settings

---

## Inventory (מלאי)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Inventory | Get inventory levels |
| GET | /api/InventoryLog | Get inventory change log |

### Inventory Query Parameters:
- `BranchUniqueId` - Filter by branch
- `ProductUniqueId` - Filter by product

---

## Client Movements (תנועות לקוח)

All movements linking orders, deliveries, invoices, and receipts.

### Document Types:
| Type | Hebrew | Endpoint | Can Create |
|------|--------|----------|------------|
| Order | הזמנה | /api/ClientOrder | ✅ POST |
| Quote | הצעת מחיר | /api/ClientQuote | ❌ GET only |
| Delivery | תעודת משלוח | /api/ClientDelivery | ❌ GET only |
| Invoice | חשבונית מס | /api/ClientInvoice | ✅ POST |
| Invoice Receipt | חשבונית מס קבלה | /api/ClientInvoiceReceipt | ✅ POST |
| Receipt | קבלה | /api/ClientReceipt | ✅ POST |
| Credit | זיכוי | /api/ClientCrediting | ✅ POST |

### Movement Flow:
```
Quote → Order → Delivery → Invoice → Receipt
  ↓                           ↓
Credit (return)           Credit (refund)
```

### Get All Client Movements:
```
GET /api/ClientMoveMents?ClientUniqueId={id}
```

---

## Client Orders (הזמנות)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/ClientOrder/{id} | Get order by ID |
| GET | /api/ClientOrder | Search orders |
| POST | /api/ClientOrder | Create order |

### Create Order Example:
```json
POST /api/ClientOrder
{
  "EssentialInfo": {
    "Company": { "UniqueId": 1 },
    "Branch": { "UniqueId": 1 },
    "Client": { "UniqueId": 42 },
    "Employee": { "UniqueId": 2 }
  },
  "ProductsInfo": {
    "CalcWithVat": true,
    "VatPercentage": 17.0,
    "Product": [
      {
        "UniqueId": 5,
        "Name": "מוצר לדוגמה",
        "UnitValue": 100.0,
        "Amount": 2,
        "TotalValue": 200.0
      }
    ]
  }
}
```

---

## Client Quotes (הצעות מחיר)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/ClientQuote/{id} | Get quote by ID |

**Note:** Quotes are read-only via API. Create in Boss software.

---

## Client Deliveries (תעודות משלוח)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/ClientDelivery/{id} | Get delivery by ID |

**Note:** Deliveries are read-only via API. Create in Boss software.

---

## Invoices (חשבוניות)

### Create Invoice:
```json
POST /api/ClientInvoice
{
  "EssentialInfo": {
    "Company": { "UniqueId": 1 },
    "Branch": { "UniqueId": 1 },
    "Client": { "UniqueId": 42 },
    "Employee": { "UniqueId": 2 }
  },
  "ProductsInfo": {
    "CalcWithVat": true,
    "VatPercentage": 17.0,
    "TotalVatInc": 1170.0,
    "Product": [
      {
        "UniqueId": 5,
        "UnitValue": 1000.0,
        "Amount": 1,
        "TotalValue": 1000.0
      }
    ]
  }
}
```

### Create Invoice Receipt (Invoice + Payment):
```json
POST /api/ClientInvoiceReceipt
{
  "EssentialInfo": { ... },
  "ProductsInfo": { ... },
  "PaymentsInfo": {
    "Total": 1170.00,
    "Payment": [
      {
        "Type": "CreditCard",
        "Value": 1170.00,
        "CreditUniqueId": 31723
      }
    ]
  }
}
```

---

## Receipts (קבלות)

Payment-only document (no products).

### Create Receipt:
```json
POST /api/ClientReceipt
{
  "EssentialInfo": {
    "Company": { "UniqueId": 1 },
    "Branch": { "UniqueId": 1 },
    "Client": { "UniqueId": 42 },
    "Employee": { "UniqueId": 2 }
  },
  "PaymentsInfo": {
    "Total": 500.00,
    "Payment": [
      {
        "Type": "Cash",
        "Value": 500.00
      }
    ]
  }
}
```

### Payment Types:
| Type | Hebrew | Notes |
|------|--------|-------|
| Cash | מזומן | Direct payment |
| Check | צ'ק | Check payment |
| CreditCard | כרטיס אשראי | Requires CreditUniqueId |
| BankTransfer | העברה בנקאית | Bank transfer |

---

## Payments (תשלומים)

External payments (Credit Card, PayPal) must be created BEFORE creating a Receipt.

### Flow:
1. `POST /api/PaymentTransaction/CreditCard` → returns UniqueId
2. Use that UniqueId in Receipt's PaymentsInfo

### Credit Card Payment:
```json
POST /api/PaymentTransaction/CreditCard
{
  "Amount": 500.00,
  "CardNumber": "4111111111111111",
  "ExpiryMonth": 12,
  "ExpiryYear": 2025,
  "CVV": "123"
}
```
Response: `{ "UniqueId": 31723, "Success": true }`

### PayPal Payment:
```
POST /api/PaymentTransaction/PayPal
```

---

## Credit/Returns (זיכויים)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/ClientCrediting/{id} | Get credit by ID |
| POST | /api/ClientCrediting | Create credit |

---

## Promotions & Discounts (מבצעים והנחות)

### 5 Discount Methods:

1. **Line-level discount** - `Discount` field on each product line
2. **Document discount** - `DiscountPercentage` in ProductsInfo
3. **SpecialOffer** - Reference existing promotion by UniqueId
4. **Manual price** - Set `UnitValue` directly
5. **Rounding** - `Rounding` field for small adjustments

### SpecialOffer Example:
```json
"ProductsInfo": {
  "SpecialOffer": [
    {
      "UniqueId": 15,
      "MappedProducts": [
        { "UniqueId": 100, "Amount": 2 },
        { "UniqueId": 101, "Amount": 1 }
      ]
    }
  ]
}
```

**Note:** SpecialOffer supported in Orders, Invoices, InvoiceReceipts. NOT in Receipts or Supplier documents.

---

## Tasks/CRM (משימות)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Task/{id} | Get task by ID |
| GET | /api/Task | Search tasks |
| POST | /api/Task | Create task |
| PUT | /api/Task/{id} | Update task |
| DELETE | /api/Task/{id} | Delete task |

### Related Endpoints:
- `/api/TaskNote` - Task notes
- `/api/TaskAttachment` - Task attachments
- `/api/TaskLog` - Activity log

### Task Object Fields:
- `UniqueId` - Task ID
- `Title` - Task title
- `Description` - Details
- `DueDate` - Due date
- `Status` - Task status
- `AssignedTo` - Assigned employee
- `Client` - Related client
- `Priority` - Priority level

---

## Addresses (כתובות)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Address/{id} | Get address by ID |
| GET | /api/Address | Search addresses |
| POST | /api/Address | Create address |
| PUT | /api/Address/{id} | Update address |

### Address Object Fields:
- `UniqueId` - Address ID
- `Street` - Street name
- `HouseNumber` - House number
- `City` - City
- `ZipCode` - Postal code
- `Country` - Country

---

## Notes (הערות)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/Note/{id} | Get note by ID |
| POST | /api/Note | Create note |
| PUT | /api/Note/{id} | Update note |

---

## SMS (הודעות)

### Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/Sms | Send SMS |
| GET | /api/SmsLog | Get SMS log |

### Send SMS:
```json
POST /api/Sms
{
  "Phone": "050-1234567",
  "Message": "Your order is ready!"
}
```

---

## Sync & Pagination

### TimeStamp Sync:
Use `TimeStamp` parameter to get only changed records:
```
GET /api/Client?TimeStamp=12345678
```
Returns only clients modified after that timestamp.

### Pagination:
```
GET /api/Product?StartPoint=0&MaxResults=100
GET /api/Product?StartPoint=100&MaxResults=100
```

### Response Structure:
```json
{
  "TotalCount": 1500,
  "StartPoint": 0,
  "MaxResults": 100,
  "Items": [ ... ]
}
```

---

## Branch Transfers (העברות בין סניפים)

### Two-step process:

1. **Send from source branch:**
```json
POST /api/BranchDelivery
{
  "SourceBranch": { "UniqueId": 1 },
  "TargetBranch": { "UniqueId": 2 },
  "Products": [ ... ]
}
```

2. **Receive at target branch:**
```json
POST /api/BranchReceiveDelivery
{
  "ChainInfo": { "UniqueId": 12345 }
}
```

Linked via `ChainInfo` object.

---

## Common Questions & Answers

### Q: Can I store multiple phone numbers for a client?
A: Yes! Phone is an ARRAY field - you can store UNLIMITED phone numbers.

### Q: How do product categories work?
A: Products can have up to 5 tags (Category array). Tags are flat - no hierarchy.

### Q: How do product filters/attributes work?
A: Products can have up to 10 filters for variants like color, size, weight. Each filter has Name and Value.

### Q: What document types can I create?
A: ClientInvoice, ClientReceipt, ClientInvoiceReceipt, ClientOrder, ClientCrediting, SupplierInvoice, SupplierCrediting.

### Q: What's the difference between ClientInvoice and SupplierInvoice?
A: ClientInvoice = You sell TO customer. SupplierInvoice = You buy FROM supplier.

### Q: How do I link documents together?
A: Use `ChainInfo` object to link related documents (Order → Delivery → Invoice).

### Q: How do I handle payments?
A: For credit cards, first POST to /api/PaymentTransaction/CreditCard, then use the returned UniqueId in the Receipt.

### Q: How do I apply discounts?
A: 5 methods: line-level Discount, document DiscountPercentage, SpecialOffer reference, manual UnitValue, or Rounding.

### Q: What's EssentialInfo?
A: Required in all movements - contains Company, Branch, Client/Supplier, and Employee.

---

## Quick Reference - What Can I Do?

### ✅ Full CRUD (Create, Read, Update, Delete):
- Clients, Products, Employees, Suppliers, Tasks, Addresses, Notes

### ✅ Create & Read:
- ClientInvoice, ClientReceipt, ClientInvoiceReceipt, ClientOrder, ClientCrediting
- SupplierInvoice, SupplierCrediting
- PaymentTransaction (CreditCard, PayPal)

### ❌ Read Only:
- ClientQuote, ClientDelivery
- Branch, Company
- Inventory, InventoryLog

---

*Boss Software Solutions WebAPI - Ask me anything in Hebrew or English!*
