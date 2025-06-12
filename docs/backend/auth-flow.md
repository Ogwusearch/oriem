```markdown
# 🔐 ORiem Finance – Backend Authentication Flow

This document outlines the full authentication flow for ORiem Finance’s backend API using **JWT-based stateless authentication**.

---

## 📌 Overview

The authentication system is built using **FastAPI**, **JWT tokens**, and **Supabase** as the user store. The system supports:

- ✅ Signup
- ✅ Login
- ✅ Token validation
- ✅ Role-based access
- ✅ Optional refresh token mechanism

---

## 🔄 Flow Summary

```text
[User] → Login/Register
      ↓
[Backend API] → Verify credentials (check Supabase)
      ↓
[Backend] → Generate JWT token
      ↓
[Client] ← Receives access token (and optional refresh token)
      ↓
[Client] → Attaches token to API requests (Authorization header)
      ↓
[Backend API] → Verifies token and grants access
```

---

## 🔑 Endpoints

| Method | Endpoint            | Description                   |
|--------|---------------------|-------------------------------|
| POST   | /auth/signup         | Register a new user           |
| POST   | /auth/login          | Authenticate and issue tokens |
| GET    | /auth/me             | Get current user profile      |
| POST   | /auth/refresh-token  | (Optional) Refresh JWT token  |

---

## 🧠 JWT Token Structure

```json
{
  "sub": "user_id_abc123",
  "role": "admin",
  "email": "admin@example.com",
  "iat": 1718027600,
  "exp": 1718031200
}
```

- `sub`: User ID
- `role`: Role for RBAC
- `iat`: Issued at
- `exp`: Expiry time

---

## 🧪 FastAPI Auth Example

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer
from jose import jwt, JWTError

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="auth/login")

def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        user_id = payload.get("sub")
        if user_id is None:
            raise HTTPException(status_code=401, detail="Invalid token")
        return {"user_id": user_id, "role": payload.get("role")}
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
```

---

## 🛡️ Security Measures

| Risk            | Mitigation Strategy                        |
|-----------------|---------------------------------------------|
| Token theft     | Use `httpOnly` secure cookies or secure storage |
| Token replay    | Track token usage patterns (optional IP/device) |
| XSS attacks     | Avoid using localStorage for tokens        |
| Expired tokens  | Validate `exp` claim before access         |

---

## 🔁 Refresh Token Strategy (Optional)

- Store a long-lived refresh token securely
- Validate with `/auth/refresh-token` to issue new access token
- Refresh token may be stored in a secure HTTP-only cookie

---

## 📁 Related Files

- [`session-management.md`](../auth/session-management.md)
- [`role-based-access.md`](../auth/role-based-access.md)
- [`jwt-auth.md`](../auth/jwt-auth.md)
```
