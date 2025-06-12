```markdown
# 🏗️ ORiem Finance – System Overview

This document provides a high-level overview of the architecture, technology stack, and flow of the **ORiem Finance** web-based banking application.

---

## 🌐 Overview

ORiem Finance is a secure, full-stack banking management system that supports:

- Customer and admin user roles
- Online transactions (deposit, withdraw, transfer)
- Loan management
- Bill payments
- Audit logging
- Real-time analytics

---

## 🧱 Architecture Summary

```
Client (React/Vite)
   ↓ (API requests)
Backend (FastAPI)
   ↓ (ORM & services)
Supabase (PostgreSQL)
```

---

## 🛠️ Tech Stack

| Layer         | Technology             |
|--------------|------------------------|
| Frontend     | React, Vite, Axios     |
| Backend      | FastAPI (Python)       |
| Database     | Supabase (PostgreSQL)  |
| Auth         | JWT / OAuth2           |
| Styling      | CSS Modules / SCSS     |
| Deployment   | Railway / Vercel       |
| Docs         | Markdown (Git-based)   |

---

## 🔐 Authentication Flow

1. User logs in via frontend.
2. Credentials sent to FastAPI backend.
3. Backend validates and returns a JWT token.
4. JWT stored in browser and used for secure API access.

---

## ⚙️ Core Modules

- **Authentication**: Login, signup, forgot password
- **Account Management**: View balances, account info
- **Transactions**: Transfer, deposit, withdraw
- **Loans**: Apply, approve, track repayment
- **Admin Panel**: User management, loan approvals, audit logs
- **Audit Logging**: Every action by a user is logged
- **Reports & Analytics**: Admin dashboards with charts

---

## 🧩 API Flow

1. Frontend sends HTTP requests to `/api/...` endpoints.
2. Backend routes validate request → call services → return data.
3. DB queries are performed via SQLAlchemy (ORM).
4. Responses use Pydantic models for validation and serialization.

---

## 🗃️ Database (Supabase)

- Tables:
  - `users`, `accounts`, `transactions`, `loans`, `repayments`
  - `admins`, `audit_logs`, `roles`, `permissions`
- Enforces role-based access (RBAC)

---

## 🛡️ Security

- HTTPS enforced
- JWT for access tokens
- Rate limiting and input validation
- Role-based access control (RBAC)

---

## 📈 Analytics

- Admin dashboard includes:
  - User growth stats
  - Active loan insights
  - Transaction volume reports
  - Audit trail overview

---

## 🧪 Testing Strategy

- Unit tests for backend services and routes
- Integration tests for API endpoints
- Frontend testing via Playwright or Vitest

---

## 📄 Related Docs

- [Database Schema](./database-schema.md)
- [Folder Structure](./folder-structure.md)
- [API Endpoints](../api/endpoints.md)
```
