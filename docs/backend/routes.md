```markdown
# 🧭 ORiem Finance – Backend Route Structure

This document outlines the structure, organization, and purpose of all backend routes used in the **FastAPI application** for ORiem Finance.

---

## 🌐 API Prefix

All routes are prefixed with `/api/v1`

Example:
```
/api/v1/auth/login
/api/v1/users/me
```

---

## 📦 Route Modules

| Module         | Path Prefix         | Description                             |
|----------------|---------------------|-----------------------------------------|
| Auth           | `/auth`             | Handles login, signup, JWT, sessions    |
| Users          | `/users`            | User profile management                 |
| Accounts       | `/accounts`         | Account creation and info               |
| Transactions   | `/transactions`     | Deposits, withdrawals, transfers        |
| Loans          | `/loans`            | Loan requests, approvals, repayment     |
| Admin          | `/admin`            | Admin-only actions (user, loan control) |
| Audit Logs     | `/audit`            | View system audit trails                |
| Notifications  | `/notifications`    | Mailbox & system notifications          |
| Reports        | `/reports`          | Analytics, charts, financial reports    |

---

## 📂 routes/ Directory Layout

```bash
backend/
└── app/
    └── routes/
        ├── auth.py
        ├── users.py
        ├── accounts.py
        ├── transactions.py
        ├── loans.py
        ├── admin.py
        ├── audit.py
        ├── notifications.py
        └── reports.py
```

Each route module uses:
```python
from fastapi import APIRouter

router = APIRouter(prefix="/module", tags=["ModuleName"])
```

---

## 🛠 Example Router Definition

```python
# routes/loans.py

from fastapi import APIRouter, Depends
from app.controllers.loan_controller import apply_loan, get_loans
from app.auth.dependencies import get_current_user

router = APIRouter(prefix="/loans", tags=["Loans"])

@router.post("/apply")
def apply_for_loan(loan_data: LoanCreate, user=Depends(get_current_user)):
    return apply_loan(loan_data, user)

@router.get("/")
def list_loans(user=Depends(get_current_user)):
    return get_loans(user)
```

---

## 🧩 Router Registration in Main App

```python
# main.py

from fastapi import FastAPI
from app.routes import auth, users, accounts, transactions, loans, admin, audit, notifications, reports

app = FastAPI(title="ORiem Finance API")

app.include_router(auth.router, prefix="/api/v1")
app.include_router(users.router, prefix="/api/v1")
app.include_router(accounts.router, prefix="/api/v1")
app.include_router(transactions.router, prefix="/api/v1")
app.include_router(loans.router, prefix="/api/v1")
app.include_router(admin.router, prefix="/api/v1")
app.include_router(audit.router, prefix="/api/v1")
app.include_router(notifications.router, prefix="/api/v1")
app.include_router(reports.router, prefix="/api/v1")
```

---

## 🧪 Route Testing

Use **Postman** or **pytest** with HTTPX for automated testing.

Example test:

```python
def test_login():
    response = client.post("/api/v1/auth/login", json={"email": "test@example.com", "password": "123456"})
    assert response.status_code == 200
    assert "access_token" in response.json()
```

---

## 📁 Related Docs

- [`auth-flow.md`](auth-flow.md)
- [`endpoints.md`](../api/endpoints.md)
- [`jwt-auth.md`](../auth/jwt-auth.md)
```
