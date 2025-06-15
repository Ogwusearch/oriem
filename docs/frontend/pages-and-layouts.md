# 🧭 ORiem Finance – Pages & Layouts Guide

This guide documents the major frontend pages and layout structure for the ORiem Finance banking web application. It covers both **Customer-facing** and **Admin** sections with routing and layout hierarchy.

---

## 🌐 Pages Overview

### 🔐 Authentication Pages

| Page                | Route              | Description                           |
|---------------------|--------------------|---------------------------------------|
| Login               | `/login`           | Customer/Admin login form             |
| Signup              | `/signup`          | Customer registration                 |
| Forgot Password     | `/forgot-password` | Reset password via email              |
| OTP Verification    | `/verify-otp`      | Verify one-time code after signup     |

---

### 👤 Customer Dashboard Pages

| Page                  | Route                    | Description                                 |
|------------------------|--------------------------|---------------------------------------------|
| Home/Dashboard         | `/dashboard`             | Overview of accounts, transactions, loans   |
| Account Details        | `/account/:id`           | View specific account info                  |
| Fund Transfer          | `/transfer`              | Send money to another account               |
| Bill Payments          | `/pay-bills`             | Pay utility bills, add payees               |
| Loan Application       | `/loans/apply`           | Request a new loan                          |
| Transaction History    | `/transactions`          | Filterable list of past transactions        |
| Notifications          | `/notifications`         | Alerts: payments, approvals, messages       |
| Profile & Settings     | `/profile`               | Edit profile, update password               |
| Support & Contact      | `/support`               | Contact form, FAQ, support messages         |
| Terms & Privacy        | `/terms`                 | Legal and privacy policies                  |

---

### 🛠️ Admin Dashboard Pages

| Page                    | Route                       | Description                                |
|-------------------------|-----------------------------|--------------------------------------------|
| Admin Home              | `/admin/dashboard`          | Metrics and system summary                 |
| Customer Management     | `/admin/users`              | View, block, or manage customer accounts   |
| Loan Management         | `/admin/loans`              | Approve or reject loan requests            |
| Transactions Overview   | `/admin/transactions`       | Full record of all system transactions     |
| Audit Logs              | `/admin/audit-logs`         | Activity logs of customers/admin actions   |
| Role & Access Control   | `/admin/roles`              | RBAC permissions for different user types  |

---

## 🧱 Layout Architecture

### 🧭 Main Layouts

```jsx
src/layouts/
├── AuthLayout.jsx       // Login/Signup pages layout (no nav)
├── CustomerLayout.jsx   // Customer dashboard layout (sidebar, topbar)
├── AdminLayout.jsx      // Admin panel layout (sidebar + breadcrumbs)
├── PageWrapper.jsx      // Shared wrapper (auth checks, padding)
└── ProtectedRoute.jsx   // Guards private routes (auth required)
```

---

### 📦 Pages Folder Structure

```bash
src/pages/
├── auth/
│   ├── login.jsx
│   ├── signup.jsx
│   └── forgot-password.jsx
├── dashboard/
│   ├── index.jsx
│   ├── account/[id].jsx
│   ├── transfer.jsx
│   ├── pay-bills.jsx
│   └── transactions.jsx
├── loans/
│   ├── apply.jsx
│   └── details/[loanId].jsx
├── profile/
│   └── index.jsx
├── admin/
│   ├── dashboard.jsx
│   ├── users.jsx
│   ├── loans.jsx
│   ├── transactions.jsx
│   ├── audit-logs.jsx
│   └── roles.jsx
└── support/
    ├── contact.jsx
    └── faq.jsx
```

---

## 🔒 Route Protection

| Layout          | Access Condition                   |
|-----------------|------------------------------------|
| AuthLayout      | Public (login/signup)              |
| CustomerLayout  | Authenticated customers only       |
| AdminLayout     | Authenticated admins only          |
| ProtectedRoute  | Wrapped around all private pages   |

---

## 🧑‍🎨 Design Guidelines

- Use `PageWrapper` for padding and setting titles
- All pages support mobile and tablet screens
- Navigation and breadcrumbs are dynamic
- Reuse `Card`, `Table`, `Tabs`, and `ChartWidget` where applicable

---

## 🧭 Navigation Flow

- After login:  
  - If **customer** → redirect to `/dashboard`  
  - If **admin** → redirect to `/admin/dashboard`
- Use `useAuth()` context to switch layout and menus dynamically

---

## 📚 Related Docs

- [`components.md`](./components.md)
- [`routes.md`](../backend/routes.md)
- [`role-based-access.md`](../auth/role-based-access.md)
- [`system-overview.md`](../architecture/system-overview.md)
