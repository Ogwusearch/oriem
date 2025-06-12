```markdown
# ❌ ORiem Finance – Backend Error Handling Guide

This document outlines the best practices and implementation patterns for handling errors in the **FastAPI backend** of ORiem Finance.

---

## 🧩 Error Categories

| Category        | Description                                 | Example                         |
|-----------------|---------------------------------------------|----------------------------------|
| Validation Error | Invalid input data                         | Missing required field           |
| Auth Error       | Login, token, or access-related issues     | Invalid token, unauthorized      |
| Not Found        | Resource doesn’t exist                     | Account ID not found             |
| Conflict         | Duplicate or conflicting operation         | Duplicate email or transaction   |
| Server Error     | Unexpected backend failures                | DB errors, logic exceptions      |

---

## 📦 Error Response Format (Standardized)

```json
{
  "status": "error",
  "message": "User not found",
  "code": 404,
  "details": {
    "resource": "User",
    "id": "12345"
  }
}
```

- `status`: always `"error"` for failed requests
- `message`: human-readable summary
- `code`: HTTP status code
- `details`: (optional) more context for debugging/logging

---

## 🚦 HTTP Status Code Guide

| Code | Type             | Usage                            |
|------|------------------|----------------------------------|
| 400  | Bad Request       | Invalid parameters, validation   |
| 401  | Unauthorized      | Missing or invalid token         |
| 403  | Forbidden         | Valid token but no permission    |
| 404  | Not Found         | Resource doesn’t exist           |
| 409  | Conflict          | Duplicate entry or logic clash   |
| 422  | Unprocessable     | Schema validation failures       |
| 500  | Server Error      | Unhandled exception              |

---

## 🛠️ FastAPI Global Error Handler

```python
from fastapi import Request, HTTPException
from fastapi.responses import JSONResponse
from fastapi.exception_handlers import RequestValidationError
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException

@app.exception_handler(HTTPException)
async def http_exception_handler(request: Request, exc: HTTPException):
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "status": "error",
            "message": exc.detail,
            "code": exc.status_code,
        },
    )

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=422,
        content={
            "status": "error",
            "message": "Validation failed",
            "code": 422,
            "details": exc.errors(),
        },
    )
```

---

## 📋 Best Practices

- ✅ Always return **structured** and **predictable** error responses
- ✅ Use descriptive messages and include context (`resource`, `field`, etc.)
- ✅ Log all server errors (`500`) with full stack traces (internally)
- ✅ Sanitize messages — never expose internal DB errors or stack traces in production responses
- ✅ Write tests for error scenarios (invalid input, unauthorized access)

---

## 🧪 Example Error Response

```json
{
  "status": "error",
  "message": "Email already exists",
  "code": 409,
  "details": {
    "field": "email"
  }
}
```

---

## 📁 Related Docs

- [`auth-flow.md`](auth-flow.md)
- [`session-management.md`](../auth/session-management.md)
- [`api/endpoints.md`](../api/endpoints.md)
```
