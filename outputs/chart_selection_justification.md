# Chart Selection Justification

For each chart on the dashboard, this document explains what business question it answers, why the chart type is appropriate, what fields drive the encodings (colour, size, label, filter), what design principle was applied, and what mistake was avoided.

---

## 1. KPI Cards (Total Sales, Total Profit, Profit Margin, Return Rate)

**Question answered**: What is the headline performance of the business at a glance?

**Why this chart type**: Big-number cards are the standard for executive dashboards because they communicate the answer in under a second. No axis to read, no comparison to make, just a number a leader can quote in a meeting.

**Encodings**: Each card uses Text only. Label text is grey 14pt regular, value text is black 32pt bold. The hierarchy is intentional: label gives context, value carries the weight.

**Design principle applied**: Visual hierarchy and economy. The cards take up about 12% of dashboard height but carry the four most important numbers on the page.

**Mistake avoided**: Showing too many KPIs. The temptation is to add Average Order Value, total customers, total orders, etc. Four is the right number for a top strip. More than that and they become wallpaper.

---

## 2. Sales Trend (dual-axis line chart)

**Question answered**: How are sales and profit changing over time, and do they move together?

**Why this chart type**: Line charts are the only sensible choice for continuous time-series data. The dual axis lets sales and profit share the same time dimension without forcing them onto a common Y scale, which would have squashed Profit (max ~₹1.8M) under Sales (max ~₹11M).

**Encodings**: 
- X axis: Month of Order Date (continuous)
- Left Y axis: SUM(Sales)
- Right Y axis: SUM(Profit), unsynchronised
- Colour: Sales blue, Profit green (consistent with the rest of the dashboard)

**Design principle applied**: Two related measures, one shared time axis. The unsynchronised axes are a deliberate choice to let both lines occupy the visual space rather than one being a thin sliver at the bottom.

**Mistake avoided**: Stacking sales and profit as bars, or showing them on a shared axis. Either would have made profit invisible because of the scale difference.

---

## 3. Regional Performance (horizontal bar chart with margin labels)

**Question answered**: Which regions generate the most sales, and is profitability consistent across regions?

**Why this chart type**: Horizontal bars are best for category-to-numeric comparisons where category names are longer than they are tall. The margin label on each bar adds a second dimension (profit efficiency) without needing a second chart.

**Encodings**:
- Rows: Region, State (state shown for drill-down on click)
- Columns: SUM(Sales) as bar length
- Label: `Profit Margin Label` field showing aggregated margin per region

**Design principle applied**: Sorted by value (descending) so the highest performer is at the top, which is where the eye lands first. Adding the margin label means one chart answers two questions instead of needing two charts.

**Mistake avoided**: Using a map for regions. Maps look impressive but communicate less than bars when there are only four categories and the question is "which is biggest". A pie chart for the same data would also have been worse because pie charts fail at precise comparison.

A second mistake avoided: using the raw `Profit Margin` field as the label, which gave a row-level aggregation error and showed nonsensical percentages. The fix was creating `Profit Margin Label = SUM(Profit)/SUM(Sales)` and using that for aggregated labels.

---

## 4. Category Profitability (horizontal bar chart with loss/profit colouring)

**Question answered**: Which sub-categories make money and which lose money?

**Why this chart type**: Horizontal bars sorted by profit show the ranking at a glance. The Profit Flag colour encoding makes loss-making segments instantly visible without needing the reader to interpret negative numbers from an axis.

**Encodings**:
- Rows: Category (outer group), Sub Category (inner)
- Columns: SUM(Profit)
- Colour: Profit Flag (Loss = red, Profitable = blue)
- Reference line: Constant = 0, red dashed, entire-table scope

**Design principle applied**: Pre-attentive colour processing. Red catches the eye before the brain reads numbers, so the reader sees "Furniture has losses" before they read "Tables: -₹150K". The category grouping in the row header adds hierarchy without cluttering the bars.

**Mistake avoided**: Showing absolute profit without context. The zero reference line is the entire point of the chart, because without it, a bar showing ₹100K profit and a bar showing -₹100K loss would look symmetrical. The dashed line forces the eye to register the direction.

---

## 5. Customer Segment (side-by-side bar chart)

**Question answered**: Do customer segments differ on volume vs profitability?

**Why this chart type**: Bars are the right answer for comparing three categorical groups across two measures. Stacking the Sales and Profit charts vertically lets the reader scan one above the other rather than interpreting a dual-axis chart for only three data points.

**Encodings**:
- Columns: Customer Segment
- Rows: SUM(Sales) (top), SUM(Profit) (bottom)
- Colour: Measure Names (Sales blue, Profit green)
- Label: Profit Margin percentage on each Profit bar

