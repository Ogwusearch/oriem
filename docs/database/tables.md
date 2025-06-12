```markdown
# 📊 ORiem Finance – Database Tables Reference

This document provides a reference guide to all primary tables used in the ORiem Finance system. It includes key fields, relationships, and usage context for each table.

---

## 🧾 Table List

| Table Name         | Purpose                                    |
|--------------------|--------------------------------------------|
| `users`            | Stores all user credentials and roles      |
| `customers`        | Profile info for banking customers         |
| `admins`           | Profile info for admin users               |
| `accounts`         | Bank account details per customer          |
| `transactions`     | All transactions (debit, credit, transfer) |
| `loans`            | Loan applications and status               |
| `loan_repayments`  | Repayment records linked to loans          |
| `audit_logs`       | Tracks system activity for accountability  |
| `role_permissions` | Role-based access control reference        |

---

## 🧑 `users`

- Stores login credentials and auth roles.
- Linked with `customers` or `admins`.

| Field         | Type      | Description               |
|---------------|-----------|---------------------------|
| `id`          | UUID      | Primary key, auth UID     |
| `email`       | Text      | Login email               |
| `password`    | Text      | Hashed password           |
| `role`        | Text      | 'admin' or 'customer'     |
| `created_at`  | Timestamp | Signup time               |

---

## 🧍 `customers`

- Contains customer profile details.

| Field          | Type      | Description              |
|----------------|-----------|--------------------------|
| `id`           | UUID      | FK → users.id            |
| `full_name`    | Text      | Customer full name       |
| `phone`        | Text      | Phone number             |
| `address`      | Text      | Mailing address          |
| `dob`          | Date      | Date of birth            |
| `created_at`   | Timestamp | Account creation date    |

---

## 🧑‍💼 `admins`

- Admin profiles with system privileges.

| Field          | Type      | Description          |
|----------------|-----------|----------------------|
| `id`           | UUID      | FK → users.id        |
| `full_name`    | Text      | Admin full name      |
| `email`        | Text      | Admin contact email  |
| `created_at`   | Timestamp | Created time         |

---

## 🏦 `accounts`

- Represents bank accounts linked to customers.

| Field          | Type      | Description                |
|----------------|-----------|----------------------------|
| `id`           | UUID      | Primary key                |
| `customer_id`  | UUID      | FK → customers.id          |
| `account_type` | Text      | 'savings', 'current'       |
| `balance`      | Numeric   | Current balance            |
| `created_at`   | Timestamp | Account opening date       |

---

## 💳 `transactions`

- Tracks deposits, withdrawals, transfers.

| Field         | Type      | Description                     |
|---------------|-----------|---------------------------------|
| `id`          | UUID      | Primary key                     |
| `type`        | Text      | 'deposit', 'withdraw', 'transfer' |
| `amount`      | Numeric   | Amount involved                 |
| `sender_id`   | UUID      | FK → accounts.id (nullable)     |
| `receiver_id` | UUID      | FK → accounts.id (nullable)     |
| `created_at`  | Timestamp | Transaction time                |

---

## 🧾 `loans`

- Loan application and tracking.

| Field          | Type      | Description                |
|----------------|-----------|----------------------------|
| `id`           | UUID      | Primary key                |
| `customer_id`  | UUID      | FK → customers.id          |
| `amount`       | Numeric   | Requested loan amount      |
| `status`       | Text      | 'pending', 'approved', 'rejected' |
| `approved_by`  | UUID      | FK → admins.id (nullable)  |
| `created_at`   | Timestamp | Application submission time|

---

## 💰 `loan_repayments`

- Tracks repayment history for each loan.

| Field         | Type      | Description               |
|---------------|-----------|---------------------------|
| `id`          | UUID      | Primary key               |
| `loan_id`     | UUID      | FK → loans.id             |
| `amount_paid` | Numeric   | Amount of repayment       |
| `paid_on`     | Date      | Date of repayment         |

---

## 📋 `audit_logs`

- Logs system activity and changes.

| Field         | Type      | Description                    |
|---------------|-----------|--------------------------------|
| `id`          | UUID      | Primary key                    |
| `user_id`     | UUID      | FK → users.id                  |
| `action`      | Text      | Description of the action      |
| `table_name`  | Text      | Table affected (optional)      |
| `timestamp`   | Timestamp | When the action occurred       |

---

## 🔐 `role_permissions`

- Role-based access control mappings.

| Field        | Type    | Description                  |
|--------------|---------|------------------------------|
| `role`       | Text    | 'admin', 'customer'          |
| `permission` | Text    | Action (e.g., 'loan:approve')|

---

## 🔗 Relationships Overview

- `users` ↔ `customers`/`admins`: One-to-one
- `customers` ↔ `accounts`: One-to-many
- `accounts` ↔ `transactions`: One-to-many (as sender/receiver)
- `customers` ↔ `loans` ↔ `loan_repayments`: One-to-many

---

## 📎 Related Docs

- [`database-schema.md`](./database-schema.md)
- [`security-rules.md`](./security-rules.md)
- [`../auth/role-based-access.md`](../auth/role-based-access.md)

```
