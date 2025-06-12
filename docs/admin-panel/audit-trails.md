# Audit Trails

Audit trails are essential for maintaining accountability and tracking actions across the banking system. They help monitor critical changes, user activities, and provide an immutable log for investigation or compliance purposes.

---

## 📌 Purpose

- Record key events and actions taken by users or the system
- Help detect unauthorized or suspicious behavior
- Ensure regulatory and security compliance
- Provide traceability and transparency for admin reviews

---

## 🧱 Data Model: `audit_logs`

| Field           | Type        | Description                              |
|----------------|-------------|------------------------------------------|
| id             | UUID        | Unique identifier                        |
| user_id        | UUID        | Reference to the user who performed the action |
| action         | Text        | Description of the action (e.g., "LOGIN", "TRANSFER_FUNDS") |
| entity_type    | Text        | Type of resource affected (e.g., "account", "loan") |
| entity_id      | UUID/Text   | ID of the affected resource              |
| metadata       | JSONB       | Additional info (IP, browser, changes)   |
| timestamp      | Timestamp   | Time of the action                       |

---

## ⚙️ How Audit Logs Are Captured

### 1. **Backend Service Layer**
Each sensitive endpoint (e.g., login, account updates, transfers) should call an audit service:

```python
# Python (FastAPI) Example
from services.audit import log_action

@app.post("/transfer")
async def transfer_funds(request: TransferRequest, user=Depends(get_current_user)):
    # ... perform transfer logic ...
    await log_action(
        user_id=user.id,
        action="TRANSFER_FUNDS",
        entity_type="transaction",
        entity_id=txn.id,
        metadata={"amount": request.amount, "recipient": request.recipient_id}
    )
```

---

### 2. **Middleware (Optional)**
Track login attempts, unauthorized access, etc.

---

### 3. **Database Triggers**
For tracking direct table changes (e.g., from SQL scripts), you can use Supabase/PostgreSQL triggers:

```sql
CREATE OR REPLACE FUNCTION log_user_update()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO audit_logs (user_id, action, entity_type, entity_id, metadata, timestamp)
  VALUES (
    NEW.id,
    'USER_UPDATE',
    'user',
    NEW.id,
    jsonb_build_object('changes', hstore_to_jsonb_loose(hstore(OLD.*) - hstore(NEW.*))),
    now()
  );
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

---

## 🔐 Access Control

- Only admins with the `view_audit_logs` permission can access the audit logs.
- Logs are read-only to all users.

---

## 🖥️ Admin UI Example

| Date & Time        | User        | Action          | Resource         | Metadata |
|--------------------|-------------|------------------|------------------|----------|
| 2025-06-10 13:22   | John Doe    | LOGIN            | -                | IP: 192.168.1.1 |
| 2025-06-10 13:25   | John Doe    | TRANSFER_FUNDS   | txn_12ab34       | Amount: $500 |
| 2025-06-10 13:30   | Admin Sarah | USER_UPDATE      | user_098f        | Email changed |

---

## ♻️ Retention & Monitoring

- Audit logs are stored for **5 years**.
- Weekly backups are automated.
- Alerts for suspicious actions (e.g., >5 failed logins) are handled via the security module.

---

## ✅ Best Practices

- Never store passwords or sensitive payloads in metadata.
- Always validate user context and sanitize inputs before logging.
- Limit audit table access to backend services only.

