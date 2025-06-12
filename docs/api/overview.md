# 🧪 ORiem Finance API & Admin Panel Testing Guide

This document outlines how to test key functionalities of the ORiem Finance platform, including backend API routes, authentication flows, transactions, loans, and admin panel operations.

---

## 🧰 Tools Required

| Tool         | Purpose                                  |
|--------------|------------------------------------------|
| **Postman**  | API endpoint testing                     |
| **Insomnia** | Alternative API testing tool             |
| **Swagger UI** | Auto-generated API testing from FastAPI |
| **cURL**     | Command-line HTTP requests               |
| **Browser DevTools** | Frontend network request tracking |
| **pytest** (optional) | Python unit/integration testing   |

---

## ✅ Authentication Flow Testing

### 🔐 Login
**Endpoint**: `POST /auth/login`  
**Payload**:
```json
{
  "email": "admin@oriem.finance",
  "password": "securepassword123"
}
```

**Expected Response**:
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...",
  "token_type": "bearer"
}
```

---

## 👤 User Management

### 🔍 Fetch All Users
**Endpoint**: `GET /admin/users`  
**Header**:
```
Authorization: Bearer <token>
```

**Expected**: List of user objects with pagination

---

## 🏦 Accounts

### 📄 Create Account
**Endpoint**: `POST /accounts/create`  
**Payload**:
```json
{
  "user_id": "uuid-123",
  "account_type": "savings"
}
```

**Expected**: Account object with generated account number

---

## 💸 Transactions

### 💵 Deposit Funds
**Endpoint**: `POST /transactions/deposit`  
**Payload**:
```json
{
  "account_id": "acc_001",
  "amount": 5000
}
```

### 🔁 Transfer Funds
**Endpoint**: `POST /transactions/transfer`  
**Payload**:
```json
{
  "from_account": "acc_001",
  "to_account": "acc_002",
  "amount": 1000
}
```

---

## 📝 Loans

### 📨 Submit Loan Application
**Endpoint**: `POST /loans/apply`  
**Payload**:
```json
{
  "user_id": "uuid-123",
  "amount": 10000,
  "duration_months": 12
}
```

### 🧾 Approve Loan (Admin)
**Endpoint**: `PATCH /admin/loans/approve/:loan_id`  
**Header**:
```
Authorization: Bearer <admin_token>
```

---

## 📋 Audit Logs

### 🔎 View Audit Logs
**Endpoint**: `GET /admin/audit-logs`  
**Expected**: Logs of user/admin actions like login, update, approval, etc.

---

## 📊 Reports

### 📈 Daily Transactions Summary
**Endpoint**: `GET /admin/reports/transactions/daily`  
**Query**: `?date=2025-06-10`

---

## 🔐 JWT Token Expiry & Refresh Testing

1. Log in and copy token
2. Wait for token expiry (e.g., 15 mins)
3. Try using expired token
4. Confirm system returns 401 Unauthorized
5. Test refresh flow (if implemented)

---

## 🧪 Frontend Testing (Browser)

- Open DevTools → Network tab
- Trigger key UI actions:
  - Login
  - Apply for loan
  - Submit transfer
- Verify correct API calls, payloads, and responses
- Check for toast notifications, success states

---

## 🧼 Cleanup for Testing

- Use test accounts (e.g., `testuser1@oriem.finance`)
- Reset DB with mock data (optional seed script)
- Use `DELETE` endpoints (if available) to clean test artifacts

---

## 🧪 Optional: Unit/Integration Tests (pytest)

Write backend tests using `pytest`:
```bash
pytest tests/test_loans.py
```

Structure:
```
tests/
  ├── test_auth.py
  ├── test_accounts.py
  ├── test_transactions.py
  └── test_loans.py
```

---

## 🧭 Related Docs

- [API Overview](./overview.md)
- [Endpoint Reference](./endpoints.md)
- [Loan Approvals](../admin-panel/loan-approvals.md)
- [Audit Logging](../admin-panel/audit-logs.md)

---

> For testing issues, contact QA team: qa@oriem.finance
