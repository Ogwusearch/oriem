```markdown
# 📁 ORiem Finance – Folder Structure

This document outlines the recommended folder structure for the **ORiem Finance** full-stack web application, including frontend (React), backend (FastAPI), and documentation.

---

## 📦 Root Directory (`/workspaces/oriem/`)

```
/workspaces/oriem/
├── backend/                # FastAPI app (Python)
├── frontend/               # React app (Vite-based)
├── docs/                   # Markdown documentation
├── scripts/                # Utility/setup/deploy scripts
├── .env                    # Environment variables
├── README.md               # Project overview
└── requirements.txt        # Backend dependencies
```

---

## 📁 Backend (`/backend/`)

```
backend/
├── app/
│   ├── api/                # Route definitions
│   ├── core/               # App config, auth, security
│   ├── models/             # SQLAlchemy ORM models
│   ├── schemas/            # Pydantic models (request/response)
│   ├── services/           # Business logic
│   ├── db/                 # DB session & init
│   └── utils/              # Helper functions
├── tests/                  # Backend unit & integration tests
├── main.py                 # FastAPI app entry point
└── alembic/                # DB migrations
```

---

## 📁 Frontend (`/frontend/`)

```
frontend/
├── public/                 # Static assets (favicon, etc.)
├── src/
│   ├── assets/             # Images, icons, styles
│   ├── components/         # Shared React components
│   ├── features/           # Feature-based folders (e.g., auth, dashboard)
│   ├── pages/              # Page-level components
│   ├── routes/             # Route configuration
│   ├── services/           # API calls (axios, etc.)
│   ├── store/              # Global state (e.g., Redux/Zustand)
│   ├── hooks/              # Custom hooks
│   └── utils/              # Frontend utility functions
├── index.html              # App HTML shell
└── main.tsx / main.jsx     # React entry point
```

---

## 📁 Documentation (`/docs/`)

```
docs/
├── architecture/
│   ├── database-schema.md
│   ├── folder-structure.md
│   └── system-design.md
├── api/
│   ├── overview.md
│   └── endpoints.md
├── admin-panel/
│   ├── user-management.md
│   ├── loan-approvals.md
│   └── audit-trails.md
└── testing/
    └── testing-strategy.md
```

---

## ✅ Notes

- Follow the **feature-based** structure in both backend and frontend for scalability.
- Keep **documentation up-to-date** under the `/docs/` folder.
- Environment-specific settings go in `.env` and should never be committed.
- Use **alembic** for managing database migrations.
```
