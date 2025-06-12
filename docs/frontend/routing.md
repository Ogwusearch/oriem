```markdown
# 🔀 ORiem Finance – Frontend Routing Guide

This guide covers the routing strategy for the ORiem Finance banking application. It includes route structures, dynamic parameters, layout assignments, and protected routes.

---

## 🚦 Routing Strategy

We use **React Router v6+** for route management with layout-based separation:

- **Public Routes** – Accessible without login (e.g., Login, Signup)
- **Protected Routes** – Require authentication (e.g., Dashboard, Profile)
- **Role-based Routes** – Restricted to Admins or Customers

---

## 🗂️ Route Structure Overview

```tsx
src/routes/
├── index.tsx               // Entry point with all route configs
├── PublicRoutes.tsx        // Routes like /login, /signup
├── ProtectedRoutes.tsx     // Authenticated routes
├── AdminRoutes.tsx         // Admin-only pages
├── UserRoutes.tsx          // Customer dashboard
└── NotFound.tsx            // 404 page
```

---

## 🌐 Route Definitions

### 🌱 Public Routes

| Path               | Component           | Layout         |
|--------------------|---------------------|----------------|
| `/`                | LandingPage         | AuthLayout     |
| `/login`           | LoginPage           | AuthLayout     |
| `/signup`          | SignupPage          | AuthLayout     |
| `/forgot-password` | ForgotPasswordPage  | AuthLayout     |
| `/verify-otp`      | OTPVerificationPage | AuthLayout     |

---

### 👤 User Routes (Protected)

| Path                      | Component                 | Layout      |
|---------------------------|---------------------------|-------------|
| `/dashboard`              | DashboardPage             | UserLayout  |
| `/account/:id`            | AccountDetailsPage        | UserLayout  |
| `/transfer`               | FundTransferPage          | UserLayout  |
| `/pay-bills`              | BillPaymentPage           | UserLayout  |
| `/transactions`           | TransactionHistoryPage    | UserLayout  |
| `/loans/apply`            | LoanApplicationPage       | UserLayout  |
| `/notifications`          | NotificationsPage         | UserLayout  |
| `/profile`                | ProfilePage               | UserLayout  |
| `/support`                | SupportPage               | UserLayout  |

---

### 🛠️ Admin Routes (Protected + Role-based)

| Path                          | Component                 | Layout        |
|-------------------------------|---------------------------|----------------|
| `/admin/dashboard`            | AdminDashboardPage        | AdminLayout    |
| `/admin/users`                | UserManagementPage        | AdminLayout    |
| `/admin/loans`                | LoanManagementPage        | AdminLayout    |
| `/admin/transactions`         | TransactionsOverviewPage  | AdminLayout    |
| `/admin/audit-logs`           | AuditLogsPage             | AdminLayout    |
| `/admin/roles`                | RolePermissionPage        | AdminLayout    |

---

## 🔐 Protected Route Wrapper

```tsx
<Route
  path="/dashboard"
  element={
    <ProtectedRoute role="user">
      <UserLayout>
        <DashboardPage />
      </UserLayout>
    </ProtectedRoute>
  }
/>
```

- `ProtectedRoute` checks:
  - If user is authenticated
  - If user has the required role (user/admin)

---

## 🧠 Dynamic Routing

- `/account/:id` – For individual account pages
- `/loans/details/:loanId` – Loan detail page (future scope)
- `/transactions/:txnId` – Transaction receipt page (optional)

Use `useParams()` to retrieve route parameters.

---

## 🧪 404 Fallback Route

```tsx
<Route path="*" element={<NotFound />} />
```

---

## 🧭 Redirects

After login:
- If user role = `admin` → redirect to `/admin/dashboard`
- If user role = `customer` → redirect to `/dashboard`

---

## 🛡️ Role-Based Access in Routes

```tsx
<ProtectedRoute role="admin">
  <AdminLayout>
    <AdminDashboard />
  </AdminLayout>
</ProtectedRoute>
```

- Blocks users from accessing unauthorized routes
- Can show "Access Denied" or redirect to `/dashboard`

---

## 🧱 Layout Hierarchy

| Layout        | Used For           |
|---------------|--------------------|
| `AuthLayout`  | Login, Signup      |
| `UserLayout`  | Customer Pages     |
| `AdminLayout` | Admin Panel        |
| `PageWrapper` | Consistent UI base |

---

## 📚 Related Docs

- [`components.md`](./components.md)
- [`pages-and-layouts.md`](./pages-and-layouts.md)
- [`role-based-access.md`](../auth/role-based-access.md)

```
