# 🔐 Data Encryption

Data encryption is a critical security layer in ORiem Finance that protects sensitive information from unauthorized access during storage and transmission.

---

## 📌 1. Why Encryption Matters

Encryption helps to:

- Protect user data from data breaches.
- Ensure compliance with security standards (e.g., GDPR, PCI-DSS).
- Prevent tampering or unauthorized access.

---

## 🔒 2. Types of Encryption Used

### A. **Encryption at Rest**
Applies to data stored in databases, backups, and local files.

- **Supabase (PostgreSQL)**:
  - Uses AES-256 encryption at the disk level.
  - Data stored in the `accounts`, `transactions`, and `loans` tables is protected by default.
  - Encrypted fields (e.g., account numbers) can be manually encrypted using libraries like `cryptography` in Python.

### B. **Encryption in Transit**
Applies to data moving between the client, backend, and Supabase.

- HTTPS is enforced across:
  - Frontend ↔ Backend
  - Backend ↔ Supabase
- TLS 1.2 or higher is required.
- JWT tokens, API keys, and session cookies are transmitted securely via HTTPS.

---

## 🧰 3. Encryption Implementation (Backend)

### Python (FastAPI)
```python
from cryptography.fernet import Fernet

# Generate a key (store securely)
key = Fernet.generate_key()
cipher = Fernet(key)

# Encrypt sensitive data
encrypted_data = cipher.encrypt(b"1234-5678-9012-3456")

# Decrypt when needed
decrypted_data = cipher.decrypt(encrypted_data)
```

Store `key` securely in environment variables or a secrets manager like HashiCorp Vault or Supabase secrets.

---

## 🔐 4. JWT Token Encryption

- JWTs are **signed** using HMAC SHA-256.
- They are not encrypted but contain encoded payloads.
- Store minimal user info (e.g., user ID, role).
- JWTs are stored in **httpOnly cookies** for frontend clients to prevent XSS access.

---

## 🧮 5. Supabase Row-Level Security (RLS)

While not encryption, RLS protects data access by:

- Enforcing **policies** on who can read/write rows.
- Preventing unauthorized exposure of encrypted content.

```sql
-- Example RLS policy
CREATE POLICY "Only owners can read"
ON transactions
FOR SELECT
USING (user_id = auth.uid());
```

---

## 📦 6. Encrypting Specific Fields

For highly sensitive fields (e.g., SSN, bank account numbers):

- Encrypt before inserting into Supabase.
- Decrypt only when needed by authorized services.
- Avoid storing plaintext in logs or analytics.

---

## ✅ 7. Best Practices

- 🔑 Use strong encryption keys and rotate them periodically.
- 🧪 Audit encrypted fields for correct implementation.
- 🚫 Never log sensitive unencrypted values.
- 🔐 Store secrets using environment variables or secret managers.
- ⚙️ Enable RLS for all tables with user-specific data.

---

_Last updated: June 11, 2025_
