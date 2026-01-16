# 📊 TableHandle


### What is TableHandle?

TableHandle manages arrays (phones, categories, prices). Parameters: Index (position, starts from 1!), Operation (NewFirstEmpty/New/Update/Delete). NewFirstEmpty doesn't need Index - system picks first empty slot.

**Example:**
```json
NewFirstEmpty:
{"TableHandle": {"Operation": "NewFirstEmpty"}}

New at index 1:
{"TableHandle": {"Index": "1", "Operation": "New"}}

Update index 2:
{"TableHandle": {"Index": "2", "Operation": "Update"}}

Delete index 3:
{"TableHandle": {"Index": "3", "Operation": "Delete"}}
```

---

### How do I add a new phone to a client?

Add phone: Use TableHandle with Operation: NewFirstEmpty (first empty slot) or New with specific Index. Client can have up to 4 phones.

**Example:**
```json
{
  "Phone": [{
    "Number": "052-1234567",
    "Description": "Mobile",
    "TableHandle": {"Operation": "NewFirstEmpty"}
  }]
}
```

---

### How do I update an existing phone?

Update phone: Use TableHandle with Index (phone position, starts from 1) and Operation: Update.

**Example:**
```json
Update phone at position 1:
{
  "Phone": [{
    "Number": "053-9876543",
    "Description": "New Office",
    "TableHandle": {"Index": "1", "Operation": "Update"}
  }]
}
```

---

### How do I delete a phone?

Delete phone: Use TableHandle with Index (phone position) and Operation: Delete.

**Example:**
```json
Delete phone at position 2:
{
  "Phone": [{
    "TableHandle": {"Index": "2", "Operation": "Delete"}
  }]
}
```

---

### How do I add categories to a client or product?

Add category: Use TableHandle with Operation: NewFirstEmpty or New with Index. Client and product can have up to 5 categories.

**Example:**
```json
{
  "Category": [
    {"UniqueId": "1", "TableHandle": {"Operation": "NewFirstEmpty"}},
    {"UniqueId": "2", "TableHandle": {"Index": "2", "Operation": "New"}}
  ]
}
```

---

### Why does Index start from 1, not 0?

In TableHandle, Index starts from 1 (not 0 like regular arrays). So Phone[1] is the first phone, Phone[2] is the second, etc.

---

### How do I perform multiple phone operations in one request?

You can combine multiple TableHandle operations in the same request - add, update and delete together.

**Example:**
```json
{
  "Phone": [
    {"Number": "077-1111111", "TableHandle": {"Operation": "NewFirstEmpty"}},
    {"Number": "052-2222222", "TableHandle": {"Index": "2", "Operation": "Update"}},
    {"TableHandle": {"Index": "4", "Operation": "Delete"}}
  ]
}
```

---
