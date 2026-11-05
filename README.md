<div align="center">

# 🎨 Asian Paints – Sales & Dealer Performance Dashboard
### *Power BI Sales Analytics & Interactive Dashboard Project*

![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-Data%20Preparation-blue?style=for-the-badge)
![DAX](https://img.shields.io/badge/DAX-Measures-0839C3?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Sales%20Analytics-1F4FD8?style=for-the-badge)
![Project](https://img.shields.io/badge/Project-Executive%20Dashboard-011E4C?style=for-the-badge)

**Power BI · Asian Paints Sales Dashboard Project**

</div>

---

# 📋 Table of Contents

- [📌 Overview](#-overview)
- [🎯 Objective](#-objective)
- [📦 Data Model](#-data-model)
- [✨ Features](#-features)
- [🏗️ Repository Structure](#️-repository-structure)
- [🗂️ Report Pages](#️-report-pages)
  - [🏠 Page 1: Executive Overview](#-page-1-executive-overview)
  - [📍 Page 2: Regional Performance](#-page-2-regional-performance)
  - [🚀 Page 3: Product Analytics](#-page-3-product-analytics)
  - [🤝 Page 4: Dealer Performance](#-page-4-dealer-performance)
  - [📊 Page 5: Trends & Executive Summary](#-page-5-trends--executive-summary)
- [🔍 Filters & Slicers](#-filters--slicers)
- [💡 Key Insights](#-key-insights)
- [🎨 Theme & Formatting](#-theme--formatting)
- [🛠️ Tools & Techniques](#️-tools--techniques)
- [🚀 How to Use This Project](#-how-to-use-this-project)
- [🎥 Video Walkthrough](#-video-walkthrough)
- [📈 Learning Outcomes](#-learning-outcomes)
- [👤 Author](#-author)

---

# 📌 Overview

The **Asian Paints – Sales & Dealer Performance Dashboard** is a five-page Microsoft Power BI report that presents revenue, profit, target achievement, regional performance, product analytics, dealer performance and executive-level trends. The dashboard reports on **FY 2023-24 (01-Apr-2023 to 31-Mar-2024)** with **FY 2022-23** used as the previous-year comparison, and the screenshots are marked **Data as on: 31-Mar-2024**.

The five interactive report pages are:

- 🏠 **Executive Overview:** headline KPIs, monthly revenue trend, revenue vs target, category treemap, state map and executive scorecard.
- 📍 **Regional Performance:** region, zone, state and city analysis with maps, rankings, target comparison and a margin heatmap.
- 🚀 **Product Analytics:** top products by revenue and profit, category contribution, inventory turnover and a revenue-vs-profit scatter analysis.
- 🤝 **Dealer Performance:** top dealers, revenue-per-dealer distribution, visit trends, dealer types, satisfaction and dealer locations.
- 📊 **Trends & Executive Summary:** year-over-year trends, cumulative YTD revenue, a 6-month forecast, scorecard and an executive summary band.

The design uses an Asian Paints-inspired **navy and blue palette**, a dark left navigation sidebar, KPI cards with previous-year comparisons, interactive charts, India maps and insight panels.

> ⚠️ This dashboard is built for **practice / portfolio purposes** using a synthetic dataset. It is not an official report of Asian Paints.

---

# 🎯 Objective

The objective is to turn sales data into an accessible decision-support dashboard that helps users:

- Monitor total revenue, profit, profit margin and order volume against the previous year.
- Track revenue performance against targets month by month.
- Compare performance across regions, zones, states and cities.
- Identify top-performing products, categories and profit drivers.
- Evaluate inventory turnover across product categories.
- Analyze dealer revenue, dealer types and revenue-per-dealer distribution.
- Monitor dealer visits and orders booked over time.
- Understand year-over-year growth and the 6-month revenue outlook.
- Present consolidated business health indicators to leadership.
- Explore metrics through slicers, hierarchy switchers and detailed tables.

---

# 📦 Data Model

The report is built on a **star-schema model** with **5 dimension tables**, **4 fact tables** and a few helper tables. The dataset covers **01-Jan-2022 to 31-Dec-2025**.

### Source Tables

| Table | Type | Rows | Key fields |
|---|---|---:|---|
| `FactSales` | Fact | 300,000 | InvoiceID, DateKey, ProductID, DealerID, CustomerID, RegionID, Quantity, SalesAmount, DiscountAmount, NetSales, CostAmount, ProfitAmount |
| `FactTargets` | Fact | 30,000 | DateKey, RegionID, ProductID, RevenueTarget, ProfitTarget |
| `FactInventory` | Fact | 100,000 | InventoryID, DateKey, ProductID, RegionID, OpeningStock, ClosingStock |
| `FactDealerVisits` | Fact | 100,000 | VisitID, DateKey, DealerID, SalesRepID, VisitType, RevenueGenerated |
| `DimDate` | Dimension | 1,461 | DateKey, Date, Year, Quarter, Month, MonthName |
| `DimProduct` | Dimension | 500 | ProductID, ProductName, Category, SubCategory |
| `DimRegion` | Dimension | 150 | RegionID, Region, Zone, State, City |
| `DimDealer` | Dimension | 5,000 | DealerID, DealerName, DealerType, City, State |
| `DimCustomer` | Dimension | 100,000 | CustomerID, CustomerName, CustomerType |
| `MarketingCampaigns` | Standalone | 1,000 | CampaignID, CampaignName, Budget, Spend |
| `ProductCostMaster` | Standalone | 500 | ProductID, ManufacturingCost, PackagingCost, TransportationCost |

### Relationships (13 total, Many-to-One, Single direction)

| Dimension (1 side) | Fact table (many side) | Join column |
|---|---|---|
| `DimDate` | FactSales, FactTargets, FactInventory, FactDealerVisits | DateKey |
| `DimProduct` | FactSales, FactTargets, FactInventory | ProductID |
| `DimRegion` | FactSales, FactTargets, FactInventory | RegionID |
| `DimDealer` | FactSales, FactDealerVisits | DealerID |
| `DimCustomer` | FactSales | CustomerID |

`MarketingCampaigns` and `ProductCostMaster` are loaded but are **not connected** to the model and are not used in any visual.

### Helper Tables & Parameters

| Object | Purpose |
|---|---|
| `_Measures` | Dedicated table that holds all DAX measures |
| `RevBucket` | Revenue-per-dealer buckets (< 2, 2-3, 3-4, 4-5, 5-6, 6-7, > 7 Lakh) |
| `Scorecard` | Metric list for the Page 5 executive scorecard |
| `Compare` | "Previous Year" option for the Compare With slicer |
| `Hierarchy Level` | Field parameter: Region → Zone → State → City (Page 2) |
| `Product Hierarchy` | Field parameter: Category → SubCategory → ProductName (Page 3) |

### Key DAX Measures

| Group | Measures |
|---|---|
| Core | Revenue (NetSales), Profit, Profit Margin %, Total Orders, Total Dealers, Total Products, Revenue per Dealer |
| Previous Year | Revenue PY, Profit PY, Margin PY %, Orders PY, YoY % measures using `SAMEPERIODLASTYEAR` |
| Targets | Revenue Target, Profit Target, Target Achievement %, Profit Achievement %, Target Scale |
| Inventory & Visits | Inventory Turnover, Visits, Orders Booked |
| Ranking & Contribution | Region / State / Product / Dealer Rank, Region / Category / State Contribution % |
| Time Intelligence | Revenue YTD (fiscal year ending 31 March), Revenue YTD PY |
| Dynamic Text | Delta, "vs PY", executive summary and status text measures |
| Statistics | R Squared for the revenue-vs-profit scatter |

### Fiscal Calendar

`DimDate` is marked as the date table and extended with **MonthStart, Month Year, Month Short, Fiscal Year, FY Label and Fiscal Month No**, so months sort **Apr → Mar** on every trend chart.

### Data Notes

- **Revenue** is defined as `NetSales` (after discount); **Profit = NetSales − Cost**.
- The dataset contains **5 regions** (West, North, South, East, Central) and **7 product categories**.
- Products have **generic names** (Asian Paint Product 1 to 500).
- **Dealer Satisfaction** is **placeholder (sample) data** created only to complete the dashboard design; it is not part of the dataset.
- The dataset has **no targets** for Orders, Revenue per Dealer or Inventory Turnover, so these rows show "-" in the scorecard target column.
- `FactTargets` values are much larger than actual revenue. The dashboard uses a **Target Scale** measure so Target Achievement % is shown on a comparable basis.
- `FactTargets` has no dealer link and `FactDealerVisits` has no region link, so Dealer Type does not filter targets and the Visits chart does not respond to Region/State slicers.
- Maps shade at **state level**, because dealer city and state values in the dataset do not always match.
- The 6-month forecast uses Power BI's built-in forecasting model and is **not a guarantee**.

---

# ✨ Features

- 💰 Revenue, profit, margin, orders and target-achievement KPI cards with **previous-year comparisons**.
- 📈 Monthly revenue trend and revenue-vs-target combo chart.
- 🌳 Category treemap and donut chart with consistent category colors.
- 🗺️ India filled maps and dealer bubble map for geographic analysis.
- 🔎 Region, state, product and dealer **ranking tables** with data bars.
- 🔥 Profit-margin heatmap by state and region.
- 🧭 **Hierarchy Level switchers** (Region → Zone → State → City and Category → Sub Category → Product).
- 📦 Inventory turnover by category with an overall-average reference line.
- 🔵 Revenue vs profit **scatter analysis** with trend line and R² value.
- 🤝 Revenue-per-dealer distribution, dealer-type analysis and top-city insights.
- 📅 Dealer visits and orders-booked monthly trend.
- 🔮 **6-month revenue forecast** with 95% confidence band.
- 📊 Executive scorecard with Actual, Target, Achievement % and vs-PY columns.
- 💡 Dynamic insight text and executive summary that update with filters.
- 🎛️ Page-specific slicers, **synced Date Range slicer** and **Reset** buttons.
- 🧭 Five-page sidebar navigation with active-page highlight.

---

# 🏗️ Repository Structure

```text
📦 AsianPaints_Dashboard
│
├── 📊 AsianPaints_Dashboard.pbix          → Power BI report file
│
├── 🎨 AsianPaints_Theme.json              → Custom Power BI color theme
│
├── 📁 Dataset                             → Source dataset files (CSV)
│
├── 📁 Asset                               → Dashboard assets
│   ├── 📁 Icon                            → Icons used in the dashboard
│   └── 📁 Images                          → Dashboard screenshots
│       ├── Page-1.png                     → Executive Overview
│       ├── Page-2.png                     → Regional Performance
│       ├── Page-3.png                     → Product Analytics
│       ├── Page-4.png                     → Dealer Performance
│       └── Page-5.png                     → Trends & Executive Summary
│
└── 📝 README.md                           → Project documentation
```

---

# 🗂️ Report Pages

## 🏠 Page 1: Executive Overview

The **Executive Overview** page summarizes overall revenue, profit, margin, orders and target achievement, with category and geographic breakdowns.

### KPI Cards

| KPI | Value displayed | vs Previous Year |
|---|---:|---|
| 💰 Total Revenue | ₹ 176.68 Cr | ▲ 1.6% (PY ₹ 173.83 Cr) |
| 💵 Total Profit | ₹ 57.52 Cr | ▲ 2.0% (PY ₹ 56.41 Cr) |
| 📊 Profit Margin % | 32.55% | ▲ 0.1% (PY 32.45%) |
| 🛒 Total Orders | 75K | ▲ 1.3% (PY 74,538) |
| 🎯 Target Achievement % | 102.46% | ▲ 1.46% (PY 101.00%) |

### Visuals

| Visual | Analysis |
|---|---|
| Revenue Trend (Monthly) | Line chart of monthly revenue, Apr 23 to Mar 24 |
| Revenue vs Target (Monthly) | Columns for revenue and target with achievement % line |
| Revenue by Category (Treemap) | Revenue share of the 7 product categories |
| Revenue by Region (Map) | State-level filled map of India |
| Executive Scorecard (vs Target) | Revenue achievement, profit achievement and margin by category |

### Revenue by Category (₹ Cr)

| Category | Revenue (₹ Cr) |
|---|---:|
| Putty | 29.04 |
| Interior Emulsion | 28.71 |
| Industrial Coating | 25.55 |
| Wood Finish | 25.48 |
| Primer | 25.39 |
| Exterior Emulsion | 21.92 |
| Waterproofing | 20.59 |

### Executive Scorecard (vs Target)

| Category | Revenue Achievement % | Profit Achievement % | Margin % |
|---|---:|---:|---:|
| Exterior Emulsion | 104.77% | 108.91% | 32.61% |
| Waterproofing | 104.57% | 113.69% | 32.58% |
| Primer | 104.19% | 109.45% | 32.41% |
| Putty | 103.51% | 107.81% | 32.65% |
| Industrial Coating | 102.95% | 106.24% | 32.45% |
| Interior Emulsion | 99.68% | 103.69% | 32.58% |
| **Total** | **102.46%** | **106.79%** | **32.55%** |

### Key Insights displayed on the page

- Revenue is **₹ 176.68 Cr**, up **1.6%** compared with the previous year.
- Profit is **₹ 57.52 Cr** at a **32.55%** margin.
- Overall target achievement is **102.46%**.
- Monthly revenue stays in a narrow band of roughly **₹ 14.2 – 15.4 Cr**, peaking in **Dec 23 (15.37)**.
- **Interior Emulsion** is the only category below 100% revenue achievement (99.68%).

### 📸 Dashboard Screenshot

<img src="Asset/Images/Page-1.png" alt="Asian Paints Executive Overview Dashboard">

---

## 📍 Page 2: Regional Performance

The **Regional Performance** page analyzes performance across **Region > Zone > State > City**, using a Hierarchy Level switcher.

### KPI Cards

| KPI | Value displayed |
|---|---:|
| 💰 Total Revenue | ₹ 176.68 Cr |
| 💵 Total Profit | ₹ 57.52 Cr |
| 🎯 Target Achievement % | 102.46% |
| 🛒 Total Orders | 75K |
| 📊 Profit Margin % | 32.55% |

### Visuals

| Visual | Analysis |
|---|---|
| Revenue by Region (Filled Map) | State-level shading of revenue across India |
| Revenue by Region (Ranking) | Region rank, revenue and % contribution |
| Revenue by State (Top 10) | Top 10 states with data bars and contribution |
| Revenue vs Target by Region | Combo chart with a Revenue / Target / Achievement matrix below |
| Profit Margin % Heatmap by State and Region | Matrix with conditional color formatting |

### Revenue by Region

| Region | Revenue (₹ Cr) | Target (₹ Cr) | Achievement % |
|---|---:|---:|---:|
| West | 43.28 | 40.84 | ~105.97% |
| Central | 34.40 | 34.64 | 99.32% |
| East | 34.34 | 33.56 | 102.32% |
| North | 33.19 | 31.96 | 103.85% |
| South | 31.47 | 31.44 | 100.09% |
| **Total** | **176.68** | | |

*West achievement is calculated from the displayed revenue and target values.*

### Revenue by State (Top 10)

| Rank | State | Contribution |
|---:|---|---:|
| 1 | Mizoram | 8.0% |
| 2 | Himachal Pradesh | 7.3% |
| 3 | West Bengal | 6.1% |
| 4 | Rajasthan | 6.0% |
| 5 | Uttar Pradesh | 5.9% |
| 6 | Tamil Nadu | 5.5% |
| 7 | Odisha | 5.4% |
| 8 | Sikkim | 4.8% |
| 9 | Haryana | 4.7% |
| 10 | Bihar | 4.0% |

### Key Insights displayed on the page

- **West** is the top region with **₹ 43.28 Cr**, followed by Central and East.
- **Mizoram** leads all states, followed by **Himachal Pradesh** and **West Bengal**.
- West (≈105.97%), North (103.85%) and East (102.32%) are above target; **Central (99.32%)** is below target.
- Profit margin stays close to **32.4% – 32.7%** across all states and regions, so the heatmap shows very little variation (overall **32.57%**).

### 📸 Dashboard Screenshot

<img src="Asset/Images/Page-2.png" alt="Asian Paints Regional Performance Dashboard">

---

## 🚀 Page 3: Product Analytics

The **Product Analytics** page offers insights across **Product > Sub Category**, covering top products, category contribution, inventory turnover and the revenue-profit relationship.

### KPI Cards

| KPI | Value displayed | vs Previous Year |
|---|---:|---|
| 💰 Total Revenue | ₹ 176.68 Cr | ▲ 1.6% (PY ₹ 173.83 Cr) |
| 💵 Total Profit | ₹ 57.52 Cr | ▲ 2.0% (PY ₹ 56.41 Cr) |
| 📊 Profit Margin % | 32.55% | ▲ 0.1% (PY 32.45%) |
| 📦 Total Products | 500 | PY 500 |
| 🔁 Avg. Inventory Turnover | 1.87 | ▲ 0.03 (PY 1.84) |

### Visuals

| Visual | Analysis |
|---|---|
| Product Revenue (Top 10) | Horizontal bars of the top 10 products by revenue |
| Product Profit (Top 10) | Horizontal bars of the top 10 products by profit |
| Category Contribution (Revenue) | Donut chart with a category contribution table |
| Inventory Turnover by Category | Column chart with an overall-average reference line |
| Top Products by Revenue | Ranked table with product, category and revenue |
| Revenue vs Profit Scatter Analysis | Product-level scatter with trend line and R² |

### Category Contribution (Revenue)

| Category | Revenue (₹ Cr) | % Contribution |
|---|---:|---:|
| Putty | 29.04 | 16.4% |
| Industrial Coating | 25.55 | 14.5% |
| Wood Finish | 25.48 | 14.4% |
| Primer | 25.39 | 14.4% |
| Exterior Emulsion | 21.92 | 12.4% |
| Waterproofing | 20.59 | 11.7% |
| Interior Emulsion | 28.71 | ~16.3% |

*Interior Emulsion's share is calculated from its revenue; it is partly hidden in the screenshot.*

### Inventory Turnover by Category

| Category | Turnover (x) |
|---|---:|
| Waterproofing | 1.94 |
| Primer | 1.90 |
| Interior Emulsion | 1.88 |
| Putty | 1.88 |
| Exterior Emulsion | 1.85 |
| Industrial Coating | 1.85 |
| Wood Finish | 1.83 |

### Key Insights displayed on the page

- **Asian Paint Product 168** and **Product 470** lead on revenue (₹ 0.45 Cr each), followed by Products 229, 99 and 36 (₹ 0.44 Cr).
- **Product 229, 99 and 168** lead on profit (₹ 0.15 Cr each).
- **Putty** contributes the largest revenue share (16.4%) and **Waterproofing** the smallest displayed share (11.7%).
- **Waterproofing** has the highest inventory turnover (1.94) against an overall average of **1.87**.
- Revenue and profit are strongly related, with **R² ≈ 0.96**.
- The page insight bar advises focusing on high-margin products and improving low-margin, high-revenue products (general guidance, not derived from the data).

### 📸 Dashboard Screenshot

<img src="Asset/Images/Page-3.png" alt="Asian Paints Product Analytics Dashboard">

---

## 🤝 Page 4: Dealer Performance

The **Dealer Performance** page tracks and analyzes dealer performance across India, including revenue distribution, visits, dealer types and location.

### KPI Cards

| KPI | Value displayed | vs Previous Year |
|---|---:|---|
| 💰 Total Dealer Revenue | ₹ 176.68 Cr | ▲ 1.6% (PY ₹ 173.83 Cr) |
| 💵 Total Profit | ₹ 57.52 Cr | ▲ 2.0% (PY ₹ 56.41 Cr) |
| 🏪 Total Dealers | 5,000 | ▲ 0.0% (PY 5,000) |
| 🛒 Total Orders | 75K | ▲ 1.3% (PY 74,538) |
| 💼 Avg. Revenue per Dealer | ₹ 3.53 Lakh | ▲ 1.6% (PY ₹ 3.48 Lakh) |
| ⭐ Avg. Dealer Satisfaction | 4.19 / 5 | PY 4.19 *(placeholder data)* |

### Visuals

| Visual | Analysis |
|---|---|
| Top 10 Dealers by Revenue | Ranked dealer table with revenue data bars |
| Revenue per Dealer (Distribution) | Number of dealers in each revenue bucket |
| Dealer Visits Trend (Monthly) | Visits and orders booked, Apr 23 to Mar 24 |
| Dealer Satisfaction Rating | Donut and rating table *(placeholder data)* |
| Revenue by Dealer Type | Revenue, profit and margin by dealer type |
| Dealer Location Map | State-level bubble map with Top 5 cities |

### Revenue per Dealer (Distribution)

| Revenue per dealer (₹ Lakh) | Dealers |
|---|---:|
| < 2 | 288 |
| 2 – 3 | 1,322 |
| 3 – 4 | 1,862 |
| 4 – 5 | 1,091 |
| 5 – 6 | 353 |
| 6 – 7 | 75 |
| > 7 | 9 |

### Revenue by Dealer Type

| Dealer type | Revenue (₹ Cr) | Profit (₹ Cr) |
|---|---:|---:|
| Distributor | 36.43 | 11.83 |
| Retail | 36.09 | 11.72 |
| Silver | 34.94 | 11.38 |
| Premium | 34.87 | 11.37 |
| Gold | 34.34 | 11.22 |

### Top 5 Cities by Dealer Revenue

| City | Revenue (₹ Cr) |
|---|---:|
| Jamnagar | 1.23 |
| Ghaziabad | 1.10 |
| Aurangabad | 0.99 |
| Burhanpur | 0.99 |
| Gorakhpur | 0.89 |

### Key Insights displayed on the page

- The top dealer is **Murthy LLC, Madanapalle**, followed by **More Inc (Ozhukarai)** and **Pingle Inc (Ramagundam)**.
- Most dealers (**1,862**) fall in the **₹ 3 – 4 Lakh** revenue bucket; the buckets add up to **5,000** dealers.
- **Premium & Gold dealers contribute 39.2%** of total revenue.
- The top 3 cities by dealer revenue are **Jamnagar, Ghaziabad and Aurangabad**.
- Orders booked (about 6,050 – 6,500 per month) are higher than visits (about 2,000 – 2,200 per month) in this dataset.

> ⚠️ **Note:** Satisfaction ratings are sample (placeholder) values and are not part of the dataset.

### 📸 Dashboard Screenshot

<img src="Asset/Images/Page-4.png" alt="Asian Paints Dealer Performance Dashboard">

---

## 📊 Page 5: Trends & Executive Summary

The **Trends & Executive Summary** page tracks performance trends, growth and the future outlook, and consolidates the key business indicators for leadership.

### KPI Cards

| KPI | Value displayed | vs Previous Year |
|---|---:|---|
| 💰 YTD Revenue | ₹ 176.68 Cr | ▲ 1.6% (PY ₹ 173.83 Cr) |
| 💵 YTD Profit | ₹ 57.52 Cr | ▲ 2.0% (PY ₹ 56.41 Cr) |
| 📊 Profit Margin % | 32.55% | ▲ 0.1% (PY 32.45%) |
| 📈 YoY Revenue Growth | 0.02 (≈ 2%) | ▲ ₹ 2.85 Cr |
| 📈 YoY Profit Growth | 2.0% | ▲ ₹ 1.10 Cr |
| 🛒 Total Orders | 75K | ▲ 1.3% (PY 74,538) |

### Visuals

| Visual | Analysis |
|---|---|
| Revenue Trend (Monthly) | FY 2023-24 vs FY 2022-23 revenue by month |
| Profit Trend (Monthly) | FY 2023-24 vs FY 2022-23 profit by month |
| Executive Scorecard (vs Target) | Actual, Target, Achievement % and vs PY by metric |
| YTD Revenue (Cumulative) | Cumulative revenue, current vs previous fiscal year |
| Revenue Forecast (Next 6 Months) | Built-in forecast with a 95% confidence band |
| YoY Growth Trend (%) | Monthly revenue and profit growth |
| Executive Summary band | Four dynamic summary statements |

### Executive Scorecard (vs Target)

| Metric | Actual | Target | Ach % |
|---|---:|---:|---:|
| Revenue (₹ Cr) | 176.68 | 172.44 | 102.5% |
| Profit (₹ Cr) | 57.52 | 53.86 | 106.8% |
| Profit Margin (%) | 32.55% | 31.23% | 104.2% |
| Orders | 75,470 | - | - |
| Revenue / Dealer (₹ Lakh) | 3.53 | - | - |
| Inventory Turnover | 1.87 | - | - |
| Target Achievement % | 102.46% | 100.00% | 102.5% |

### Key Insights displayed on the page

- **Steady Revenue Growth:** YTD revenue is ₹ 176.68 Cr, up 1.6% compared with the previous year.
- **Healthy Profitability:** YTD profit is ₹ 57.52 Cr with a margin of 32.55%, showing improvement.
- **On Track with Targets:** target achievement stands at 102.46% across the organization.
- **Forecast Outlook:** a 6-month revenue forecast with a 95% confidence band is shown on the page.
- The status box reads **"Excellent Performance!"** because revenue is above target and up compared with the previous year.

### 📸 Dashboard Screenshot

<img src="Asset/Images/Page-5.png" alt="Asian Paints Trends and Executive Summary Dashboard">

---

# 🔍 Filters & Slicers

The report provides page-specific filters in the sidebar and header:

| Page | Available filters |
|---|---|
| Executive Overview | Date Range, Region, Product Category, Dealer Type, State, Zone |
| Regional Performance | Date Range, Hierarchy Level, Region, Zone, State, City |
| Product Analytics | Date Range, Hierarchy Level, Category, Sub Category, Product, Price Tier |
| Dealer Performance | Date Range, Region, Dealer Type, Zone, State, City |
| Trends & Executive Summary | Date Range, Compare With, Region, Zone, Category, Dealer Type |

- The **Date Range slicer is synced** across all five pages.
- A **Reset** button (bookmark-based) appears on pages 2 to 5 and restores the default filters.
- Charts and tables support cross-filtering within Power BI.

---

# 💡 Key Insights

Based on the dashboard screenshots dated **31-Mar-2024** (FY 2023-24):

- 💰 Total revenue is **₹ 176.68 Cr**, up **1.6%**, and total profit is **₹ 57.52 Cr**, up **2.0%**.
- 📊 Profit margin is **32.55%**, a small improvement of 0.1 percentage points over the previous year.
- 🎯 Overall target achievement is **102.46%**.
- 🛒 The business handled about **75K orders** across **5,000 dealers** and **500 products**.
- 🌍 **West** is the top region (₹ 43.28 Cr); **Central** is the only region below target (99.32%).
- 🗺️ **Mizoram** is the top state by revenue, followed by Himachal Pradesh and West Bengal.
- 🎨 **Putty** is the top category (₹ 29.04 Cr), while **Waterproofing** has the lowest revenue (₹ 20.59 Cr) but the highest inventory turnover (1.94).
- 🤝 **Premium & Gold** dealers contribute **39.2%** of total revenue; Distributor and Retail lead on revenue.
- 📈 Revenue and profit are highly correlated across products (**R² ≈ 0.96**).
- 🔮 The dashboard includes a 6-month revenue forecast with a 95% confidence band.

**Interpretation note:** Profit margin is nearly flat (about 32.5%) across regions, states, categories and dealer types, and monthly revenue is nearly flat (₹ 14.2 – 15.4 Cr), because the dataset itself has little variation. Dealer satisfaction is placeholder data and should not be treated as a real finding.

---

# 🎨 Theme & Formatting

### Design

- Asian Paints-inspired **navy, blue and white** color palette (custom `AsianPaints_Theme.json`).
- Dark navy left sidebar (`#011E4C`) with an active-page blue highlight (`#2C3EC5`).
- White rounded cards with subtle outlines on a light `#FAFBFE` canvas.
- Large KPI values, soft-colored icons and green/red previous-year indicators.
- Consistent category colors across treemap, donut and scorecard.
- Canvas size of **1536 × 1024 px** on every page.
- Segoe UI typography with consistent headings and data labels.
- Insight bars and summary panels for contextual storytelling.

### Dashboard Layout

- **Executive Overview:** KPI cards at the top; revenue trend and target charts in the middle; treemap, map and scorecard below.
- **Regional Performance:** KPI cards, then map, ranking and top-states table, with the target chart and heatmap below.
- **Product Analytics:** KPI cards, top-10 charts and category donut, then turnover, product table and scatter, with an insight bar.
- **Dealer Performance:** KPI cards, dealer table, distribution and visits trend, then satisfaction, dealer type and location map.
- **Trends & Executive Summary:** KPI cards, trend charts and scorecard, then cumulative, forecast and YoY charts, with the summary band.

---

# 🛠️ Tools & Techniques

| Tool / Feature | Purpose |
|---|---|
| Microsoft Power BI Desktop | Creating and viewing the report |
| Power Query Editor | Loading CSV files, setting data types and adding custom columns |
| Star-Schema Data Model | 13 Many-to-One relationships across 5 dimensions and 4 facts |
| DAX Measures | KPIs, YoY, targets, ranking, contribution, YTD and dynamic text |
| Field Parameters | Hierarchy Level switchers for region and product analysis |
| Time Intelligence | `SAMEPERIODLASTYEAR`, `TOTALYTD` with a March fiscal year-end |
| Card / KPI Visuals | Revenue, profit, margin, orders and target KPIs |
| Column, Bar & Line Charts | Trends, rankings and distributions |
| Combo Charts | Revenue vs target with achievement % |
| Treemap & Donut Charts | Category contribution |
| Scatter Chart | Revenue vs profit with trend line and R² |
| Filled & Bubble Maps | Geographic revenue analysis |
| Table / Matrix Visuals | Scorecards, rankings and the margin heatmap |
| Conditional Formatting | Data bars, status icons and color gradients |
| Slicers & Sync Slicers | Filtering and synchronized date range |
| Bookmarks & Buttons | Page navigation and Reset buttons |
| Analytics Pane | Constant line, trend line and 6-month forecast |

---

# 🚀 How to Use This Project

1. **Clone or download** the repository.
2. Open **`AsianPaints_Dashboard.pbix`** using **Microsoft Power BI Desktop**.
3. If a data-source error appears, select **Home → Transform data → Data source settings** and point each query to the matching file in the `Dataset` folder.
4. *(Optional)* Import `AsianPaints_Theme.json` from **View → Themes → Browse for themes**.
5. Select **Refresh** after configuring the data connections.
6. For map visuals, enable **File → Options → Security → Use Map and Filled map visuals**.
7. Use the left sidebar to navigate among the five report pages.
8. Set the **Date Range** slicer to `01-Apr-2023` – `31-Mar-2024` to match the screenshots.
9. Use the page slicers, Hierarchy Level switchers and chart interactions to explore the data.
10. Use **Reset** on pages 2 to 5 to restore the default filters.

**Note:** `FactSales.csv` is large (about 38 MB, 300,000 rows). If it is stored as a `.zip` in the repository, extract it before refreshing.

---

# 🎥 Video Walkthrough

A walkthrough can be added here demonstrating all five report pages, KPI cards, filters, charts and drill-down interactions.

🔗 **[Watch the video here](YOUR_VIDEO_LINK_HERE)**

---

# 📈 Learning Outcomes

- ✅ Design a professional, multi-page sales analytics dashboard in Power BI.
- ✅ Prepare and clean data using Power Query.
- ✅ Build a star-schema data model with correct relationships.
- ✅ Write DAX measures for KPIs, previous-year comparisons and targets.
- ✅ Use time intelligence with a custom fiscal-year calendar.
- ✅ Analyze regional, product and dealer performance.
- ✅ Use field parameters for dynamic hierarchy switching.
- ✅ Create maps, treemaps, scatter plots, heatmaps and combo charts.
- ✅ Apply conditional formatting, bookmarks and synced slicers.
- ✅ Build executive summaries and forecasts for data storytelling.

---

# 👤 Author

<div align="center">

# Priya Shihora

[![GitHub](https://img.shields.io/badge/GitHub-priya_shihora-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/priyashihora)

**Asian Paints – Sales & Dealer Performance Dashboard | Power BI Project**

*Power BI · Sales Analytics · Data Visualization*

---

### ⭐ Thank You For Visiting This Project ⭐

Made with ❤️ using Microsoft Power BI

</div>
