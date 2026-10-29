# BigBasket — Customer Purchase & Market Basket Analysis

A retail customer-behavior and market basket analysis of BigBasket (online grocery) transaction data. The project turns raw transaction records into actionable insights on purchasing behavior, popular categories, product affinities, and customer segments.

**Analysis window:** March 2011 – December 2014
**Scope:** 62,141 transaction records · 106 customers · 8,387 orders · 1,732 SKUs · 216 product categories

---

## 1. Business Objective

Analyze customer transactions and product purchasing patterns to improve retail sales, customer engagement, and cross-selling. Specifically:

- Identify the most frequently purchased product categories
- Measure order volume, purchasing frequency, and repeat-order behavior
- Discover products frequently purchased together (cross-sell / bundling opportunities)
- Analyze purchasing trends over time
- Segment customers by purchasing activity
- Provide data-driven recommendations for marketing and inventory

---

## 2. Datasets

| File | Role | Records | Notes |
|------|------|---------|-------|
| `bigBasket.csv` | Primary transaction dataset | 62,141 | Columns: Member, Order, SKU, Created On, Description |
| `IMB575-XLS-ENG.csv` | Supporting customer–category dataset | 62,141 | Member + Description; 56,182 duplicate rows are repeat purchases, not errors |
| `Memberdata.csv` | Single-customer case study (M09736) | 626 | 62 orders, 127 SKUs — not representative of the full base |

**Columns (primary dataset):**
- `Member` — unique customer identifier
- `Order` — order identifier
- `SKU` — product (stock-keeping unit) identifier
- `Created On` — transaction date/time
- `Description` — product category / description

---

## 3. Data Cleaning & Preparation

- **Mixed date formats standardized** — `Created On` contained two representations: 36,569 Excel serial numbers (e.g. `41904.94792`) and 25,572 string dates (e.g. `09/04/14 22:56`). All were parsed to proper datetimes with **zero unparseable values**.
- **Validated identifiers** — Member, Order, and SKU counts confirmed (106 / 8,387 / 1,732).
- **Data quality** — zero missing values, zero exact duplicate rows in the primary dataset.
- **Date range** — corrected to March 2011 – December 2014 (41 months of activity).

---

## 4. Key KPIs

| KPI | Value |
|-----|-------|
| Total Orders | 8,387 |
| Total Customers | 106 |
| Total SKUs | 1,732 |
| Total Product Categories | 216 |
| Total Product Records | 62,141 |
| Items per Order | 7.41 |
| Orders per Customer | 79.12 |
| Repeat Customer Rate | 100% |

> **Not computable:** Revenue, average order value (AOV), gross margin, and customer lifetime value (CLV) — the dataset has no price, quantity, or cost fields. This is a **descriptive** behavioral analysis.

---

## 5. Methodology

1. **Data cleaning** (Python / pandas) — date standardization, validation, duplicate investigation.
2. **KPI computation** — order volume, basket size, frequency, repeat-rate.
3. **Category analysis** — frequency ranking and Pareto (cumulative share).
4. **Market basket analysis** — order-level category baskets; support, confidence, and lift for co-occurring pairs (minimum 2% support).
5. **Customer segmentation** — behavioral RFB model (Recency, Frequency, category Breadth), quartile-scored into four segments.
6. **Time-series trends** — monthly and yearly order patterns.

---

## 6. Headline Findings

- **100% repeat customers** — every one of the 106 customers placed multiple orders (avg 79, min 24): a loyal recurring-order base.
- **Fresh produce dominates** — Other Vegetables, Beans, and Root Vegetables are the top 3 categories; the top 10 account for **52.8%** of all records.
- **Strongest affinities (by lift):** Almonds ↔ Cashews (5.63), Snacky Nuts → Namkeen (3.67), and a tightly clustered dal family (Urad / Toor / Moong / Other Dals).
- **Highest-volume pairs:** vegetable staples (Beans, Other Vegetables, Root Vegetables) co-occur in ~24% of orders.
- **Trend:** sharp growth from 1,291 orders (2012) to 3,802 (2013), holding at 3,274 (2014).

### Customer Segments (RFB)

| Segment | Customers | Profile |
|---------|-----------|---------|
| Champions | 32 | Frequent + recent (avg 107 orders, 3.6 days recency) — reward & retain |
| At-Risk Loyal | 21 | Frequent historically, ~52 days lapsed — **highest win-back priority** |
| Promising | 21 | Recent but lower frequency — nurture to grow |
| Needs Attention | 32 | Low frequency + lapsing — re-engage |

---

## 7. Recommendations

1. **Cross-sell bundles** — nuts (Almonds + Cashews), dal combos (Urad + Toor + Moong), snack pairings (Snacky Nuts + Namkeen).
2. **Win-back campaign** — target the 21 At-Risk Loyal customers.
3. **Inventory focus** — prioritize always-on stock and forecasting for fresh produce (anchors ~24% of baskets).
4. **Grow Promising segment** — frequency incentives and reorder nudges.
5. **Collect price & quantity data** — unlock revenue, AOV, margin, and CLV analysis.

---

## 8. Deliverables

| File | Description |
|------|-------------|
| `BigBasket_Clean_Transactions.csv` | Cleaned transaction dataset (62,141 rows), standardized dates, sorted by customer/order |
| `BigBasket_Retail_Analytics.xlsx` | 8-sheet analytics workbook for Power BI / Tableau: KPIs · Categories · Monthly Trend · Yearly Trend · Customers · Segments · MBA Rules · Top Pairs |
| `BigBasket_Analysis.pptx` | 10-slide insights & recommendations deck |
| `charts/` | Chart images (top categories, monthly trend, yearly orders, segments, top pairs, Pareto) |
| `README.md` | This document |

---

## 9. Constraints & Limitations

- **No pricing/quantity data** — revenue, profit, and margin cannot be measured.
- **Limited coverage** — only 106 customers; results may not generalize to the full business.
- **Retention is a proxy** — repeat-order behavior approximates retention; it is not a cohort-based retention rate.
- **Associations are descriptive** — co-occurrence shows correlation, not causation.
- **Generalization** — historical patterns (2011–2014) may not reflect current preferences.

---

## 10. Tools & Reproducibility

- **Python** — pandas, NumPy (cleaning, KPIs, segmentation, market basket)
- **matplotlib** — chart generation
- **xlsxwriter** — Excel workbook
- **pptxgenjs** — PowerPoint deck

**Workflow:** clean & validate → compute KPIs → category & trend analysis → market basket (support/confidence/lift) → customer segmentation → export workbook + deck.
