# 🔧 Supabase Configuration Guide

This guide walks you through setting up Supabase for the **ORiem Finance** platform.

---

## 🏁 1. Create a Supabase Project

1. Go to [https://app.supabase.com](https://app.supabase.com)
2. Click **"New Project"**
3. Enter:
   - **Project Name**: `oriem-finance`
   - **Database Password**: Set a secure password (save it)
4. Click **"Create Project"**

---

## 🔑 2. Get Your API Keys

Once the project is created:

- Navigate to **Project Settings > API**
- Copy:
  - `Project URL`
  - `anon` public key (for frontend)
  - `service_role` key (for backend)

---

## 🗄️ 3. Set Up Database

Use the **SQL Editor** to run schema creation scripts (see [`database-schema.md`](../architecture/database-schema.md)):

1. Go to **SQL Editor**
2. Click **New Query**
3. Paste your table creation SQL (e.g., for `accounts`, `transactions`, etc.)
4. Click **Run**

---

## 🔁 4. Enable Row Level Security (RLS)

1. Go to **Table Editor**
2. For each sensitive table (e.g., `accounts`, `transactions`, `users`):
   - Click the table
   - Toggle **RLS** to **ON**
   - Add policies (see [`security-rules.md`](../database/security-rules.md))

---

## 🪝 5. Set Up Triggers & Functions

1. Use SQL Editor to run any custom triggers/functions (e.g., audit logs)
2. See [`triggers.md`](../database/triggers.md) for details

---

## 🔄 6. Supabase Configuration in `.env`

Example for the **frontend**:

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

Example for the **backend**:

```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

---

## 🚨 7. Security Tips

- Do **not** expose `service_role` key in frontend
- Use environment variables, not hardcoded strings
- Apply appropriate RLS policies for every sensitive table
- Monitor usage and enable **log retention**

---

## ✅ Verification

- Test inserting/fetching data from Supabase using Postman or code
- Confirm permissions and policies are enforced
- Use Supabase Auth if needed (email/password, OAuth, etc.)

---

_Last updated: June 11, 2025_
