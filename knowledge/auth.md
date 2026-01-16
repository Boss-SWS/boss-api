# 🔑 Authentication


### Tell me about Auth

**Authentication**

Boss API uses Basic Authentication with username and password.

**How to authenticate:**
• Include Authorization header in every request
• Format: `Basic base64(username:password)`

**Example:**
```
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

**Getting credentials:**
• Contact Boss support to get API credentials
• Each user has specific permissions

**Note:** All endpoints require authentication (marked with [Autorize] attribute).

---

### How do I authenticate with the API?

Use HTTP Basic Authentication:

1. Combine username and password: `username:password`
2. Encode in Base64
3. Add header: `Authorization: Basic {encoded}`

Include this header in every API request.

**Example:**
```json
// JavaScript example
const credentials = btoa('myuser:mypassword');

fetch('https://api.boss-sws.com/api/Client/1', {
  headers: {
    'Authorization': `Basic ${credentials}`
  }
})
```

---

### Why am I getting 401 Unauthorized?

**401 Unauthorized** means authentication failed.

**Common causes:**
• Wrong username or password
• Missing Authorization header
• Incorrect Base64 encoding
• User account disabled
• Missing permissions for this endpoint

**Check:**
1. Verify credentials are correct
2. Ensure header format is `Basic {base64}`
3. Check user has required permissions

---
