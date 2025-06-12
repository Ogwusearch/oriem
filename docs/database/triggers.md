```markdown
# 🔁 ORiem Finance – Database Triggers

This document outlines the key database triggers implemented in the ORiem Finance system. Triggers are used to automate system tasks such as audit logging, enforcing constraints, or sending notifications when certain database operations occur.

---

## 📌 What Are Triggers?

Triggers are special procedures in SQL that automatically execute in response to certain events on a table, such as `INSERT`, `UPDATE`, or `DELETE`.

---

## ✅ Audit Log Trigger

**Purpose:** Automatically logs activity for key tables (accounts, transactions, loans) into the `audit_logs` table.

### ➕ Trigger: `log_activity_on_insert`
**Fires on:** `INSERT`  
**Tables covered:** `accounts`, `transactions`, `loans`  
**Action:** Inserts an audit record detailing the user and the action.

```sql
CREATE OR REPLACE FUNCTION log_insert_activity()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO audit_logs(user_id, action, table_name, timestamp)
  VALUES (NEW.created_by, 'INSERT', TG_TABLE_NAME, NOW());
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Attach to relevant tables
CREATE TRIGGER log_insert_accounts
AFTER INSERT ON accounts
FOR EACH ROW EXECUTE FUNCTION log_insert_activity();

CREATE TRIGGER log_insert_transactions
AFTER INSERT ON transactions
FOR EACH ROW EXECUTE FUNCTION log_insert_activity();

CREATE TRIGGER log_insert_loans
AFTER INSERT ON loans
FOR EACH ROW EXECUTE FUNCTION log_insert_activity();
```

---

## 🔄 Balance Check Trigger

**Purpose:** Prevents overdrafts during withdrawals or transfers.

### 🔁 Trigger: `check_sufficient_balance`
**Fires on:** `INSERT` into `transactions`  
**Condition:** If transaction type is `'withdraw'` or `'transfer'`  
**Action:** Validates balance before allowing transaction.

```sql
CREATE OR REPLACE FUNCTION validate_balance()
RETURNS TRIGGER AS $$
DECLARE
  acc_balance NUMERIC;
BEGIN
  IF NEW.type IN ('withdraw', 'transfer') THEN
    SELECT balance INTO acc_balance FROM accounts WHERE id = NEW.sender_id;

    IF acc_balance < NEW.amount THEN
      RAISE EXCEPTION 'Insufficient balance';
    END IF;
  END IF;

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER enforce_balance
BEFORE INSERT ON transactions
FOR EACH ROW EXECUTE FUNCTION validate_balance();
```

---

## 🧾 Loan Approval Timestamp Trigger

**Purpose:** Automatically sets `approved_at` timestamp when a loan is approved.

```sql
CREATE OR REPLACE FUNCTION set_loan_approval_timestamp()
RETURNS TRIGGER AS $$
BEGIN
  IF NEW.status = 'approved' AND OLD.status IS DISTINCT FROM 'approved' THEN
    NEW.approved_at := NOW();
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER auto_set_loan_approval_date
BEFORE UPDATE ON loans
FOR EACH ROW EXECUTE FUNCTION set_loan_approval_timestamp();
```

---

## 📥 Repayment Balance Update Trigger

**Purpose:** Updates remaining loan balance after a repayment.

```sql
CREATE OR REPLACE FUNCTION update_loan_balance()
RETURNS TRIGGER AS $$
BEGIN
  UPDATE loans
  SET amount = amount - NEW.amount_paid
  WHERE id = NEW.loan_id;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER adjust_loan_balance
AFTER INSERT ON loan_repayments
FOR EACH ROW EXECUTE FUNCTION update_loan_balance();
```

---

## 🧠 Notes

- Triggers run at the **database level** and are critical for enforcing integrity.
- Always test thoroughly to avoid recursive or unintended consequences.
- Supabase provides audit hooks, but custom triggers give more control.

---

## 📎 Related Docs

- [`tables.md`](./tables.md)
- [`security-rules.md`](./security-rules.md)
- [`../backend/error-handling.md`](../backend/error-handling.md)

```
