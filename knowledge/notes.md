# 📝 Notes

> Notes can be attached to any entity (Client, Product, Supplier, etc.) via NoteUniqueId field


### What is Note in the system?

Note - notes that can be attached to any entity (client, product, supplier, branch, etc.). Any entity with a NoteUniqueId field can have a note.

**Endpoint:** `GET /api/Note/{id}`

---

### How do I create a note?

Create note: POST /api/Note. Specify ActorType (entity type), ActorUniqueId (entity ID), and Text (note content).

**Endpoint:** `POST /api/Note`

**Example:**
```json
POST /api/Note
{
  "ActorType": "Client",
  "ActorUniqueId": 123,
  "Text": "VIP customer - give 10% discount"
}
```

---

### How do I get a note?

Get note: GET /api/Note/{UniqueId}. The UniqueId is in the NoteUniqueId field of the entity.

**Endpoint:** `GET /api/Note/{id}`

---

### How do I update a note?

Update note: PUT /api/Note/{UniqueId}. Send the new Text.

**Endpoint:** `PUT /api/Note/{id}`

---

### How do I delete a note?

Delete note: DELETE /api/Note/{UniqueId}.

**Endpoint:** `DELETE /api/Note/{id}`

---

### Which entities can have notes?

Notes can be attached to any entity with a NoteUniqueId field: Client, Product, Supplier, Branch, Company, Employee, Task, and more.

---
