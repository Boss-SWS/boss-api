# 💬 SMS


### How do I send SMS?

Send SMS: POST /api/SMS. Parameters: Reason (Config/Manual/Advertise/Delivering), Message (content), EmployeeUniqueId, HeadLine (internal tag), Recipients (Actor or PhoneNumber).

**Endpoint:** `POST /api/SMS`

**Example:**
```json
POST /api/SMS
{
  "Reason": "Advertise",
  "Message": "Your message here",
  "EmployeeUniqueId": 7,
  "HeadLine": "Internal tag",
  "Recipients": {
    "Actor": [{"ActorType": "Client", "ActorUniqueId": 666}],
    "PhoneNumber": ["052-5555555"]
  }
}
```

---

### What are the different SMS Reason types?

Reason types: Config - automatic system messages, Manual - one-time manual send, Advertise - marketing and promotions, Delivering - delivery notifications.

**Endpoint:** `POST /api/SMS`

---

### How do I send verification SMS?

Verification SMS: POST /api/SMS/Authentication. Sends verification code to customer. Parameters: PhoneNumber, Message (with code), EmployeeUniqueId, HeadLine.

**Endpoint:** `POST /api/SMS/Authentication`

**Example:**
```json
POST /api/SMS/Authentication
{
  "PhoneNumber": "0523339912",
  "Message": "Your verification code is ...",
  "EmployeeUniqueId": 6,
  "HeadLine": "API Verification"
}
```

---

### How do I send SMS to multiple recipients?

Multiple recipients: Add multiple objects to Recipients.Actor array or multiple phones to Recipients.PhoneNumber array. Can combine both Actor and PhoneNumber - sends to all.

**Endpoint:** `POST /api/SMS`

---
