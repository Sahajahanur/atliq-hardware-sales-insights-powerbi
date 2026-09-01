# 📊 AtliQ Hardware — Sales Insights & Profitability

> An end-to-end Power BI business analytics case study — from a raw MySQL sales database to a decision-ready, three-page profitability dashboard.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-217346?style=flat&logo=microsoft&logoColor=white)
![Power Query](https://img.shields.io/badge/Power%20Query-E97627?style=flat)

> **Portfolio case study:** This project is based on the publicly available AtliQ Hardware training dataset and uses a self-directed, simulated stakeholder-feedback process. It is not an actual client engagement.

🔗 [**Live Power BI Report**](https://app.powerbi.com/groups/me/reports/70265cf2-d9f4-40a3-94f6-d4222fdf3f48/1123c8c68e757c8c94ee?experience=power-bi)
📄 [**Full Project Report (PDF)**](./AtliQ_Hardware_Sales_Insights_Project_Report.pdf)
💼 [**LinkedIn**](https://www.linkedin.com/in/sahajahanur-laskar/)
📧 [**Email**](mailto:connectingsrl@gmail.com)

---

## 📌 Project Snapshot

| Metric | Value | Notes |
|---|---|---|
| **Total Revenue** | ₹985M | 2017–2020, all markets |
| **Sales Quantity** | ~2M units | 2017–2020 |
| **Total Profit** | ₹24.7M | Aggregate, 2017–2020 |
| **Transactions** | ~150K | `sales_transactions` records |
| **2020 Revenue** | ₹142.22M | SQL-validated |
| **2019 Revenue** | ₹336.02M | SQL-validated |

---

## 🧩 The Business Problem

AtliQ Hardware supplies computer hardware and peripherals to retail clients across India. The Sales Director relied on **verbal, inconsistent updates** from Regional Managers and **dozens of disconnected Excel files** to gauge performance — a process that was slow, prone to an optimistic ("rosy") bias, and made it nearly impossible to spot declining markets or underperforming products in time to act.

**Business questions this project set out to answer:**
- How is revenue trending over time (2017–2020)?
- Which markets generate the most **revenue** — and which generate the most **profit**?
- Which customers contribute most to revenue, and which have weak margins?
- How does revenue split between Brick-and-Mortar and E-Commerce?
- Where are underperforming markets/customers relative to a profit target?
- Is revenue concentration in a small customer base creating risk?

---

## 🛠️ Tech Stack & Workflow

```
MySQL (source) → SQL validation → Power Query (ETL) → Star Schema → DAX measures → Power BI dashboard → Power BI Service
```

| Layer | Tools / Techniques |
|---|---|
| **Database** | MySQL, MySQL Workbench |
| **SQL** | `SELECT`, `JOIN`, `GROUP BY`, aggregation, filtering — exploratory analysis & validation |
| **ETL** | Power Query (M) — filtering, conditional columns, currency normalization, data-type correction |
| **Data Modeling** | Star schema (1 fact table, 4 dimension tables) |
| **BI Platform** | Power BI Desktop |
| **Calculations** | DAX — `SUM`, `DIVIDE`, `ALL`, iterative contribution measures |
| **Deployment** | Power BI Service (cloud publishing, scheduled refresh) |
| **UX** | Slicers, bookmarks, conditional formatting, comments, mobile layout |
| **Method** | AIMS Grid project scoping; stakeholder-feedback-driven iteration |

---

## 🗂️ Project Workflow

### 1. Project Scoping — AIMS Grid
Before any technical work, the project was scoped using the **AIMS Grid** (Purpose, Stakeholders, End Result, Success Criteria). Success criteria (10% reduction in manual reporting effort, 5% improvement in decision speed) were defined as **targets**, not independently measured outcomes.

### 2. Data Discovery & SQL Validation
Connected to a MySQL database (`sales_transactions`, `customers`, `products`, `markets`, `date` — ~150,000 transaction records) owned by the engineering team. Ran exploratory SQL (row counts, market filters, year-over-year revenue aggregation) to validate figures **before** anything touched Power BI — confirming ₹142.22M (2020) vs. ₹336.02M (2019) revenue as an early, SQL-verified signal of decline.

### 3. Data Cleaning & ETL (Power Query)
| Issue | Resolution |
|---|---|
| Non-India markets (New York, Paris) with blank zone | Filtered out rows where zone was blank |
| Negative/zero `sales_amount` | Filtered out; flagged to engineering |
| Mixed currencies (INR / USD) | Added a Normalized Sales Amount column (USD → INR) |
| Duplicate currency labels (`INR` vs `INR\r`) | Standardized and de-duplicated |
| Normalized column loaded as Text | Corrected to Decimal Number |
| Blank `product_code` in some records | Documented as a known source-system limitation |

### 4. Data Modeling — Star Schema
```
        Customers        Products
              \            /
               \          /
            Sales Transactions (Fact)
               /          \
              /            \
         Markets          Date
```
4 dimension tables, 1 fact table, single-direction filter flow — simple, fast, and easy to reason about.

### 5. DAX Measures
| Measure | Purpose |
|---|---|
| `Revenue` | `SUM` of Normalized Sales Amount |
| `Sales Quantity` | `SUM` of sales_qty |
| `Profit Margin` | Total profit generated |
| `Profit Margin %` | `DIVIDE(Profit Margin, Revenue, 0)` |
| `Revenue Contribution %` | Current Revenue ÷ Revenue across `ALL()` markets/customers/products |
| `Profit Margin Contribution %` | Current Profit ÷ Profit across `ALL()` markets/customers/products |

### 6. Dashboard — 3 Pages

**Page 1 — Sales Overview**
KPI cards, Revenue/Sales Qty by Market, Top 5 Customers, Top 5 Products, Brick-and-Mortar vs. E-Commerce split, Revenue Trend, Year/Month slicers.

**Page 2 — Profitability & Customer Performance** *(added after stakeholder feedback)*
Revenue Contribution %, Profit Contribution %, Profit % by Market, and a full Customer Performance Summary table.

**Page 3 — Consolidated Performance**
Revenue Contribution % with a configurable **Profit Target** parameter, Revenue Trend overlay (current vs. prior year vs. margin %), and a sortable customer-level profitability matrix.

### 7. Stakeholder Feedback & Iteration
Simulated stakeholder review drove real changes: a Revenue-measure bug fix, the entire profitability layer (Page 2), the customer performance matrix, the channel-mix chart, and the Profit Target exception-flagging feature. Data gaps (order-count granularity, longer history) were logged rather than faked.

---

## 🖼️ Dashboard Screenshots

**Page 1 — Sales Overview**
![Sales Overview](./screenshots/key_insights.png)

**Page 2 — Profitability & Customer Performance**
![Profitability & Customer Performance](./screenshots/Profit_Analysis.png)

**Page 3 — Consolidated Performance**
![Consolidated Performance](./screenshots/Performance_Insights.png)

---

## 💡 Key Business Insights

- **Revenue ≠ Profit:** Delhi NCR drives 52.8% of revenue but only 48.5% of profit contribution, at a thin 2.3% margin.
- **Mumbai over-indexes on profitability** relative to its 15.2% revenue share — worth studying as a reference market.
- **Bengaluru shows a −20.8% profit margin** — a clear candidate for pricing/discount review.
- **Customer concentration risk:** Electricalsara Stores alone drives ~42–46% of revenue at a thin margin.
- **Multi-year revenue decline** (2017 → 2020) confirms the original business concern.
- **Channel mix** remains ~80% Brick-and-Mortar vs. ~20% E-Commerce (2020).

*(Insight language is deliberately investigative — "warrants further review" — rather than asserting root causes the dashboard alone can't prove. Full reasoning in the [project report](./AtliQ_Hardware_Sales_Insights_Project_Report.pdf).)*

---

## ⚠️ Limitations & Assumptions

- ~4 years of transaction history (2017–2020); no order-count field, only line-level quantity.
- USD→INR conversion uses a single fixed rate (a production build would use an effective-date FX table).
- Dashboard reflects **scheduled refresh**, not real-time streaming data.
- Stakeholder feedback cycle was a **self-directed, simulated exercise**, not a live client engagement.
- AIMS Grid success criteria are **stated targets**, not independently measured results.

*(Full detail in the [project report](./AtliQ_Hardware_Sales_Insights_Project_Report.pdf), Section 13.)*

---

## 📁 Repository Contents

```
atliq-hardware-sales-insights-powerbi/
│
├── README.md
├── sales_insight.pbix
├── AtliQ_Hardware_Sales_Insights_Project_Report.pdf
└── screenshots/
    ├── key_insights.png
    ├── Performance_Insights.png
    └── Profit_Analysis.png
```

---

## 👤 Author

**Sahajahanur Rahman Laskar 

📧 [connectingsrl@gmail.com](mailto:connectingsrl@gmail.com) &nbsp;·&nbsp; 💼 [LinkedIn](https://www.linkedin.com/in/sahajahanur-laskar/) &nbsp;·&nbsp; 💻 [GitHub](https://github.com/Sahajahanur)
