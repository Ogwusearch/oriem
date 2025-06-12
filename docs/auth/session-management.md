```markdown
# 🔐 Session Management – ORiem Finance

This document outlines how session management is handled in ORiem Finance to maintain secure and seamless user authentication throughout the application.

---

## 📌 Overview

ORiem uses **JWT (JSON Web Tokens)** for stateless authentication. A session begins when a user logs in and is issued a token, which is then sent with each request for authorization.

---

## 🔑 Session Lifecycle

1. **Login**: User submits credentials → JWT is issued on successful auth
2. **Token Storage**: Token is stored in:
   - **Web**: `httpOnly` Secure Cookie (recommended) or localStorage (fallback)
   - **Mobile**: Secure storage (Keychain, Keystore)
3. **Token Use**: Token is attached to `Authorization` header in API requests
4. **Session Expiry**: Token includes an `exp` (expiration) claim
5. **Refresh** (Optional): Refresh tokens can extend sessions without forcing re-login

---

## 🧠 JWT Payload Example

```json
{
  "sub": "user_id_123",
  "role": "customer",
  "email": "user@example.com",
  "iat": 1718027600,
  "exp": 1718031200
}
```

- `sub`: Subject (user ID)
- `role`: User role for RBAC
- `iat`: Issued at (timestamp)
- `exp`: Expiration time

---

## 🔄 Token Refresh (Optional Strategy)

If using short-lived access tokens:

- Issue a **refresh token** with longer TTL
- Store in secure cookie
- Expose endpoint: `POST /auth/refresh-token`
- On access token expiry, use refresh token to issue a new one

---

## 🛡️ Security Considerations

| Threat                     | Mitigation Strategy                                |
|---------------------------|----------------------------------------------------|
| Token theft                | Use httpOnly cookies, secure storage               |
| XSS                        | Avoid localStorage for tokens                      |
| Token replay               | Add IP/device checks or rotate refresh tokens      |
| Session fixation           | Re-issue tokens on login/logout                    |
| Token expiration bypass    | Validate `exp` on every request                    |

---

## 🧪 Session Validation in FastAPI

```python
from fastapi import Depends, HTTPException, status
from jose import JWTError, jwt

def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        user_id = payload.get("sub")
        if user_id is None:
            raise credentials_exception
        return {"user_id": user_id, "role": payload.get("role")}
    except JWTError:
        raise HTTPException(status_code=status.HTTP_401_UNAUTHORIZED)
```

---

## 🔁 Logout Strategy

- **Frontend**: Clear token from storage (cookie/localStorage)
- **Backend**: (If using refresh tokens) blacklist the refresh token or rotate it
- Optional: Invalidate refresh tokens in DB or cache (e.g., Redis)

---

## 📁 Related Docs

- [JWT Auth](./jwt-auth.md)
- [Role-Based Access Control](./role-based-access.md)
- [API Overview](../api/overview.md)
```
