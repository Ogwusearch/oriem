# 🧑‍💼 Admin Panel: Loan Approvals

This section of the ORiem Finance admin panel enables authorized staff to manage incoming loan applications efficiently. It provides tools for reviewing applicant details, approving or rejecting loans, and tracking active loan statuses.

---

## 📌 Key Features

- View list of all loan applications.
- Filter by status: `pending`, `approved`, `rejected`, `active`.
- Review loan details before making decisions.
- Approve or reject with one click.
- Record decision-maker (admin) and timestamp.
- View applicant history and creditworthiness.

---

## 📂 UI Layout

### Loan Approvals Table

| Field        | Description                            |
|--------------|----------------------------------------|
| Customer     | Full name of loan applicant            |
| Amount       | Loan amount requested (e.g., $5,000)   |
| Term         | Duration of loan (e.g., 12 months)     |
| Interest     | Interest rate (e.g., 7.5%)             |
| Status       | `pending`, `approved`, `rejected`      |
| Actions      | Approve ✅ / Reject ❌ / View Details 🔍 |

---

## 🔍 Review Modal (Details View)

Displayed when the admin clicks **View Details**:

- **Customer Information**
  - Full name
  - Email
  - Account number
  - Credit score (if available)
- **Loan Request**
  - Amount
  - Duration
  - Interest rate
  - Total repayment estimate
- **Previous Loans**
  - History of prior loans (if any)
  - Repayment behavior
- **Decision Form**
  - Approve or reject
  - Optional comment field
  - Confirm action button

---

## ⚙️ API Integration

### Fetch Loan Applications

```http
GET /api/admin/loans?status=pending
Authorization: Bearer <admin_token>
```

### Approve Loan

```http
POST /api/admin/loans/{loan_id}/approve
Authorization: Bearer <admin_token>
```

**Request Body:**
```json
{
  "comment": "Approved based on repayment history"
}
```

### Reject Loan

```http
POST /api/admin/loans/{loan_id}/reject
Authorization: Bearer <admin_token>
```

**Request Body:**
```json
{
  "comment": "Insufficient income for requested amount"
}
```

---

## 🛡️ Role-Based Access

Only users with the `loan_approval` or `manage_loans` role can:

- View pending applications
- Approve/reject loans
- Access repayment records

---

## 📝 Audit Logging

Each action (approve/reject) must be logged:

### Audit Log Entry

| Field         | Description                                |
|---------------|--------------------------------------------|
| action        | `approve_loan` or `reject_loan`            |
| actor_id      | Admin who performed the action             |
| target_id     | Loan ID                                    |
| comment       | Admin's optional comment                   |
| timestamp     | When the action was taken                  |
| metadata      | JSON: IP address, role, etc.               |

---

## ✅ Best Practices

- Always review the borrower’s account and transaction history.
- Avoid approving multiple active loans for the same user.
- Use comments to provide internal context for each decision.
- Notify customers automatically after a decision is made.

---

## 📣 Notifications

| Trigger                  | Recipient  | Message Type       |
|--------------------------|------------|---------------------|
| Loan approved            | Customer   | Email / in-app alert |
| Loan rejected            | Customer   | Email / in-app alert |
| Decision pending > 7 days| Admin role | Reminder / alert    |

---

## 🧭 Navigation Path

```text
Admin Panel → Loans → Loan Approvals
```

---

## 📌 Future Enhancements

- Add credit scoring integration
- Auto-approval rules based on AI thresholds
- Bulk approval/rejection tools
