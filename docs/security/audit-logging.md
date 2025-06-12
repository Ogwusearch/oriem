# 🧾 Audit Logging

Audit logging is essential for tracking user actions, detecting anomalies, ensuring accountability, and maintaining regulatory compliance. This document outlines the audit logging approach used in the ORiem Finance platform.

---

## 📌 1. What Is Audit Logging?

Audit logs are records of important system events, such as user logins, data modifications, or role changes. They answer the questions:

- **Who** performed the action?
- **What** was done?
- **When** did it happen?
- **Where** (endpoint or component) did it occur?
- **Why** (optional reason, if provided)?

---

## 🗂️ 2. Audit Log Table Schema

The `audit_logs` table in Supabase stores all critical actions:

| Column        | Type        | Description                            |
|---------------|-------------|----------------------------------------|
| id            | UUID (PK)   | Unique identifier for the log          |
| user_id       | UUID        | ID of the user who performed the action |
| action        | Text        | Description of the action              |
| module        | Text        | Related system module (e.g., "loans")  |
| status        | Text        | Result status (e.g., "success", "fail")|
| ip_address    | Text        | IP address of the requester            |
| user_agent    | Text        | User agent string                      |
| metadata      | JSONB       | Optional extra data                    |
| created_at    | Timestamp   | Time the event occurred                |

---

## 🔐 3. What Gets Logged?

Examples of logged actions:

| Action                        | Module        | Logged? |
|------------------------------|---------------|---------|
| User login                   | auth           | ✅      |
| Password reset               | auth           | ✅      |
| Role changes                 | admin panel    | ✅      |
| Fund transfers               | transactions   | ✅      |
| Loan approval/rejection      | loans          | ✅      |
| Failed access attempt        | security       | ✅      |
| API key usage                | integrations   | ✅      |

---

## ⚙️ 4. How Logging Works

1. **Backend (FastAPI):**
   - Middleware captures each request and logs critical events.
   - Specific controller functions also trigger audit log entries on sensitive operations.

2. **Frontend:**
   - Only sends optional metadata (`action reason`, `context`) during requests.
   - Never responsible for writing logs directly.

3. **Database (Supabase):**
   - Logs are written to the `audit_logs` table using the backend service layer.

---

## 🛑 5. Sensitive Data Handling

- Never log passwords, full credit card numbers, or sensitive PII.
- Mask account numbers and tokens if needed.
- Use `metadata` field for context (e.g., transaction_id, loan_id), but sanitize values.

---

## 📈 6. Querying Audit Logs

Admins can view and filter logs via:

- The **Admin Dashboard** → `Audit Logs` page
- SQL console using queries like:

```sql
SELECT * FROM audit_logs
WHERE user_id = 'uuid'
ORDER BY created_at DESC
LIMIT 100;
```

---

## 🧪 7. Example Entry

```json
{
  "user_id": "6a91-xyz...",
  "action": "Approved loan request",
  "module": "loans",
  "status": "success",
  "ip_address": "192.168.1.101",
  "user_agent": "Mozilla/5.0",
  "metadata": {
    "loan_id": "loan_2038",
    "amount": 10000
  },
  "created_at": "2025-06-11T12:45:00Z"
}
```

---

## ✅ 8. Best Practices

- Log every action with minimal performance impact.
- Use batch inserts or background workers if needed.
- Review logs regularly for anomalies or abuse.
- Encrypt logs at rest and restrict access to logs.

---

_Last updated: June 11, 2025_
