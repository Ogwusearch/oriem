# UI Component Styling Guide

This document outlines the standard styles, theming, and guidelines for UI components in the ORiem Finance application.

## 🎨 Design Language

- **Design System**: Custom Tailored Design (no Tailwind)
- **Color Palette**:
  - Primary: `#0052cc` (Deep Blue)
  - Secondary: `#f4f5f7` (Light Gray)
  - Success: `#36b37e`
  - Warning: `#ffab00`
  - Error: `#ff5630`
- **Typography**:
  - Font Family: `Inter, sans-serif`
  - Base Font Size: `16px`
  - Font Weights: 400 (normal), 600 (semi-bold), 700 (bold)

---

## 📦 Core Component Styles

### ✅ Button

```css
.button {
  padding: 10px 16px;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s ease;
}

.button.primary {
  background-color: #0052cc;
  color: white;
}

.button.secondary {
  background-color: #f4f5f7;
  color: #0052cc;
}

.button:hover {
  opacity: 0.9;
}
```

### 🧾 Input

```css
.input {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}

.input:focus {
  border-color: #0052cc;
  outline: none;
}
```

### 📄 Card

```css
.card {
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.04);
}
```

---

## 🌗 Theme Support

We support **Light and Dark Mode** via CSS variables:

### Light Theme

```css
:root {
  --bg-color: #ffffff;
  --text-color: #1e1e1e;
  --primary-color: #0052cc;
}
```

### Dark Theme

```css
[data-theme="dark"] {
  --bg-color: #1e1e1e;
  --text-color: #ffffff;
  --primary-color: #4c9aff;
}
```

---

## 🧠 UX Guidelines

- Use meaningful icons and labels
- Maintain adequate spacing between components
- Avoid overuse of colors; use colors to indicate status
- Maintain accessibility (ARIA labels, keyboard nav, contrast)

---

## 📌 Component Checklist

| Component      | Status  | Notes                      |
|----------------|---------|----------------------------|
| Button         | ✅ Done | Primary, Secondary, Disabled |
| Input          | ✅ Done | Text, Password, Email      |
| Card           | ✅ Done | With header and footer     |
| Modal          | 🚧 WIP | Animations & accessibility |
| Tabs           | 🚧 WIP | Mobile responsive          |
| Tooltip        | ✅ Done | Delay and hover trigger    |

---

## 📁 Location

All reusable components are stored in:
```
/frontend/src/components/
```

---

## 🔄 Updates

To propose changes:
1. Fork the repo or checkout a new branch
2. Update styles and test across themes
3. Submit a pull request with screenshots or GIFs

---

_Last updated: June 11, 2025_
