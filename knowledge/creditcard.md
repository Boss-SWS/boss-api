# 💳 Credit Card


### How do I report an external credit card payment?

External credit report: POST /api/PaymentTransaction/CreditCard. Used when payment done on ecommerce site and needs to be reported to VS. Includes card details, credit company, deal type and amount.

**Endpoint:** `POST /api/PaymentTransaction/CreditCard`

---

### What are the required fields for credit card report?

Required: CreditCardNumber (last 4 digits), MerchantType (External), ActorType (Client), ActorUniqueId, DealType (Charge/Credit), CreditType, ExpirationMonth, ExpirationYear, IssueCreditCompanyCode, SettledCompanyCode, BrandCode, CurrencyType, DealValue.

**Endpoint:** `POST /api/PaymentTransaction/CreditCard`

---

### What are the CreditType options?

CreditType options: RegularCredit, AdifPlus30, Immediate, ClubCredit, SuperCredit, Credit, Payments, ClubPayments.

**Endpoint:** `POST /api/PaymentTransaction/CreditCard`

---

### What are the credit company codes?

IssueCreditCompanyCode / SettledCompanyCode: Tourist, Isracard, VisaCal, Tranzila, MAX, Cardcom, Finitione. BrandCode: PrivateCard, MasterCard, Visa, Mastro, Diners, Amex, Isracard, JCB, discover.

**Endpoint:** `POST /api/PaymentTransaction/CreditCard`

---

### How do I report installment payments?

For installment transactions add: NumOfPayments (number of payments), FirstPaymentValue (first payment amount), EachPaymentValue (each subsequent payment amount).

**Endpoint:** `POST /api/PaymentTransaction/CreditCard`

---
