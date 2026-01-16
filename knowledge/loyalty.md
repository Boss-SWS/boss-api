# ⭐ Loyalty & Analytics

> RewardPoints for loyalty program, Prognosis for customer analytics/predictions


### What is RewardPoints?

RewardPoints - loyalty/accumulation points system. Each client has a RewardPoints field containing: Allowed (if enabled), Accumulate (accumulated points).

**Endpoint:** `GET /api/RewardPoints`

---

### How do I get a client's reward points?

Get points: GET /api/RewardPoints?ActorType=Client&UniqueId={clientId}. Returns the client's points status.

**Endpoint:** `GET /api/RewardPoints`

---

### How do I update a client's reward points?

Update points: PUT /api/RewardPoints?ActorType=Client&UniqueId={clientId}&Value={points}. Value is the new points amount.

**Endpoint:** `PUT /api/RewardPoints`

---

### What is Prognosis?

Prognosis - customer behavior analysis and prediction. Contains Score indicating customer's activity/loyalty level. Used to identify customers at risk of churn.

**Endpoint:** `GET /api/Prognosis`

---

### How do I get Prognosis?

Get Prognosis: GET /api/Prognosis. Parameters: TranId, StartPoint, MaxResults. Prognosis is also included in the Client object itself.

**Endpoint:** `GET /api/Prognosis`

---

### How do points appear in Client?

Inside the Client object there's a RewardPoints field containing points status: Allowed (enabled/not), Accumulate (accumulated points amount).

---
