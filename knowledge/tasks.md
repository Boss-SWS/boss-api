# ✅ Tasks


### How do I create a new task?

Create task: POST /api/Task. Fields: Type, Status, HeadLine, CreationDateTime, CreatorEmployee, ReminderDateTime, Priority (Low/Medium/High), AssignedEmployee, AssosiatedActors.

**Endpoint:** `POST /api/Task`

**Example:**
```json
POST /api/Task
{
  "Type": "Managment",
  "Status": "Active",
  "HeadLine": "Sales reminder - new client",
  "CreatorEmployee": {"UniqueId": 1},
  "ReminderDateTime": "2018-03-20T14:43:00",
  "Priority": "High",
  "AssosiatedActors": [{"Type": "Client", "UniqueId": 123}]
}
```

---

### How do I get a task by ID?

Get task: GET /api/Task/{UniqueId}.

**Endpoint:** `GET /api/Task/{UniqueId}`

**Example:**
```json
GET /api/Task/3
```

---

### How do I search for tasks?

Search tasks: GET /api/Task. Supports standard params: Phrase, StartPoint, MaxResults.

**Endpoint:** `GET /api/Task`

**Example:**
```json
GET /api/Task?Phrase=sales&MaxResults=20
```

---

### How do I update a task?

Update task: PUT /api/Task/{UniqueId}. Send fields to update.

**Endpoint:** `PUT /api/Task/{UniqueId}`

**Example:**
```json
PUT /api/Task/3
{
  "Status": "Completed"
}
```

---

### How do I delete a task?

Delete task: DELETE /api/Task/{UniqueId}.

**Endpoint:** `DELETE /api/Task/{UniqueId}`

**Example:**
```json
DELETE /api/Task/3
```

---

### How do I add a note to a task?

Add note: POST /api/TaskNote. Fields: TaskUniqueId, Text, WriterEmployee.

**Endpoint:** `POST /api/TaskNote`

**Example:**
```json
POST /api/TaskNote
{
  "TaskUniqueId": 3,
  "Text": "Called client, no answer"
}
```

---
