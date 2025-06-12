```markdown
# ORiem Finance – Database Schema Overview

## Tables

### 1. users
- `id` (UUID, PK)
- `full_name` (String)
- `email` (String, Unique)
- `phone_number` (String)
- `password_hash` (String)
- `role` (Enum: 'customer', 'admin')
- `created_at` (Timestamp)
- `updated_at` (Timestamp)

### 2. accounts
- `id` (UUID, PK)
- `user_id` (FK → users.id)
- `account_type` (Enum: 'savings', 'current')
- `account_number` (String, Unique)
- `balance` (Decimal)
- `status` (Enum: 'active', 'inactive', 'frozen')
- `created_at` (Timestamp)

### 3. transactions
- `id` (UUID, PK)
- `account_id` (FK → accounts.id)
- `type` (Enum: 'deposit', 'withdrawal', 'transfer')
- `amount` (Decimal)
- `description` (String)
- `recipient_account` (String, nullable)
- `status` (Enum: 'pending', 'completed', 'failed')
- `timestamp` (Timestamp)

### 4. loans
- `id` (UUID, PK)
- `user_id` (FK → users.id)
- `amount` (Decimal)
- `term_months` (Integer)
- `interest_rate` (Decimal)
- `status` (Enum: 'pending', 'approved', 'rejected', 'repaid')
- `created_at` (Timestamp)

### 5. loan_repayments
- `id` (UUID, PK)
- `loan_id` (FK → loans.id)
- `amount_paid` (Decimal)
- `payment_date` (Date)

### 6. audit_logs
- `id` (UUID, PK)
- `admin_id` (FK → users.id)
- `action` (Text)
- `target` (Text)
- `timestamp` (Timestamp)
- `ip_address` (String)

### 7. role_permissions
- `id` (UUID, PK)
- `role` (String)
- `permission` (String)

## Relationships

- **users** 1 ⇨ * **accounts**
- **accounts** 1 ⇨ * **transactions**
- **users** 1 ⇨ * **loans**
- **loans** 1 ⇨ * **loan_repayments**
- **users** (admin) 1 ⇨ * **audit_logs**

## Roles

- `customer`: Can view accounts, perform transactions, apply for loans.
- `admin`: Can approve/reject loans, view audit logs, manage users.
```
