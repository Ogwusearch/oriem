```markdown
# 🔐 ORiem Finance – Supabase Database Security Rules

This document outlines the row-level security (RLS) policies and general database security measures in place for ORiem Finance, ensuring secure access and data integrity across all roles and operations.

---

## 🔒 RLS (Row-Level Security) Overview

Supabase uses PostgreSQL’s native **Row-Level Security (RLS)**. Policies must be defined per table to control who can read/write data.

---

## 🧑‍💼 Roles

| Role        | Description                      |
|-------------|----------------------------------|
| `admin`     | Full access to all tables        |
| `customer`  | Access only to own data          |
| `employee`  | Limited access for support tasks |

---

## ✅ Common Policy Template

```sql
-- Example: Only allow customers to view their own data
CREATE POLICY "Customers can view their own records"
  ON customers
  FOR SELECT
  USING (auth.uid() = id);
```

---

## 🗄️ Table-Level RLS Examples

### 1. `customers`

```sql
-- Only allow reading own customer data
CREATE POLICY "Customer can read their data"
  ON customers
  FOR SELECT
  USING (auth.uid() = id);

-- Only admins can insert/update/delete
CREATE POLICY "Admins can modify customer data"
  ON customers
  FOR ALL
  TO admin
  USING (true);
```

---

### 2. `accounts`

```sql
-- Customers can only view their own accounts
CREATE POLICY "Customer view own accounts"
  ON accounts
  FOR SELECT
  USING (customer_id = auth.uid());

-- Allow admin and employee to read/update
CREATE POLICY "Admin and Employee can manage accounts"
  ON accounts
  FOR ALL
  TO admin, employee
  USING (true);
```

---

### 3. `transactions`

```sql
-- Customers can view their own transactions
CREATE POLICY "View own transactions"
  ON transactions
  FOR SELECT
  USING (sender_id = auth.uid() OR receiver_id = auth.uid());

-- Admins can view all transactions
CREATE POLICY "Admin can view all"
  ON transactions
  FOR SELECT
  TO admin
  USING (true);
```

---

### 4. `loans`

```sql
-- Customers can apply and view their loans
CREATE POLICY "Customer loan access"
  ON loans
  FOR SELECT, INSERT
  USING (customer_id = auth.uid());

-- Admins can approve or reject
CREATE POLICY "Admin loan control"
  ON loans
  FOR UPDATE, DELETE
  TO admin
  USING (true);
```

---

## 🔑 Authentication Integration

Supabase’s `auth.uid()` returns the user ID from the JWT token and is used in policies to restrict access.

---

## 📁 Storage Rules (Optional)

If you're using Supabase Storage:

```sql
-- Only the file owner can read/write
CREATE POLICY "File owner access only"
  ON storage.objects
  FOR ALL
  USING (auth.uid() = owner);
```

---

## 🧪 Testing Policies

Use Supabase SQL Editor or CLI:

```sql
-- Simulate as a user
SET auth.uid = 'user-uuid';
SELECT * FROM transactions;
```

---

## 🚨 Best Practices

- Always enable RLS after creating policies.
- Keep roles well-defined and minimal.
- Audit access via `audit_logs` table.
- Validate inputs on the backend in addition to RLS.

---

## 🔗 Related

- [`jwt-auth.md`](../auth/jwt-auth.md)
- [`role-based-access.md`](../auth/role-based-access.md)
- [`database-schema.md`](./database-schema.md)

```
