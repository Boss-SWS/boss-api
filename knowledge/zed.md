# 📊 Z Reports (End of Day)

> Zed (Z Report) - end of day cash register closing report. Read-only via API.


### What is Zed (Z Report)?

Zed - cash register closing / end of day report. Contains: UniqueId, Status (Opened/Closed), OpenInfo (open datetime), CloseInfo (close datetime), Branch, Company, MoveMent (movement range).

**Endpoint:** `GET /api/Zed/{id}`

---

### How do I get a Z report?

Get Z report: GET /api/Zed/{UniqueId}. Note: Read-only - Z reports are created and closed in the POS system itself.

**Endpoint:** `GET /api/Zed/{id}`

---

### What is Status in Zed?

Status indicates report state: InValid (invalid), Opened (register open), Closed (register closed). A Closed report is a complete Z report with all day's data.

---

### What is MoveMent in Zed?

MoveMent in Zed contains the range of movements included in the report: ActorType (movement type), FirstUniqueId (first movement), LastUniqueId (last movement). Shows which documents were included in the day.

---

### Why can't I create/close Z via API?

Z reports are part of an official register closing process that includes cash counting, reconciliation, and printing. This process is performed in the POS system itself, not via API.

---
