```markdown
# ⚙️ ORiem Finance – Backend Services Architecture

This document outlines the structure and responsibilities of all backend service modules in the **FastAPI-based** ORiem Finance backend.

Services are responsible for **business logic** and database interactions. They are invoked from route or controller layers and abstract away direct DB access.

---

## 📁 Directory Layout

```bash
backend/
└── app/
    └── services/
        ├── auth_service.py
        ├── user_service.py
        ├── account_service.py
        ├── transaction_service.py
        ├── loan_service.py
        ├── admin_service.py
        ├── audit_service.py
        ├── notification_service.py
        └── report_service.py
```

---

## 🧠 Service Responsibilities

| Service File          | Purpose                                               |
|-----------------------|-------------------------------------------------------|
| `auth_service.py`     | Login, signup, JWT, session, password management      |
| `user_service.py`     | Create/update profile, fetch user data                |
| `account_service.py`  | Open accounts, update balances, get account info      |
| `transaction_service.py` | Process deposits, withdrawals, transfers          |
| `loan_service.py`     | Apply, approve, reject, and track loan repayments     |
| `admin_service.py`    | Admin dashboard logic: users, loans, transactions     |
| `audit_service.py`    | Log and retrieve audit trails                         |
| `notification_service.py` | Inbox, system messages, alerts                  |
| `report_service.py`   | Analytics and financial report generation             |

---

## 🔧 Example: `loan_service.py`

```python
from app.models import Loan, LoanRepayment
from app.schemas import LoanCreate
from app.database import db_session

def apply_for_loan(data: LoanCreate, user_id: str):
    loan = Loan(**data.dict(), user_id=user_id, status="pending")
    db_session.add(loan)
    db_session.commit()
    return loan

def approve_loan(loan_id: str, admin_id: str):
    loan = db_session.query(Loan).filter(Loan.id == loan_id).first()
    if loan:
        loan.status = "approved"
        loan.approved_by = admin_id
        db_session.commit()
        return loan
    raise Exception("Loan not found")
```

---

## 🧪 Service Unit Testing

Service functions should be independently testable without requiring FastAPI routing.

Example test:

```python
def test_apply_for_loan():
    loan_data = LoanCreate(amount=10000, term_months=12)
    loan = apply_for_loan(loan_data, user_id="user_123")
    assert loan.status == "pending"
    assert loan.amount == 10000
```

---

## 🧩 Service Design Best Practices

- ✅ Services must be stateless (no global state).
- ✅ Always validate inputs at schema/controller level.
- ✅ Use dependency injection for DB/session/testability.
- ✅ Log audit events inside services (e.g., via `audit_service`).
- ✅ Separate query vs command operations when possible.

---

## 📚 Related Docs

- [`routes.md`](routes.md)
- [`auth-flow.md`](auth-flow.md)
- [`database-schema.md`](../architecture/database-schema.md)
- [`endpoints.md`](../api/endpoints.md)
```