**Design principle applied**: Same colour mapping as the Sales Trend (Sales blue, Profit green) maintains consistency across the dashboard. The margin labels add the answer to the implicit question "which segment is most efficient" without needing a third row.

**Mistake avoided**: A single stacked bar showing Sales and Profit on the same axis. That would have crushed Profit (which is ~15% of Sales) into an invisible strip and made segment-to-segment comparison difficult.

---

## 6. Shipping Performance (stacked vertical bar chart)

**Question answered**: How are orders distributed across delivery speeds, and which ship modes are responsible for delays?

**Why this chart type**: Stacked bars show both the total per bucket and the composition within each bucket. The reader gets two answers at once: where most orders sit (Standard 4-5 days), and what causes the slow ones (almost all Standard Class).

**Encodings**:
- Columns: Shipping Delay Bucket (manually sorted: Express → Fast → Standard → Delayed)
- Rows: Distinct count of Order Id
- Colour: Ship Mode

**Design principle applied**: Logical ordering on the X axis. The buckets are sorted by speed rather than alphabetically. Alphabetical would have shown Delayed → Express → Fast → Standard, which is meaningless. Sorting by speed lets the reader follow the bars left-to-right as a story (fastest to slowest).

**Mistake avoided**: Using a pie chart for ship mode breakdown. Pie charts make it impossible to compare the same slice across different totals (e.g. "is Standard Class proportionally bigger in Delayed than in Express?"). Stacked bars answer that comparison naturally.

---

## 7. Discount vs Profit (scatter plot with reference lines)

**Question answered**: Is there a relationship between discount level and profitability, and where does it break down?

**Why this chart type**: Scatter is the only chart that shows individual-order-level data without aggregation. The pattern of dots reveals what an aggregated view would hide: clusters of high-profit orders at low discounts, and clusters of loss-making orders at high discounts.

**Encodings**:
- Columns: Discount
- Rows: Profit
- Mark: Circle
- Colour: Category (Furniture, Office Supplies, Technology)
- Size: SUM(Sales) (larger bubble = higher-value order)
- Reference line 1: Constant = 0 on profit axis, orange dashed (break-even line)
- Reference line 2: Constant = 0.2 on discount axis, orange dotted, labelled "20% Discount Threshold"

**Design principle applied**: Letting the data speak. The 20% reference line was added because the scatter shape suggested it visually, the loss-making dots cluster to its right. Annotating that threshold makes the insight explicit. The category colour reveals that Furniture (blue dots) is overrepresented in the loss zone.

**Mistake avoided**: Aggregating discount and profit into category averages. That would have shown three dots instead of 4,200, hiding the entire point of the chart. The Order Id was put on Detail to disaggregate at the order level (an early version of the chart had only three dots because of default aggregation fixed by adding Order Id to Detail).

---

## 8. Return Analysis (horizontal bar chart with diverging colour)

**Question answered**: Which categories and sub-categories have the highest return rates?

**Why this chart type**: Horizontal bars sort cleanly by return rate. The diverging colour palette (green to red through orange) reinforces the visual ranking and makes the worst performers leap out without requiring the reader to scan numbers.

**Encodings**:
- Rows: Category, Sub Category
- Columns: Return Rate calculated field
- Colour: Return Rate (green-orange-red diverging)
- Reference line: Average of Return Rate (orange dashed, entire-table scope)

**Design principle applied**: Encoding redundancy. Both bar length and colour communicate the same information (higher return rate). This is intentional because the colour is faster to process at a glance, while the bar length lets the reader read off precise values. Both encodings reinforce the same hierarchy.

**Mistake avoided**: Using a single solid colour for all bars. That would have forced the reader to compare bar lengths to identify the worst performers. The diverging palette lets them spot the red bars instantly. The average reference line provides a benchmark, so a bar's distance from the line tells the reader how far above or below typical it is.

---

## Cross-chart design principles

**Consistent colour mapping**: Sales is blue, Profit is green, Loss is red, Profitable is blue, throughout the dashboard. Once the reader learns the colour vocabulary on one chart, every other chart uses the same vocabulary.

**Minimal labels**: Labels are added only where they answer the chart's primary question. Bar charts get value labels, scatter and shipping charts do not. Tooltips fill in the rest.

**Reference lines for context**: The two scatter reference lines, the category bar zero line, and the return analysis average line all serve the same purpose: giving the reader a benchmark so individual values mean something.

**Avoided across the board**: 3D effects, pie charts, gauges, decorative chart types, and dual-axis charts where the dual axis was not needed. The Sales Trend dual axis is the only one on the dashboard and it is justified by the scale difference between sales and profit.
