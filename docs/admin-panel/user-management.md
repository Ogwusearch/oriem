# 👤 Admin Panel: User Management

This section enables admins to manage all registered users of the ORiem Finance platform — both customers and staff. Functions include viewing profiles, updating statuses, assigning roles, and ensuring compliance.

---

## 📌 Key Features

- View all users (customers, admins, support staff)
- Filter and search by:
  - Role (Customer, Admin, Support)
  - Status (Active, Suspended, Deactivated)
  - Email or Name
- View user profile & account history
- Assign / change roles and permissions
- Activate, suspend, or delete accounts
- Export user data to CSV

---

## 📂 UI Layout

### User Table

| Field        | Description                       |
|--------------|-----------------------------------|
| Full Name    | User’s full legal name            |
| Email        | Registered email address          |
| Role         | `customer`, `admin`, `support`    |
| Status       | `active`, `suspended`, `inactive` |
| Created At   | Date of registration              |
| Actions      | View 👁️ / Edit ✏️ / Suspend 🚫 / Delete ❌ |

---

## 🔍 User Profile View

- **Basic Info**
  - Name, email, phone, address
  - Date of birth
  - KYC documents (if uploaded)
- **Accounts**
  - List of linked accounts
  - Balances and recent transactions
- **Loan Activity**
  - Past and current loans
  - Repayment status
- **Role & Permissions**
  - Assigned role
  - Permission groups
- **Account Controls**
  - Reset password
  - Suspend or deactivate
  - Add internal notes

---

## ⚙️ API Endpoints

### Get All Users

```http
GET /api/admin/users
Authorization: Bearer <admin_token>
```

### Update User Role

```http
PUT /api/admin/users/{user_id}/role
Authorization: Bearer <admin_token>
```

**Request Body:**
```json
{
  "role": "support"
}
```

### Suspend User

```http
POST /api/admin/users/{user_id}/suspend
Authorization: Bearer <admin_token>
```

### Delete User

```http
DELETE /api/admin/users/{user_id}
Authorization: Bearer <admin_token>
```

---

## 🛡️ Role-Based Access

Only users with `user_management` or `admin` role can:

- View and manage users
- Change roles and permissions
- Suspend or delete accounts

---

## 📝 Audit Logging

Every critical change must be logged:

### Audit Log Example

| Field       | Description                           |
|-------------|---------------------------------------|
| action      | `suspend_user`, `change_role`, etc.   |
| actor_id    | Admin who performed the action        |
| target_id   | ID of the affected user               |
| timestamp   | When the action occurred              |
| comment     | Optional reason or justification      |
| metadata    | IP address, browser info, etc.        |

---

## ✅ Best Practices

- Always verify identity before suspending users.
- Use role-based permissions to avoid over-privileged accounts.
- Record all actions for future audits.
- Monitor inactive accounts for suspicious behavior.

---

## 📣 Notifications

| Action          | Recipient | Notification Type     |
|------------------|-----------|------------------------|
| Role changed     | User      | Email / in-app         |
| Account suspended| User      | Email with reason      |
| New admin added  | Superadmin| Email notification     |

---

## 🧭 Navigation Path

```text
Admin Panel → Users → User Management
```

---

## 📌 Future Enhancements

- Bulk role assignment
- KYC approval queue
- Advanced user risk scoring
