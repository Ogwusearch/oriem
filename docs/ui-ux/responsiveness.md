# Responsive Design Guidelines

This document outlines the responsive design strategy for ORiem Finance. Our goal is to ensure that the app provides a seamless experience across devices — mobile, tablet, and desktop.

---

## 📐 Breakpoints

We follow a mobile-first approach with the following standard breakpoints:

| Device        | Width (px)     |
|---------------|----------------|
| Mobile        | 0 - 640px      |
| Tablet        | 641px - 1024px |
| Small Desktop | 1025px - 1440px|
| Large Desktop | 1441px and up  |

These breakpoints are used in both CSS and component logic.

---

## 🧱 Layout Structure

- **Fluid grids**: Use percentage-based widths and flex/grid layouts.
- **Max-width containers**: Prevent content from stretching on large screens.
- **Sidebars & drawers**:
  - Collapsible on tablets and mobile.
  - Persistent on large desktops.

---

## 🧩 Component Responsiveness

| Component        | Behavior on Mobile                             |
|------------------|-------------------------------------------------|
| Navbar           | Converts to hamburger menu                     |
| Sidebar          | Hidden behind a drawer, opens via button       |
| Tables           | Becomes card-style list or horizontal scroll   |
| Forms            | Single-column with full-width inputs           |
| Charts           | Responsive via container resizing              |
| Modals           | Fullscreen with clear exit action              |

---

## 📱 Mobile-First Design Principles

- Design for the smallest screen first.
- Optimize tap targets (minimum 48px height/width).
- Prioritize vertical stacking and scrolling.
- Avoid hover-only features (use tap or long-press alternatives).

---

## 🎯 Best Practices

- **Media queries**: Use `em` or `rem` for scalability.
- **Grid/flex utilities**: Avoid fixed pixel widths.
- **Typography**: Use fluid type scales or clamp().
- **Icons & images**: Use SVGs or responsive `<img>` sizes.
- **Test devices**: Simulate multiple screen sizes during development.

---

## 🧪 Tools & Testing

- **Browser DevTools**: Chrome device toolbar, Firefox responsive mode.
- **Lighthouse**: Audit for mobile performance & accessibility.
- **Polypane / Responsively**: Visualize across many screen sizes.
- **Manual testing**: iOS, Android, tablet, and desktop devices.

---

## 💡 Example CSS Snippet

```css
/* Example mobile-first layout */
.container {
  display: flex;
  flex-direction: column;
  padding: 1rem;
}

@media (min-width: 768px) {
  .container {
    flex-direction: row;
    padding: 2rem;
  }
}
```

---

## 🔁 Continuous Improvement

- Conduct regular UX tests on mobile and tablet.
- Track user behavior and bounce rates by device.
- Optimize performance and loading speed for 3G/4G networks.

---

_Last updated: June 11, 2025_
