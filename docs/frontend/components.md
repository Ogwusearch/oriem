```markdown
# 🧩 ORiem Finance – Frontend Components Guide

This document outlines reusable React components used in the ORiem Finance frontend, categorized by domain and purpose. Components are organized for scalability, clean UI, and consistent design.

---

## 🗂️ Directory Structure

```bash
src/
└── components/
    ├── common/
    ├── auth/
    ├── dashboard/
    ├── accounts/
    ├── transactions/
    ├── loans/
    ├── admin/
    ├── notifications/
    ├── profile/
    └── utils/
```

---

## 🔧 Common Components

| Component          | Purpose                                 |
|--------------------|-----------------------------------------|
| `Button`           | Reusable button (primary, secondary)    |
| `Input`            | Text, email, password fields            |
| `SelectDropdown`   | Reusable dropdowns                      |
| `Modal`            | Dialogs, confirmations, forms           |
| `Card`             | Layout container for data blocks        |
| `ToastNotification`| Success/error info alerts               |
| `Tabs`             | Tabbed interface                        |
| `Loader`           | Spinners, loading indicators            |

---

## 🔐 Auth Components

| Component             | Purpose                              |
|------------------------|--------------------------------------|
| `LoginForm`            | Login input + validation             |
| `SignupForm`           | Registration form                    |
| `ForgotPasswordForm`   | Reset password flow                  |
| `OTPVerificationInput` | 6-digit OTP input field              |
| `AuthWrapper`          | Layout for login/signup              |

---

## 👤 User & Profile

| Component         | Purpose                                  |
|-------------------|------------------------------------------|
| `UserAvatar`      | Profile image and initials               |
| `UserProfileCard` | User's name, email, status               |
| `EditProfileForm` | Update profile info                      |
| `ThemeToggle`     | Light/dark mode switch                   |
| `LanguageSelector`| Dropdown for language preferences        |

---

## 💳 Account Components

| Component         | Purpose                                  |
|-------------------|------------------------------------------|
| `AccountCard`     | Show balance, type, status               |
| `AccountDetails`  | Expanded view of account                 |
| `AccountForm`     | Create/edit account                      |

---

## 💸 Transaction Components

| Component              | Purpose                              |
|------------------------|--------------------------------------|
| `TransactionList`      | Display transaction history          |
| `TransactionItem`      | One line per transaction             |
| `TransactionFilter`    | Date/status/type filters             |
| `TransactionSummaryCard`| Aggregated data view               |
| `TransferForm`         | Transfer funds between accounts      |
| `ReceiptModal`         | Display transfer receipt             |

---

## 🏦 Loan Components

| Component              | Purpose                              |
|------------------------|--------------------------------------|
| `LoanApplicationForm`  | Apply for new loan                   |
| `LoanDetailsCard`      | Show loan amount, status, schedule   |
| `LoanStatusTracker`    | Visual status: pending → approved    |
| `RepaymentHistoryTable`| List repayments made                 |
| `LoanApprovalPanel`    | Admin: approve/reject actions        |

---

## 🧮 Analytics & Admin

| Component             | Purpose                               |
|-----------------------|---------------------------------------|
| `StatsCard`           | KPIs (e.g., Total Users, Revenue)     |
| `ChartWidget`         | Bar, Line, Pie using chart lib        |
| `UserManagementTable` | Admin view of all users               |
| `LoanManagementTable` | Approve, reject, view all loans       |
| `TransactionOverviewTable`| Full transaction dataset         |
| `AuditLogItem`        | Action log: who, what, when           |
| `RolePermissionEditor`| Admin: edit access levels             |

---

## 📬 Mailbox & Notifications

| Component              | Purpose                              |
|------------------------|--------------------------------------|
| `MessageCard`          | One inbox message                    |
| `InboxList`            | Full message list                    |
| `SendMessageForm`      | Contact or support form              |
| `SystemNotificationCard`| Alerts for account, loans, etc     |

---

## 🧰 Utilities

| Component              | Purpose                              |
|------------------------|--------------------------------------|
| `EmptyState`           | Placeholder for no data              |
| `PaginationControls`   | Navigate paginated data              |
| `SearchBar`            | Filter table or cards                |
| `ConfirmationDialog`   | Confirm user actions (delete, etc.)  |
| `ErrorBoundary`        | Catch and display render errors      |

---

## 🧩 Design Guidelines

- **Atomic Design:** Build small → complex (e.g. Button → Card → Dashboard)
- **CSS-in-JS / SCSS Modules / Styled Components** supported
- **Accessibility** (`aria`, keyboard-nav) is mandatory
- All components are **responsive & mobile-friendly**

---

## 📚 Related Docs

- [`folder-structure.md`](../architecture/folder-structure.md)
- [`routes.md`](../../backend/routes.md)
- [`user-management.md`](../../admin-panel/user-management.md)
```
