# 🌐 ORiem Finance API — Endpoints Documentation

This document outlines the core REST API endpoints available for frontend-to-backend communication within ORiem Finance. All routes require proper **authentication** and role-based authorization where applicable.

---

## 🔐 Authentication & Security

- **Base URL**: `https://api.oriem.finance/v1`
- **Auth Method**: Bearer Token (JWT)
- **Headers**:
  ```http
  Authorization: Bearer <token>
  Content-Type: application/json
  ```

---

## 📁 Authentication

| Method | Endpoint              | Description              |
|--------|------------------------|--------------------------|
| POST   | `/auth/signup`         | Register new user        |
| POST   | `/auth/login`          | Authenticate user        |
| POST   | `/auth/logout`         | Invalidate token         |
| POST   | `/auth/forgot-password`| Send password reset link |
| POST   | `/auth/reset-password` | Reset with token         |

---

## 👤 Users

| Method | Endpoint                  | Description                         |
|--------|---------------------------|-------------------------------------|
| GET    | `/users/me`               | Get current user info               |
| PUT    | `/users/me`               | Update profile                      |
| GET    | `/users/{id}`             | Admin: Get user by ID               |
| GET    | `/admin/users`            | Admin: Get all users                |
| PUT    | `/admin/users/{id}/role`  | Admin: Update user role             |
| POST   | `/admin/users/{id}/suspend`| Admin: Suspend user                |
| DELETE | `/admin/users/{id}`       | Admin: Delete user                  |

---

## 🏦 Accounts

| Method | Endpoint              | Description                       |
|--------|------------------------|-----------------------------------|
| GET    | `/accounts`            | Get user accounts                 |
| POST   | `/accounts`            | Create new account                |
| GET    | `/accounts/{id}`       | Get account details               |
| PUT    | `/accounts/{id}`       | Update account info               |
| DELETE | `/accounts/{id}`       | Delete account                    |

---

## 💸 Transactions

| Method | Endpoint                  | Description                         |
|--------|---------------------------|-------------------------------------|
| POST   | `/transactions/deposit`   | Deposit into an account             |
| POST   | `/transactions/withdraw`  | Withdraw from an account            |
| POST   | `/transactions/transfer`  | Transfer between accounts           |
| GET    | `/transactions/history`   | Get user transaction history        |
| GET    | `/admin/transactions`     | Admin: View all transactions        |

---

## 🧾 Loans

| Method | Endpoint                    | Description                          |
|--------|-----------------------------|--------------------------------------|
| POST   | `/loans/apply`              | Apply for a loan                     |
| GET    | `/loans`                    | Get current user's loans             |
| GET    | `/loans/{id}`               | Get loan details                     |
| POST   | `/loans/{id}/repay`         | Make a repayment                     |
| GET    | `/admin/loans`              | Admin: View all loan applications    |
| POST   | `/admin/loans/{id}/approve` | Admin: Approve loan                  |
| POST   | `/admin/loans/{id}/reject`  | Admin: Reject loan                   |

---

## 📅 Scheduled Payments

| Method | Endpoint                   | Description                          |
|--------|----------------------------|--------------------------------------|
| POST   | `/payments/schedule`       | Schedule a bill payment              |
| GET    | `/payments/scheduled`      | View upcoming scheduled payments     |
| DELETE | `/payments/{id}`           | Cancel scheduled payment             |

---

## 🧮 Reports & Analytics (Admin)

| Method | Endpoint                     | Description                         |
|--------|------------------------------|-------------------------------------|
| GET    | `/admin/reports/summary`     | System summary (accounts, loans)    |
| GET    | `/admin/reports/transactions`| Transaction charts & stats          |
| GET    | `/admin/reports/loan-stats`  | Loan approval/rejection ratios      |

---

## 🔍 Audit Logs (Admin)

| Method | Endpoint           | Description                      |
|--------|--------------------|----------------------------------|
| GET    | `/admin/audit-logs`| View all admin/user activities   |

---

## 📬 Notifications & Messages

| Method | Endpoint                    | Description                        |
|--------|-----------------------------|------------------------------------|
| GET    | `/notifications`            | Get user/system notifications      |
| POST   | `/admin/messages/send`      | Admin: Send message to a user      |
| GET    | `/admin/messages/inbox`     | Admin: View message inbox          |

---

## ⚙️ System & Metadata

| Method | Endpoint               | Description                        |
|--------|------------------------|------------------------------------|
| GET    | `/meta/banks`          | List of supported banks            |
| GET    | `/meta/account-types`  | Account types (savings, current)   |
| GET    | `/meta/loan-types`     | Loan types supported               |

---

## ✅ Status Codes

| Code | Meaning                    |
|------|----------------------------|
| 200  | OK                         |
| 201  | Created                    |
| 400  | Bad Request                |
| 401  | Unauthorized               |
| 403  | Forbidden                  |
| 404  | Not Found                  |
| 500  | Internal Server Error      |

---

## 🧪 Testing Tips

- Use Postman or Insomnia for testing endpoints.
- Always send `Authorization` header for protected routes.
- Validate request body with proper field types.

---

## 📌 Future Enhancements

- WebSocket endpoints for real-time notifications
- GraphQL support (optional)
- Bulk operations: CSV import/export for admin
