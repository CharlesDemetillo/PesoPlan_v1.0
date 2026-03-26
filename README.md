# PesoPlan 💰

A local-first personal finance tracker desktop app built with **Electron**, **React**, **TypeScript**, and **SQLite**. Designed for the Philippine market — tracks income, bills, expenses, cashflow, and savings goals with full offline support and PIN-based security.

---

## Features

- 🔒 **Multi-account support** — Up to 5 accounts, each with PIN-based authentication and security question recovery
- 📊 **Dashboard** — KPIs, monthly cashflow bar chart, expense breakdown pie chart, and color-coded annual overview table with hover tooltips
- ⚙️ **Setup** — Configure income sources (weekly/bi-weekly/semi-monthly/monthly/custom), budget targets (50/30/20 rule), expense categories, and year management
- 📅 **Year Management** — Add new fiscal years, carry over bills/income/savings to the next year, track all years side by side
- 🧾 **Bills & Loans** — Track recurring bills and loans with monthly payment toggles, loan progress bars, and term tracking
- 🛒 **Expenses** — Log monthly expenses with separate Month and Year selectors; budget vs. actual view with over-budget alerts
- 💰 **Cashflow** — Auto-aggregated monthly breakdown with income, bills, expenses, savings, net, cumulative, and flow columns
- 🐷 **Smart Savings** — Goal-based savings from a monthly pool; expandable rows show per-goal allocation per month
- 💱 **Currency Inputs** — All peso amount fields formatted with commas (e.g. ₱100,000.00) as you type
- 🌐 **Fully offline** — All data stored locally in a SQLite database per user account

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript, Tailwind CSS |
| State | Zustand |
| Charts | Recharts |
| Icons | Lucide React |
| Desktop | Electron 28 |
| Database | SQLite via `better-sqlite3` |
| Auth | Argon2 PIN hashing |
| Build | Vite + electron-builder |

---

## Prerequisites

