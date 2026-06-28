## Business Problem Summary

The retail leadership team needed an executive-level dashboard to monitor performance across seven key areas: sales trends, regional performance, category profitability, customer segment behaviour, shipping performance, discount impact, and return patterns. The goal was not a collection of charts but a coherent visual story that enables leadership to identify risks and opportunities and take action — with all critical filters available interactively.

## Dataset Description

**File:** `data/dashboard_sales_data.xlsx`  
**Sheet:** `dashboard_sales_data`  
**Rows:** 4,200 orders  
**Period:** January 2024 – December 2025 (24 months)  
**Market:** India (North, South, East, West regions)

## Tableau Workbook Description

**File:** `tableau/executive_dashboard.twbx`  

The workbook contains:
- 1 datasource connection (Excel direct)
- 7 calculated fields
- 7 individual worksheet views
- 1 executive dashboard with 6 chart zones + KPI row + filter panel
- 2 dashboard action filters (Region → all views; Category → profitability views)

## Calculated Fields

Seven calculated fields are created in Tableau, plus three KPI helper fields.

| Field Name | Formula | Purpose |
|---|---|---|
| `Profit Margin` | `[Profit] / [Sales]` | Row-level profit margin per order. Used for filtering and order-level analysis. |
| `Profit Margin Label` | `SUM([Profit]) / SUM([Sales])` | Aggregated margin for labels and KPIs. Used in Regional Performance and the Profit Margin KPI card. |
| `Cost` | `[Sales] - [Profit]` | Reverse-derived cost of goods sold per order. |
| `Average Order Value` | `SUM([Sales]) / COUNTD([Order Id])` | Average sales value per distinct order. |
| `Return Rate` | `SUM([Return Flag]) / COUNT([Order Id])` | Proportion of orders flagged as returned. |
| `Shipping Delay Bucket` | `IF [Delivery Days] <= 1 THEN "Express (0-1d)" ELSEIF ... END` | Groups orders into delivery speed bands. |
| `Profit Flag` | `IF [Profit] < 0 THEN "Loss" ELSE "Profitable" END` | Binary flag for loss-making orders. Used for colour coding in Category Profitability. |
| `Discount Band` | `IF [Discount] = 0 THEN "No Discount" ELSEIF ... END` | Groups orders by discount tier. |
| `Total Sales (M)` | `SUM([Sales]) / 1000000` | Sales in millions for KPI display. |
| `Total Profit (M)` | `SUM([Profit]) / 1000000` | Profit in millions for KPI display. |

## Dashboard Components

- **Size**: 1920 × 1080 (fixed)
- **Title**: Executive Dashboard
- **Top row**: 4 KPI cards (Total Sales, Total Profit, Profit Margin, Return Rate)
- **Middle row**: Sales Trend, Regional Performance, Shipping Performance, Return Analysis
- **Bottom row**: Category Profitability, Customer Segment, Discount vs Profit
- **Right panel**: 5 filter controls (Region, Category, Customer Segment, Order Date, Ship Mode)

## Filters and Interactions

### Dashboard Filters (interactive, apply to all views)
- **Region** — Multi-select filter (North / South / East / West)
- **Category** — Multi-select filter (Furniture / Office Supplies / Technology)
- **Customer Segment** — Single-select filter (Consumer / Corporate / Home Office)
- **Date Range** — Continuous date slider (Jan 2024 – Dec 2025)
- **Ship Mode** — Multi-select filter (Same Day / First Class / Second Class / Standard Class)

## Key Business Insights

1. **South is the largest regional market** (₹64.7M sales, 1,210 orders), but East achieves the best margin (15.6%).
2. **Technology drives 84% of total profit** (18.2% margin) — the business is heavily concentrated in one category.
3. **Furniture's 6.9% margin** is critically thin; Tables and Bookcases generate negative profit at 25–30% discount.
4. **Discounts above 20% destroy value** — orders above 30% discount average a loss of ₹1,601 per transaction.
5. **Home Office is the most profitable segment** (₹11.6M profit, 15.5% margin).
6. **15.9% of orders (666) face delivery delays of 6+ days**, representing a customer satisfaction risk.
7. **Furniture return rate is 7.67%**, more than double Technology's 3.03%, compounding margin problems.
8. **Office Supplies is undermonetised** — highest order count (1,669) but lowest revenue (₹11.5M).
9. **31–35% discount band (64 orders)** is unprofitable.

## Dashboard Story Summary

The business is growing and profitable at the headline level (₹217M sales, 15.3% margin), but three structural issues require leadership attention:
1. **Technology concentration (84% of profit)** — a single-point strategic risk. Diversification is needed.
2. **Uncontrolled discounting** — a linear and destructive relationship between discount depth and profitability, with a hard-stop needed at 25%.
3. **Furniture returns and thin margins** — the second-largest category is chronically underperforming and has double the return rate of Technology.

The South region and Home Office segment are the business's strongest performing units. The East region's margin discipline should be benchmarked across all regions. Office Supplies represents the clearest growth opportunity, high purchase frequency but very low average order value.

## Assumptions

- **Currency**: All financial figures are in Indian Rupees (₹). KPI labels divide raw figures by 1,000,000 for readability (M).
- **Return rate**: Calculated as `SUM(return_flag) / COUNT(order_id)` so every order has equal weight regardless of value.
- **Profit margin at row level vs aggregated**: Profit Margin is intentionally kept as a row-level field for filtering and per-order analysis. A separate `Profit Margin Label` field aggregates SUM(Profit)/SUM(Sales) for use on aggregated visuals (bar labels, KPI cards).
- **Rajasthan in two regions**: A small number of orders show Rajasthan tagged under both North and West. This is treated as a source data inconsistency and left as-is rather than reassigned.
- **Date range**: Order dates run January 2024 to December 2025. The Sales Trend view shows continuous months across this 24-month window.

## Screenshots Included

| File | Contents |
|---|---|
| `screenshots/full_dashboard.png` | Complete executive dashboard with KPI row |
| `screenshots/sales_trend_view.png` | Monthly Sales vs Profit line chart + Profit Margin bar chart |
| `screenshots/regional_performance_view.png` | Three-panel regional analysis: Sales, Profit, Margin % |
| `screenshots/category_profitability_view.png` | Category and sub-category profit + margin + revenue mix |
| `screenshots/filter_interaction_view.png` | Dashboard with filters applied showing filtered views |