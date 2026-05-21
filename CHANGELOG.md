# Changelog

All notable changes to **PesoPlan** are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.3.0] — 2026-05-21

### Added

#### Analytics Module
- New **Analytics** page accessible via `Ctrl+7` and the sidebar (`BarChart2` icon).
- Three-tab layout: **Insights**, **Income Loss Simulator**, and **Purchase Planner**.
- New `AppView` value `'analytics'` added to `types/index.ts`.
- New `AnalyticsModule.tsx` component under `src/components/modules/`.

#### Analytics — Insights Tab
- Proactive cashflow analysis engine with 10 detection rules, grouped by severity (critical / warning / info):
  1. **Income source ending** — alerts when an active income source ends this month or next, with the monthly income impact.
  2. **Cashflow gap** — flags any month where outflows exist but income is zero.
  3. **Deficit months** — lists months where net cashflow (income − bills − expenses − savings) is negative, with total deficit amount.
  4. **Bill spike** — detects months where bills exceed 130% of the year's average bill amount.
  5. **Expense spike** — detects months where expenses exceed 140% of the average across months that have expenses.
  6. **Loan ending soon** — positive alert when a loan or subscription ends within the next 2 months, showing freed-up amount.
  7. **Savings goal at risk** — warns if projected pool allocation will not reach a goal's `target_amount` by `target_month`/`target_year`.
  8. **Low savings rate** — info card when the effective annual savings rate falls below 80% of the budget target.
  9. **High bills-to-income ratio** — warns when bills consume more than 60% of income in any month.
  10. **Surplus streak** — positive insight when 3 or more consecutive months have positive net cashflow.
- Summary stat cards: Critical count, Warning count, Info count, Projected Savings YTD, and Average Monthly Net.
- Empty state when no issues are detected.

#### Analytics — Income Loss Simulator Tab
- Dropdown to select an income source and a start month for the simulated loss.
- Per-month table comparing baseline vs. scenario for income, net cashflow, and savings.
- Row-level indicators: months that newly go into deficit, months that cannot cover bills.
- Summary cards: total income lost, total savings impact, new deficit months, and months that can't cover bills.

#### Analytics — Purchase Planner Tab
- Inputs: purchase name, price, down payment, annual interest rate (%), loan term (months or years), and loan start month/year.
- Outputs:
  - Monthly payment, total payment, total interest, total cost, cost-increase %, and down payment %.
  - DTI (debt-to-income) ratio with rating: Comfortable (<20%) / Manageable (<30%) / Stretched (<40%) / Risky (≥40%).
  - Net cashflow after adding the new loan payment, with affordability indicator.
  - Months to save for the down payment at the current average savings rate.
  - Recommended down payment to stay under a 28% DTI threshold.
  - Month-by-month cashflow impact table for the loan's duration (capped at 12 months displayed).
  - Full amortization schedule: payment number, monthly payment, principal portion, interest portion, remaining balance.
  - Down payment comparison table for 10%, 20%, 30%, and the user-chosen percentage — showing monthly payment, total interest, total cost, and DTI per scenario.
- Handles 0% interest rate (simple division amortization).
- New `SavingsGoal` fields `target_month` and `target_year` used by goal-risk detection in Insights tab.

#### Security — Idle Auto-lock
- Session locks automatically after **5 minutes of inactivity** (configurable via the `idle_timeout_minutes` setting, default `'5'`).
- Timer resets on any `mousedown`, `mousemove`, `keydown`, `scroll`, or `touchstart` event.
- On timeout, the user is logged out and returned to the account selection screen.
- `idle_timeout_minutes` field added to the `Settings` interface in `types/index.ts` and default added to `useAppStore.ts`.

---

## [1.2.0] — 2026-03-31

### Added

#### Dashboard — Pay Period Coverage
- Expandable month rows in the annual overview table showing pay period breakdown.
- Each pay period card displays the income source, payday range, per-period income, assigned bills, total bill amount, and coverage status (Covered / Short).
- Supports all pay cycle types: weekly, bi-weekly, semi-monthly, monthly, and custom.
- New utility functions `getSourcePaydays` and `getPayPeriodsForMonth` in `utils.ts`.
- New `PayPeriod` and `PayPeriodBill` interfaces in `types/index.ts`.

#### Bills & Loans — Redesigned UI
- **Summary stat cards** — Current month total, Loans total, Subs & Utilities total, and Paid This Month counter at the top of the page.
- **Filter bar** — Search bills by name, filter by type (Loan / Subscription / Utility / One-time), and filter by status (Active / Completed). Shows "X of Y bills" count.
- **Sorted bill cards** — Bills grouped by type priority (Loans → Subscriptions → Utilities → One-time) with colored left borders and type icons.
- **Type and payment badges** — Color-coded type badge and Paid/Unpaid badge per bill card.
- **Improved payment tracker** — Current month highlighted with blue border, paid months in emerald green with check icon, inactive months dimmed. Cleaner button sizing.

#### Bills & Loans — Edit with Confirmation
- **Pencil icon** on each bill card to enter inline edit mode.
- Inline edit form replaces the card content with all editable fields (name, type, amount, due day, start month, term, already paid).
- Edited card highlighted with a blue ring.
- **Confirmation modal** on save — shows a diff of all changes (old → new) with changed fields highlighted in amber. Unchanged fields shown in gray.
- User must type **"Edit"** (case-sensitive) to enable the Confirm button.

#### Bills & Loans — Delete with Confirmation
- **Delete confirmation modal** — red-themed, shows bill details (name, type badge, amount, due day, term for loans).
- User must type **"Delete"** (case-sensitive) to enable the Confirm button.
- Clicking backdrop or Cancel dismisses the modal.

#### Bills & Loans — Success Toasts
- Green success banner after a confirmed edit or delete action.
- Shows the bill name and action taken (updated / deleted).
- Auto-dismisses after 3 seconds with manual close button.

### Fixed
- **PostCSS config warning** — Converted `postcss.config.js` from ES module (`export default`) to CommonJS (`module.exports`) to eliminate the `MODULE_TYPELESS_PACKAGE_JSON` warning on startup.
- **Pay period bill assignment** — Bills were incorrectly assigned to the period whose date range *contained* the due day, meaning a bill due on the 21st appeared under the 25th Payday. Periods now run **from the payday forward** (payday → next payday − 1), so bills are only shown under the paycheck that has already arrived.
- **Advance-paid bills shown in coverage** — Bills already marked as paid for the current month were still appearing in the pay period coverage cards. Paid bills are now excluded from the breakdown, correctly reflecting remaining obligations.
- **Before 1st Payday carry-over** — Bills due before the month's first payday are now grouped into a dedicated "Before 1st Payday" card with ₱0 income and a "Carry-over from previous month" label, making it clear these require funds from the prior month's last paycheck.

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
