# Changelog

All notable changes to **PesoPlan** are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.1.0] — 2026-03-26

### Added
- **Desktop Notifications** — Bill due date alerts via the OS Notifications API. Notifies for overdue, due today, and due-soon bills (configurable days-before threshold). Triggers on app startup and when bill data changes.
- **Notification Settings UI** — Toggle and days-before slider added to the Setup module under a dedicated Notifications section.
- **`getMonthlyBillAmount` utility** — Canonical shared function in `utils.ts` used by Dashboard, Cashflow, and Savings modules to ensure consistent bill calculations across the entire app.

### Fixed
- **Loan termination bug** — Loans were counted past their `term_months` because termination used paid-month count (which never reached the term for unpaid future months). Fixed to use position-based `endMonth = start_month + remainingPayments - 1`, which correctly terminates loans regardless of payment status.
- **One-time bill type** — Bills of type `One-time` were repeating every month from `start_month` onward. Now correctly charged only on `start_month`.
- **CashflowModule missing term check** — Bills loop had no `term_months` or `One-time` guard, causing loans to run indefinitely and one-time charges to repeat.
- **SavingsModule bills calculation** — Bills total used a naive `monthly_payment` sum with only a `start_month` check, ignoring loan terms and One-time types.
- **BillsModule header total** — "Total monthly" was summing all bills' `monthly_payment` unconditionally. Now uses `getMonthlyBillAmount` for the real-world current month and labels the field with the month name (e.g. `Mar total:`).
- **Savings column in pre-goal months** — Dashboard and Cashflow were showing a savings amount for months where no savings goals were active yet. Savings is now `₱0` for any month before the earliest goal's `start_month`, consistent with the Savings module pool.
- **Savings pool March showing ₱0** — Monthly Savings Pool in the Savings module was returning `₱0` for months before any goal started. Restored correct behavior: pool is `₱0` when no goals are active (by design), matching user intent.
- **Tooltip clipping** — Tooltips inside overflow-hidden containers were clipped. Fixed by rendering tooltips via `ReactDOM.createPortal` at `document.body` level with `position: fixed` coordinates.

---

## [1.0.0] — 2026-01-01

### Added

#### Authentication & Accounts
- Multi-account system — up to 5 independent user accounts.
- PIN-based login with Argon2 hashing.
- Security question recovery flow for forgotten PINs.
- Account selection screen on launch.

#### Dashboard
- KPI cards — Total Income YTD, Total Outflow YTD, Net Savings YTD, Average Savings Rate.
- Monthly cashflow bar chart (income, bills, expenses, savings) using Recharts.
- Expense breakdown pie chart by category.
- Color-coded annual overview table with columns: Month, Income, Bills, Expenses, Savings, Net, Rate, Status.
- Hover tooltips on Net, Rate, and Status headers explaining the metric.

#### Setup Module
- Income sources with pay cycle types: weekly, bi-weekly, semi-monthly, monthly, custom.
- Per-source `start_month` and `end_month` for partial-year income.
- Budget targets based on the 50/30/20 rule.
- Expense categories with custom budget limits.
- Year Management — add new fiscal years, carry over bills, income sources, and savings goals to the next year with `paid_before` tracking.

#### Bills & Loans
- Bill types: Subscription, Utility, Loan, One-time.
- Monthly payment toggles per bill per month.
- Loan progress bar showing percent of term completed.
- `paid_before` field to carry over partial loans from prior years.
- `start_month` to control when a bill becomes active within the year.
- Due day tracking per bill.

#### Expenses
- Monthly expense log with category assignment.
- Separate Month and Year selectors for cross-year browsing.
- Budget vs. actual view with over-budget alerts per category.

#### Cashflow Module
- Monthly breakdown table: Income, Bills, Expenses, Savings, Net, Cumulative, Flow.
- Cumulative net cashflow line chart.
- Year totals row.
- Flow indicator (up/down arrow) per month.

#### Smart Savings
- Goal-based savings from a monthly pool calculated as `(income − bills − expenses) × save_pct`.
- Goal cards with target amount, allocated amount, progress bar, remaining, monthly needed, start month, and status.
- Expandable monthly savings pool table rows showing per-goal allocation and distribution bars.
- `share_pct` per goal controlling how the pool is divided.
- Pool only activates for months where at least one goal is active (`start_month ≤ month`).

#### UI & UX
- Collapsible sidebar with icon-only mode.
- Portal-based hover tooltips that escape overflow containers.
- Comma-formatted peso currency inputs (e.g. `₱100,000.00`) using `CurrencyInput`.
- Alternating row stripes and per-column color coding across all tables.
- Keyboard shortcuts: `Ctrl+1` through `Ctrl+6` for module navigation.
- Fully offline — all data stored in a per-account SQLite database in the OS user data directory.

---

## Versioning

This project follows [Semantic Versioning](https://semver.org/):
- **MAJOR** — breaking changes to data schema or core behavior
- **MINOR** — new features, backward-compatible
- **PATCH** — bug fixes and small improvements
