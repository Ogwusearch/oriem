# 🔐 Access Control

Access control ensures that only authorized users can access or modify specific resources within the ORiem Finance platform. This document outlines the strategy for Role-Based Access Control (RBAC), permissions, and secure practices.

---

## 🛡️ 1. Access Control Strategy

We use **Role-Based Access Control (RBAC)** to manage access throughout the system. Each user is assigned a role, and each role has specific permissions associated with it.

**Core Roles:**

| Role      | Description                        |
|-----------|------------------------------------|
| admin     | Full system access (manage users, loans, transactions) |
| employee  | Limited access to customer-related operations |
| customer  | Access to personal account data and operations |

---

## 🧾 2. Permissions Matrix

| Feature                  | Admin | Employee | Customer |
|--------------------------|:-----:|:--------:|:--------:|
| View User Accounts       | ✅    | ✅       | ❌       |
| Manage User Accounts     | ✅    | ❌       | ❌       |
| View Own Account         | ❌    | ❌       | ✅       |
| Transfer Funds           | ❌    | ❌       | ✅       |
| View All Transactions    | ✅    | ✅       | ❌       |
| View Own Transactions    | ❌    | ❌       | ✅       |
| Approve Loans            | ✅    | ✅       | ❌       |
| Apply for Loan           | ❌    | ❌       | ✅       |
| Manage Roles & Permissions | ✅  | ❌       | ❌       |

---

## 🔑 3. Role Enforcement

Access control is enforced at:

- **Backend Routes:** Each protected endpoint checks the user role before performing actions.
- **Frontend UI:** UI elements are conditionally rendered based on the user role.
- **Database (Supabase):** RLS (Row-Level Security) policies restrict unauthorized data access.

---

## 🧩 4. Supabase RLS Policy Example

**Only allow customers to view their own transactions:**

```sql
CREATE POLICY "Customers can view their transactions"
ON transactions
FOR SELECT
TO authenticated
USING (auth.uid() = customer_id);
```

---

## 🔄 5. Changing Roles

Role changes are performed by **admins only** via the admin dashboard or directly in the database. Every role update is:

- Logged in the `audit_logs` table
- Requires admin authentication and confirmation

---

## 🔍 6. Auditing & Logs

All access control violations or escalations are logged. See [`audit-trails.md`](../security/audit-trails.md) for more.

---

## 🚨 7. Best Practices

- Use environment variables to secure sensitive keys
- Never expose `admin` privileges to frontend logic
- Always validate tokens and roles server-side
- Regularly review permissions and RLS policies

---

_Last updated: June 11, 2025_
