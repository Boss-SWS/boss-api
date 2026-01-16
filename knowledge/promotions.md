# 🏷️ Promotions & Discounts


### How do discounts and promotions work in Boss?

In Boss, discounts are almost always **loaded onto products** (not cart-level). This means the discount must be associated with specific products in the cart.

**Applies to:**
• Orders (הזמנה)
• Client Invoice (חשבונית)
• Client Invoice Receipt (חשבונית מס קבלה)

**5 Methods to report discounts** (simple → complex):
1. Send final price per product
2. Send cart discount amount (Boss splits it)
3. Send cart discount + promotion ID
4. Send full promotion with MappedProducts
5. Use Boss promotion engine

Ask about each method for details.

---

### Discount method 1: Price in ProductsInfo

**Method 1: Send final price per product**

The simplest approach - send the price after discount directly in ProductsInfo.

**Result:** Boss records it as "manual discount" (הנחה ידנית)

**Use when:** Your system already calculates final prices per product.

**Example:**
```json
{
  "ProductsInfo": [
    {
      "ProductId": 123,
      "Quantity": 2,
      "Price": 45.00  // Price AFTER discount (was 50)
    }
  ]
}
```

---

### Discount method 2: Cart discount without promotion

**Method 2: Send total cart discount**

Send only the discount amount - Boss automatically splits it among all products in ProductsInfo.

**Result:** Boss records as "manual discount" (same as method 1)

**Use when:** You have a cart-level discount and don't need to track promotion names.

**Example:**
```json
{
  "DiscountAmount": 10.00,
  "ProductsInfo": [
    { "ProductId": 123, "Quantity": 1, "Price": 50.00 },
    { "ProductId": 456, "Quantity": 1, "Price": 50.00 }
  ]
}
// Boss splits 10₪ discount between both products
```

---

### Discount method 3: Cart discount with promotion ID

**Method 3: Send discount + promotion ID**

Send discount amount with SpecialOffer containing only the promotion ID (no MappedProducts).

Boss splits the discount among products AND shows the promotion name.

**Result:** Shows promotion name in reports

**Use when:** You want to track which promotions were used.

**Example:**
```json
{
  "DiscountAmount": 10.00,
  "SpecialOffer": {
    "SpecialOfferId": 17,
    "SpecialOfferName": "Summer Sale"
    // NO MappedProducts - Boss splits automatically
  },
  "ProductsInfo": [
    { "ProductId": 123, "Quantity": 1, "Price": 50.00 },
    { "ProductId": 456, "Quantity": 1, "Price": 50.00 }
  ]
}
```

---

### Discount method 4: Full dynamic promotion

**Method 4: Full promotion with MappedProducts**

You specify exactly which products the discount applies to using MappedProducts array.

**Result:** 
• Shows promotion name
• Profitability calculated only for participating products
• Full control over discount allocation

**Use when:** You need precise control over which products are discounted.

**Example:**
```json
{
  "SpecialOffer": {
    "SpecialOfferId": 17,
    "SpecialOfferName": "Buy 2 Get 10% Off",
    "DiscountAmount": 10.00,
    "MappedProducts": [
      {
        "ProductId": 123,
        "Quantity": 2,
        "DiscountAmount": 10.00
      }
    ]
  },
  "ProductsInfo": [
    { "ProductId": 123, "Quantity": 2, "Price": 50.00 },
    { "ProductId": 456, "Quantity": 1, "Price": 30.00 }
  ]
}
// Discount applies ONLY to product 123
```

---

### Discount method 5: Boss promotion engine

**Method 5: Use Boss promotion engine**

Full integration with Boss's built-in promotion system. Boss calculates the discounts based on promotion rules.

**Requires:** Complete understanding of Boss promotion types:
• X+Y promotions
• Percentage discounts
• Buy X get Y free
• Tiered discounts
• etc.

**Use when:** Your site/system has no promotion logic - let Boss handle it.

**Note:** This is the most complex method and requires detailed setup.

---

### What is the SpecialOffer structure?

SpecialOffer object for reporting promotions:

**Example:**
```json
{
  "SpecialOffer": {
    "SpecialOfferId": 17,
    "SpecialOfferName": "Summer Sale",
    "DiscountAmount": 15.00,
    "MappedProducts": [
      {
        "ProductId": 123,
        "Quantity": 1,
        "DiscountAmount": 10.00
      },
      {
        "ProductId": 456,
        "Quantity": 1,
        "DiscountAmount": 5.00
      }
    ]
  }
}
```

**Object Structure:**
```
SpecialOffer:
  SpecialOfferId (int) - Promotion ID in Boss
  SpecialOfferName (string) - Display name
  DiscountAmount (decimal) - Total discount amount
  MappedProducts (array, optional) - Products participating in promotion

MappedProducts item:
  ProductId (int) - Product UniqueId
  Quantity (decimal) - Quantity participating
  DiscountAmount (decimal) - Discount for this product
```

---

### Which discount method should I use?

**Choose based on your needs:**

| Need | Method |
|------|--------|
| Simplest integration | 1 - Price in ProductsInfo |
| Cart-level discount, no tracking | 2 - DiscountAmount only |
| Track promotion names | 3 - SpecialOffer without MappedProducts |
| Precise profitability per product | 4 - Full MappedProducts |
| No promotion logic in your system | 5 - Boss engine |

**Most integrations use method 1, 2, or 3.**

---

### Which movements support SpecialOffer?

SpecialOffer is supported in movements that contain products:

**Supported:**
• Order (הזמנה) ✅
• Client Invoice (חשבונית) ✅
• Client Invoice Receipt (חשבונית מס קבלה) ✅

**NOT supported:**
• Receipt (קבלה) - no products
• Supplier movements - not applicable

---
