# Meridian Financial Health Dashboard — README

## What this is
A single-file, self-contained HTML dashboard (no build step, no backend) that visualizes a company's financial health across five views: **Overview, Income Statement, Balance Sheet, Cash Flow, and Forecast & Planning**. Currently populated with **illustrative sample data** for a fictional company ("Meridian Textiles"), reporting in ₹ Lakhs.

## Tech stack
- Plain HTML/CSS/JS — no framework, no bundler
- **Chart.js 4.4.0** (loaded via CDN) for all charts
- Google Fonts: **Space Grotesk** (display) and **JetBrains Mono** (numbers/labels)
- Dark theme, CSS custom properties (`:root` variables) drive all colors

## Tech stack
- Plain HTML/CSS/JS — no framework, no bundler
- **Chart.js 4.4.0** (loaded via CDN) for all charts
- Google Fonts: **Space Grotesk** (display) and **JetBrains Mono** (numbers/labels)
- Dark theme, CSS custom properties (`:root` variables) drive all colors

## Structure

**Layout:** `.sidebar` (nav) + `.main` (content), CSS grid, responsive breakpoint at 1100px (sidebar becomes horizontal, grids collapse to 1 column).

**Navigation:** 5 `.section` divs (`overview`, `income`, `balance`, `cashflow`, `forecast`), toggled by JS click handlers on `.nav-item` — only one `.section.active` shown at a time (near the end of the `<script>` block).

**Sections:**

| Section | Contents |
|---|---|
| Overview | Health score ring, 5 KPI cards, revenue/expense/profit chart, auto-generated insights, working capital, expense breakdown, cash runway |
| Income Statement | P&L trend bar chart, margin trend line chart (gross/EBITDA/net), latest-vs-prior table |
| Balance Sheet | Assets/liabilities/equity trend, asset composition donut, current ratio, debt-to-equity, snapshot table |
| Cash Flow | Stacked cash flow by activity + closing cash line, cumulative cash chart, statement table |
| Forecast & Planning | Linear-regression + seasonal-index revenue/profit forecast, cash runway projection, auto-generated planning notes, horizon selector (3/6/12 months) |

## Data model (all sample/synthetic)
Located near the top of the main `<script>` block:

- `months` — 12 trailing months (Jul–Jun)
- `revenue`, `cogs`, `opex`, `grossProfit`, `netProfit`, `ebitda` — derived income-statement series (revenue has built-in seasonality + random noise)
- `currentAssets`, `fixedAssets`, `currentLiab`, `longTermDebt`, `equity` — hardcoded balance sheet series
- `cfOperating`, `cfInvesting`, `cfFinancing`, `netCashFlow`, `cashBal` — cash flow series, `cashBal` accumulates from a starting balance of 62
- `expenseBreakdown` — latest-month expense split by category

## Key logic to know

- **Insights (Overview & Forecast):** rule-based, not AI — simple threshold checks (e.g., margin down → warn, current ratio < 1.5x → warn, COGS > 55% of revenue → warn) generate the `.insight` cards dynamically.
- **Forecast model** (`linreg`, `seasonalIndex`, `buildForecast`): ordinary least-squares linear regression on the 12-month trailing series, with a seasonal index applied to revenue only. Cash forecast projects net cash flow trend forward and accumulates. This is explicitly flagged in the UI footnote as directional-only, not a guarantee.
- **Formatting helpers:** `fmt()` (₹ + 1 decimal + "L"), `fmtPct()`, `round1()`.
- **Chart defaults:** a shared `baseOpts()` function standardizes tooltip/legend/grid styling across most charts.

## How to adapt this for real data
Replace the hardcoded arrays in the data block near the top of the `<script>` tag with values pulled from an accounting export (the footnote in the Forecast section mentions Tally, Zoho Books, or QuickBooks as intended sources). Everything downstream (KPIs, charts, tables, insights, forecast) recalculates automatically from those arrays — no other code needs to change as long as array lengths/order stay consistent with `months`.

## File
- `financial-health-dashboard.html` — open directly in any modern browser, no server required.
