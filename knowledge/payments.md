# 💳 Payments


### How do payments work in Boss API?

External payments (Credit Card, PayPal) must be created BEFORE creating a Receipt or Invoice Receipt.

**Flow:**
1. POST /api/PaymentTransaction/CreditCard or PayPal
2. Receive UniqueId in response
3. Use UniqueId in Receipt's PaymentsInfo.Payment.CreditUniqueId

**Payment Types in Receipt:**
• Cash
• Check
• CreditCard (requires CreditUniqueId)
• BankTransfer

---

### How do I create a credit card transaction?

Create credit card transaction before creating receipt:

POST /api/PaymentTransaction/CreditCard

**Required fields:**
• DealValue - Transaction amount
• CreditCardNumber - Card number (last 4 digits for display)
• ApprovalNumber - From payment processor

**Optional fields:**
• MerchantType - Payment processor type
• DealType - Shva deal type
• CreditType - Regular, Installments, etc.
• NumOfPayments - For installments
• FirstPaymentValue, EachPaymentValue - Installment amounts
• ExpirationMonth, ExpirationYear
• CvvNumber, OwnerId
• ActorType, ActorUniqueId - Link to customer

**Endpoint:** `POST /api/PaymentTransaction/CreditCard`

**Example:**
```json
POST /api/PaymentTransaction/CreditCard
{
  "DealValue": 500.00,
  "CreditCardNumber": "****1234",
  "ApprovalNumber": "0012345",
  "MerchantType": 1,
  "DealType": 1,
  "CreditType": 1,
  "NumOfPayments": 1,
  "ActorType": "Client",
  "ActorUniqueId": 42
}

Response: { "UniqueId": 31723 }
```

---

### What is the credit card object structure?

Credit Card Transaction structure:

**Endpoint:** `POST /api/PaymentTransaction/CreditCard`

**Object Structure:**
```
CreditCardTransaction:
  UniqueId (long) - Returned after creation
  CreditCardNumber (string) - Card number
  MerchantType (enum) - Payment processor
  ApprovalNumber (string) - Approval code
  PreSavedIndex (long?) - Pre-saved card index
  ActorType (enum) - Client, Supplier, etc.
  ActorUniqueId (long?) - Actor's UniqueId
  DealType (enum) - Shva deal type
  CreditType (enum) - Regular, Installments, Credit
  ExpirationMonth (long?)
  ExpirationYear (long?)
  IssueCreditCompanyCode (enum) - Issuing company
  SettledCompanyCode (enum) - Settlement company
  BrandCode (enum) - Visa, Mastercard, etc.
  OwnerId (string) - ID number
  CvvNumber (string)
  CurrencyType (enum) - Currency
  StarDiscountValue (decimal?) - Star discount
  DealValue (decimal?) - Transaction amount
  NumOfPayments (long?) - Number of installments
  FirstPaymentValue (decimal?)
  EachPaymentValue (decimal?)
```

---

### How do I create a PayPal transaction?

Create PayPal transaction before creating receipt:

POST /api/PaymentTransaction/PayPal

**Fields:**
• TransactionId - PayPal transaction ID (from PayPal)
• Total - Transaction amount
• ActorType, ActorUniqueId - Link to customer
• CreationDateTime - Transaction time

**Endpoint:** `POST /api/PaymentTransaction/PayPal`

**Example:**
```json
POST /api/PaymentTransaction/PayPal
{
  "TransactionId": "5TY12345AB678901C",
  "Total": 250.00,
  "ActorType": "Client",
  "ActorUniqueId": 42,
  "CreationDateTime": "2025-01-16T10:30:00Z"
}

Response: { "UniqueId": 5678 }
```

---

### How do I use payment in a receipt?

After creating payment transaction, use the UniqueId in Receipt:

**PaymentsInfo.Payment fields:**
• Type - "Cash", "Check", "CreditCard", "BankTransfer"
• Value - Payment amount
• CreditUniqueId - Required for CreditCard type

**Endpoint:** `POST /api/ClientReceipt`

**Example:**
```json
// First: Create credit card transaction
POST /api/PaymentTransaction/CreditCard
{ "DealValue": 500.00, ... }
// Response: { "UniqueId": 31723 }

// Then: Create receipt with payment
POST /api/ClientReceipt
{
  "EssentialInfo": {
    "Company": {"UniqueId": 1},
    "Branch": {"UniqueId": 1},
    "Client": {"UniqueId": 42},
    "Employee": {"UniqueId": 1}
  },
  "PaymentsInfo": {
    "Total": 500.00,
    "Payment": [
      {
        "Type": "CreditCard",
        "Value": 500.00,
        "CreditUniqueId": 31723
      }
    ]
  }
}
```

---
