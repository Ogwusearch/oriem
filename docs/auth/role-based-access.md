```markdown
# 🔐 Role-Based Access Control (RBAC) – ORiem Finance

This document outlines how RBAC is implemented in ORiem Finance to enforce secure, role-specific permissions across the system.

---

## 📌 Overview

RBAC ensures that users can only access functionalities that are permitted for their roles. In ORiem, the following roles are defined:

- `admin`: Full access to all resources
- `customer`: Access to their own account data and transactions
- `auditor` (optional): Read-only access to logs and reports

---

## 🧩 Role Definitions

Roles are stored in the `role_permissions` table in Supabase:

```sql
-- Example: role_permissions table
| id | role      | permission             |
|----|-----------|------------------------|
| 1  | admin     | manage_users           |
| 2  | admin     | view_transactions      |
| 3  | customer  | view_own_account       |
| 4  | customer  | initiate_transfer      |
| 5  | auditor   | view_audit_logs        |
```

Each API route or UI component checks permissions before allowing access.

---

## ⚙️ Implementation in FastAPI

### Middleware or Dependency

```python
def check_permission(required_permission: str):
    def dependency(user: dict = Depends(get_current_user)):
        role = user["role"]
        if not has_permission(role, required_permission):
            raise HTTPException(status_code=403, detail="Access denied")
        return user
    return dependency
```

### Permission Checker

```python
def has_permission(role: str, required_permission: str) -> bool:
    permissions = {
        "admin": ["manage_users", "view_transactions", "approve_loans"],
        "customer": ["view_own_account", "initiate_transfer", "apply_loan"],
        "auditor": ["view_audit_logs"]
    }
    return required_permission in permissions.get(role, [])
```

---

## 🚫 Protected Routes Example

```python
@app.get("/api/admin/users", dependencies=[Depends(check_permission("manage_users"))])
def get_all_users():
    ...
```

Only users with `manage_users` permission (e.g., admins) can access this endpoint.

---

## 🛡️ Best Practices

- Enforce RBAC both in frontend (UI visibility) and backend (API protection)
- Avoid hardcoding permissions; prefer a database-driven structure
- Always validate roles and permissions server-side
- Consider logging unauthorized attempts in the audit log

---

## 📁 Related Docs

- [JWT Auth](./jwt-auth.md)
- [Admin Panel – User Management](../admin-panel/user-management.md)
- [API Endpoints](../api/endpoints.md)
```
