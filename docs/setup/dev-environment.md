# 🛠️ Development Environment Setup

This guide outlines how to set up the development environment for ORiem Finance. Follow these steps to get your local workspace ready for frontend, backend, and database development.

---

## 📦 Prerequisites

Ensure you have the following installed:

- **Node.js** (v18.x or higher)
- **npm** or **pnpm** (preferred)
- **Python** (v3.10+ for FastAPI backend)
- **PostgreSQL CLI** (optional, for manual DB access)
- **Git**
- **Docker** (optional, for running services)
- **VS Code** or any IDE

---

## 📁 Project Structure Overview

```
oriem/
├── frontend/          # React (Vite)
├── backend/           # FastAPI + Node.js services
├── docs/              # Documentation
├── .env               # Shared environment variables
└── README.md
```

---

## 🔧 Setup Steps

### 1. Clone the Repository

```bash
git clone git@github.com:your-org/oriem.git
cd oriem
```

### 2. Install Frontend Dependencies

```bash
cd frontend
pnpm install
# or
npm install
```

### 3. Install Backend Dependencies

For Python backend:

```bash
cd ../backend/python
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

For Node.js backend:

```bash
cd ../node
pnpm install
```

---

## ⚙️ Environment Variables

Create a `.env` file in the root and respective folders:

```env
# .env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your-service-role-key
JWT_SECRET=your-super-secret-key
DATABASE_URL=postgresql://user:pass@localhost:5432/oriem
```

Also create `.env.local` in frontend and `.env` in backend.

---

## ▶️ Running the Project

### Start Frontend

```bash
cd frontend
pnpm dev
```

### Start Python Backend

```bash
cd backend/python
uvicorn main:app --reload
```

### Start Node.js Backend (if used)

```bash
cd backend/node
pnpm dev
```

---

## 🧪 Testing Setup

- Frontend: `vitest`, `@testing-library/react`
- Backend: `pytest`, `httpx`, or `supertest` for Node

Run tests:

```bash
# Frontend
pnpm test

# Backend (Python)
pytest
```

---

## 🐳 Docker (Optional)

For containerized dev setup:

```bash
docker-compose up --build
```

Configure `docker-compose.yml` for:
- Supabase local
- Backend services
- PostgreSQL

---

## 🔄 Syncing with Supabase

Use Supabase CLI to pull/push schema:

```bash
supabase link --project-ref your-project-id
supabase db pull
supabase db push
```

---

## ✅ Final Checks

- [ ] All services start successfully
- [ ] `.env` files configured
- [ ] Able to connect to Supabase
- [ ] Frontend connects to backend
- [ ] Auth and DB flows functional

---

_Last updated: June 11, 2025_
