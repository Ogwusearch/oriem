```markdown
# ⚙️ ORiem Finance – State Management Guide

This guide outlines the state management approach for the ORiem Finance frontend application.

---

## 🧰 Tools Used

| Tool           | Purpose                                |
|----------------|----------------------------------------|
| **Zustand**    | Global state management                |
| **React Context** | Authentication and Theme context     |
| **React Query** | API data fetching and caching         |
| **LocalStorage** | Persistence for auth tokens/settings  |

---

## 🗂️ Directory Structure

```bash
src/
├── store/               # Zustand stores
│   ├── useUserStore.ts
│   ├── useAccountStore.ts
│   ├── useLoanStore.ts
│   └── useUIStore.ts
├── context/             # React Context Providers
│   ├── AuthContext.tsx
│   ├── ThemeContext.tsx
│   └── NotificationContext.tsx
```

---

## 🧠 Zustand Store Example

### `useUserStore.ts`

```ts
import { create } from 'zustand';

export const useUserStore = create((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));
```

Access in a component:

```tsx
const { user, setUser } = useUserStore();
```

---

## 🧠 React Context Example

### `AuthContext.tsx`

```tsx
import { createContext, useContext, useState, useEffect } from 'react';

const AuthContext = createContext(null);

export const AuthProvider = ({ children }) => {
  const [auth, setAuth] = useState(null);

  useEffect(() => {
    const token = localStorage.getItem('token');
    if (token) setAuth({ token });
  }, []);

  return (
    <AuthContext.Provider value={{ auth, setAuth }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => useContext(AuthContext);
```

---

## 🔁 API State with React Query

### Usage in a component:

```tsx
import { useQuery } from '@tanstack/react-query';

const fetchAccounts = async () => {
  const res = await fetch('/api/accounts');
  return res.json();
};

const { data, isLoading, error } = useQuery(['accounts'], fetchAccounts);
```

---

## 💾 Persistence

| Data                    | Storage          | Method          |
|-------------------------|------------------|------------------|
| JWT Token               | `localStorage`   | `setItem/getItem` |
| User preferences/theme  | `localStorage`   | or `Zustand persist` |
| Notifications read      | Zustand (session only) | – |

---

## 🧪 Testing State

Use [React Testing Library](https://testing-library.com/) with mocked Zustand stores or context providers to isolate component logic from global state.

---

## 🧩 When to Use What?

| Situation                        | Tool           |
|----------------------------------|----------------|
| User auth/session info           | React Context  |
| Global UI (modals, theme, loading) | Zustand       |
| API-driven data (accounts, loans) | React Query    |
| Auth token, theme preference     | LocalStorage   |

---

## 🔗 Related Docs

- [`components.md`](./components.md)
- [`routing.md`](./routing.md)
- [`jwt-auth.md`](../auth/jwt-auth.md)

```
