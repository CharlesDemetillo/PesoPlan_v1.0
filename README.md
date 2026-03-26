# PesoPlan — Release Notes

**Version 1.1.0** · March 26, 2026  
Personal Finance Tracker for Windows  
Developed by Charles Alexis Demetillo

---

## What's New in v1.1.0

### Improvements
- **Bills header now shows current month's total** — The Bills & Loans page subtitle displays the correct total for the current real-world month instead of a cumulative sum of all bills regardless of their active period.
- **Desktop notifications for bill due dates** — PesoPlan can now alert you when a bill's due date is approaching. Configure how many days in advance you want to be notified in the Setup page under **Notifications**.

### Bug Fixes
- **Loans no longer count past their term** — A 3-month loan now stops after exactly 3 months. Previously, unpaid future months were still being included in the bills total beyond the loan's end date.
- **One-time charges no longer repeat** — Bills marked as `One-time` are now only counted for their start month. Previously they were incorrectly included in every following month.
- **Savings column now shows ₱0 before goals start** — The Dashboard, Cashflow, and Savings module all consistently show ₱0 savings for months before your earliest savings goal begins.
- **Savings pool is consistent across all pages** — The monthly savings pool in the Savings module now matches the figures shown in Dashboard and Cashflow for the same month.
- **Bills calculation is now consistent across all pages** — Dashboard, Cashflow, and Savings modules all use the same bill calculation logic, eliminating discrepancies when manually verifying totals.

---

## System Requirements

| | Minimum |
|---|---|
| OS | Windows 10 (64-bit) or later |
| RAM | 256 MB |
| Storage | 150 MB free disk space |
| Display | 1024 × 600 resolution or higher |

> Internet connection is **not required**. PesoPlan is fully offline.

---

## Installation

1. Run **PesoPlan-Setup-1.1.0.exe**
2. Follow the installer prompts
3. Choose your installation directory (default: `C:\Program Files\PesoPlan`)
4. Launch PesoPlan from the Desktop shortcut or Start Menu

To uninstall, go to **Settings → Apps** and remove PesoPlan, or use the uninstaller in the installation directory.

---

## First-Time Setup

1. On first launch, click **Add Account**
2. Enter your name, a PIN (minimum 4 characters), and a security question for recovery
3. Log in with your PIN
4. Go to **Setup** to configure:
   - Your income sources and pay cycle
   - Your savings rate (%)
   - Expense categories and budget limits
5. Add your bills under **Bills & Loans**
6. Start tracking expenses under **Expenses**

---

## Data Storage

Your data is stored **locally** on your computer at:

```
C:\Users\<YourName>\AppData\Roaming\pesoplan\
```

Each account has its own database file. **Back up this folder** to preserve your data.

---

## Known Limitations

- Maximum of **5 accounts** per installation
- Bill tracking is **year-scoped** — use Year Management in Setup to carry data into a new year
- Savings goals are **goal-scoped by start month** — months before the earliest goal show ₱0 in the savings pool

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + 1` | Dashboard |
| `Ctrl + 2` | Setup |
| `Ctrl + 3` | Bills & Loans |
| `Ctrl + 4` | Expenses |
| `Ctrl + 5` | Cashflow |
| `Ctrl + 6` | Savings |

---

## Previous Release

### v1.0.0 — January 1, 2026 (Initial Release)

- Multi-account PIN authentication with security question recovery
- Dashboard with KPI cards, cashflow bar chart, and expense breakdown pie chart
- Setup module — income sources, budget targets, expense categories, year management
- Bills & Loans — recurring bills, loans with term tracking, one-time charges
- Expenses — monthly logging with budget vs. actual view and over-budget alerts
- Cashflow — monthly breakdown with cumulative tracking
- Smart Savings — goal-based pool with per-goal allocation and monthly breakdown
- Fully offline, local SQLite storage

---

## Contact

**Charles Alexis Demetillo**  
Portfolio Project — for personal use and demonstration purposes only.

Copyright © 2026 Charles Alexis Demetillo. All rights reserved.
