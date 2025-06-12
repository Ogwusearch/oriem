```markdown
# 🔐 JWT Authentication – ORiem Finance

This document explains how JSON Web Token (JWT) authentication is implemented in the ORiem Finance system.

---

## 📌 Overview

JWT is used to securely transmit information between the client and server as a digitally signed token. In ORiem, we use JWT for stateless authentication, enabling secure access to protected resources.

---

## 🔁 Auth Flow

1. **Login Request**
   - User submits credentials (`email`, `password`)
   - POST `/api/auth/login`

2. **Token Generation**
   - If valid, server generates:
     - `access_token` (short-lived)
     - `refresh_token` (optional, longer-lived)
   - Tokens are signed with a secret key

3. **Frontend Storage**
   - `access_token` is stored in memory or localStorage
   - Used in Authorization header for API requests

4. **Protected Routes**
   - FastAPI dependency checks token:
     - Signature
     - Expiry
     - Scope/role

---

## 🛠️ JWT Structure

A JWT token contains three parts:
```
header.payload.signature
```

Example:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6... (header.payload.signature)
```

---

## 🧪 Sample JWT Payload

```json
{
  "sub": "user_id_12345",
  "email": "user@example.com",
  "role": "customer",
  "exp": 1718120390
}
```

---

## 🔧 FastAPI Implementation

### Dependencies

```python
from fastapi import Depends, HTTPException
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError, jwt

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/api/auth/login")
SECRET_KEY = "your-secret"
ALGORITHM = "HS256"
```

### Decode & Verify Token

```python
def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        user_id = payload.get("sub")
        if user_id is None:
            raise HTTPException(status_code=401, detail="Invalid token")
        return {"user_id": user_id, "role": payload.get("role")}
    except JWTError:
        raise HTTPException(status_code=401, detail="Token error")
```

---

## 🧹 Token Expiry

- Access Token: 15–30 minutes
- Refresh Token (optional): 7 days

---

## 🛡️ Security Tips

- Use HTTPS always
- Keep the `SECRET_KEY` secret and strong
- Implement token blacklisting or rotation (for refresh tokens)
- Protect admin routes with role checks

---

## 🔄 Logout

- Frontend deletes the access token
- (Optional) Backend revokes refresh token or adds it to a blacklist

---

## 📁 Related Docs

- [API Endpoints](../api/endpoints.md)
- [System Overview](../architecture/system-overview.md)
- [User Management](../admin-panel/user-management.md)
```
