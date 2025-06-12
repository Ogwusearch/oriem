# 🌿 Environment Variables Example

This file provides a reference `.env` file used across the ORiem Finance project. Copy and rename this file to `.env` in the respective directories (`/`, `/frontend`, `/backend`, etc.) and replace values with your actual credentials.

---

## 📁 Root `.env`

```env
# === General Configuration ===
NODE_ENV=development
PORT=3000

# === Supabase Credentials ===
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# === JWT Secret ===
JWT_SECRET=your-jwt-secret

# === Database URL ===
DATABASE_URL=postgresql://user:password@localhost:5432/oriem_db
```

---

## 📁 Frontend `.env.local`

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
VITE_API_BASE_URL=http://localhost:8000
```

---

## 📁 Backend `.env`

```env
# === FastAPI Config ===
API_PORT=8000
CORS_ORIGINS=http://localhost:5173

# === Supabase ===
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# === PostgreSQL ===
DATABASE_URL=postgresql://user:password@localhost:5432/oriem_db

# === JWT ===
JWT_SECRET=your-backend-jwt-secret
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

---

## 🔐 Notes

- Do **not** commit `.env` files to version control.
- Use [Doppler](https://doppler.com/) or similar tools to manage secrets securely.
- Always rotate secrets for production and keep development keys limited in scope.

---

_Last updated: June 11, 2025_