- [Node.js](https://nodejs.org/) v18 or later
- npm v9 or later

---

## Getting Started

### 1. Install dependencies

```bash
cd pesoplan
npm install
```

### 2. Rebuild native modules for Electron

`better-sqlite3` is a native addon and must be compiled against Electron's Node version:

```bash
npx electron-rebuild -f -w better-sqlite3
```

> Run this once after `npm install`, and again whenever you update Electron.

### 3. Start the development environment

```bash
npm run electron:dev
```

This starts the Vite dev server. `vite-plugin-electron` automatically builds and launches the Electron window. The app will hot-reload on frontend changes.

---

## First-Time Setup

1. On first launch, you'll see the **Account Selection** screen
2. Click **Add Account**, enter a name, choose a PIN (minimum 4 characters), and set a security question
3. Log in with your PIN — you can manage up to **5 separate accounts**
4. Start in **Setup** to configure your income sources, expense categories, and budget targets
5. Use **Year Management** in Setup to add a new fiscal year or carry over existing data when the year ends

---

## Available Scripts

| Command | Description |
|---|---|
| `npm run electron:dev` | Start full Electron app in development mode |
| `npm run dev` | Start Vite dev server only (browser preview, no IPC) |
| `npm run build` | Compile TypeScript + build frontend + package with electron-builder |
| `npm run electron:build` | Build and package the Electron app without TypeScript check |
| `npm run lint` | Run ESLint across all TypeScript/TSX source files |
| `npm run test` | Run unit tests with Vitest |
| `npm run test:watch` | Run Vitest in watch mode |

---

## Project Structure

```
pesoplan/
├── electron/               # Electron main process
│   ├── main.ts             # App entry point, window management
│   ├── preload.ts          # Secure IPC bridge (contextBridge)
│   ├── database.ts         # SQLite setup and schema migrations
│   ├── auth.ts             # PIN hashing, login, recovery logic
│   └── data-handlers.ts    # IPC handlers for all CRUD operations
├── src/                    # React renderer process
│   ├── App.tsx             # Main app shell, auth flow, routing
│   ├── main.tsx            # React entry point
│   ├── components/
│   │   ├── auth/
│   │   │   ├── AccountSelectScreen.tsx  # Multi-account picker
│   │   │   ├── LockScreen.tsx           # PIN login + recovery flow
│   │   │   └── RegisterScreen.tsx       # Account registration
│   │   ├── layout/
│   │   │   └── Sidebar.tsx              # Collapsible navigation sidebar
│   │   ├── modules/
│   │   │   ├── Dashboard.tsx            # Overview + charts
│   │   │   ├── SetupModule.tsx          # Income, budgets, categories, year mgmt
│   │   │   ├── BillsModule.tsx          # Bills and loans tracker
│   │   │   ├── ExpensesModule.tsx       # Monthly expense logging
│   │   │   ├── CashflowModule.tsx       # Cashflow analysis
│   │   │   └── SavingsModule.tsx        # Savings goals
│   │   └── ui/
│   │       ├── CurrencyInput.tsx        # Comma-formatted peso input
│   │       └── Tooltip.tsx              # Portal-based hover tooltip
│   ├── store/
│   │   └── useAppStore.ts               # Zustand global state
│   ├── types/
│   │   └── index.ts                     # TypeScript interfaces
│   └── lib/
│       └── utils.ts                     # Utility functions
├── public/                 # Static assets (icons, etc.)
├── index.html              # Vite HTML entry
├── vite.config.ts          # Vite + Electron plugin config
├── tailwind.config.js      # Tailwind CSS config
├── tsconfig.json           # TypeScript config (renderer)
├── tsconfig.node.json      # TypeScript config (main process)
└── package.json
```

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+1` | Go to Dashboard |
| `Ctrl+2` | Go to Setup |
| `Ctrl+3` | Go to Bills |
| `Ctrl+4` | Go to Expenses |
| `Ctrl+5` | Go to Cashflow |
| `Ctrl+6` | Go to Savings |

---

## Building for Distribution

```bash
npm run electron:build
```

Output files will be in the `release/` directory:

| Platform | Output |
|---|---|
| Windows | `.exe` (NSIS installer) and `.msi` |
| macOS | `.dmg` |
| Linux | `.AppImage`, `.deb`, `.rpm` |

> Make sure icons exist at `public/icon.ico` (Windows), `public/icon.icns` (macOS), and `public/icon.png` (Linux) before building.

---

## Troubleshooting

**App doesn't launch / blank window**
- Make sure you ran `npm run electron:dev`, not just `npm run dev`
- Check the terminal for Electron main process errors

**`better-sqlite3` errors on startup**
- Run `npx electron-rebuild -f -w better-sqlite3` to recompile for the correct Electron version

**Lint errors in IDE (module not found)**
- These resolve after `npm install` — restart your IDE/TypeScript server if they persist

**Data is missing after restart**
- Data is stored in the OS user data directory:
  - Windows: `%APPDATA%\pesoplan\`
  - macOS: `~/Library/Application Support/pesoplan/`
  - Linux: `~/.config/pesoplan/`

---

## UI Highlights

- **Collapsible sidebar** — icon-only mode to maximise screen space
- **Color-coded tables** — each column (income, bills, expenses, savings, net, cumulative) has a distinct color in Dashboard and Cashflow
- **Alternating row stripes** and cell borders across all tables for easy scanning
- **Hover tooltips** on complex headers (Net, Cumulative, Flow, Rate, Status, Remaining, Progress, Pool Amount, Distribution) rendered via React Portal to avoid clipping
- **Expandable savings rows** — click any month in the Savings pool to see per-goal allocation
- **Separate Month / Year selectors** in Expenses for cross-year browsing
- **Formatted currency inputs** — all peso fields display with comma separators on blur

---

## Accessibility

PesoPlan targets **WCAG 2.1 AA** compliance:
- All interactive elements have ARIA labels
- Keyboard navigable with visible focus indicators
- Screen reader-friendly table structures and role attributes

---

## Author

**Charles Alexis Demetillo**
Full-Stack Developer · Portfolio Project

---

## License

Copyright © 2026 Charles Alexis Demetillo. All rights reserved.
Private / Unlicensed — for personal use and portfolio demonstration purposes only.
