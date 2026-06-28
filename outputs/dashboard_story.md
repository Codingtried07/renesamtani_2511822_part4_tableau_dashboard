# Dashboard Story

## Executive Summary

The business sold ₹217M across 4,200 orders between January 2024 and December 2025, producing ₹33.3M in profit at a 15.3% margin. Performance is healthy on the surface, but the dashboard reveals an uneven picture underneath. Technology is carrying the business, Furniture is dragging it down, and a clear discount cliff at 20% is eroding margin on orders that should be profitable. Returns are concentrated in one category and shipping delays are concentrated in one ship mode. Most of the value from this dashboard is in pointing leadership at one or two specific levers rather than dozens of small ones.

## What is performing well

Technology is doing the work. Copiers, Accessories, Phones, and Machines each contribute around ₹6.5M to ₹7.3M in profit. The category as a whole accounts for 84% of profit while generating 71% of sales. Its return rate sits below average across every sub-category, with Copiers as the most reliable product line in the catalogue at 2.72% returns.

Regional performance is consistent. The four regions sit in a tight band between 15.1% and 15.6% margin, with no obvious weak point. South leads on volume at ₹64.7M but East has the best margin at 15.6%, which is worth understanding even if no region is in trouble.

Premium shipping is delivering on time. Same Day and First Class orders are completing in the Express and Fast buckets where they should be. The faster customers pay for the service, the more reliably they get it.

## What is underperforming

Furniture is the consistent problem on this dashboard. It shows up as a weak performer in three separate views, and the views reinforce each other rather than telling different stories.

In Category Profitability, Furniture sits at 6.9% margin overall. Bookcases and Tables show visible loss segments crossing the zero reference line, meaning a meaningful share of their orders lose money even before returns are factored in.

In Return Analysis, the three worst-performing sub-categories on returns are all Furniture. Bookcases returns at 8.42%, Furnishings around 8%, and Chairs around 7.5%. The overall return rate is 4.55%, so Furniture sub-categories are running at nearly double the company average.

In Discount vs Profit, the loss-making dots clustered below zero at high discounts are mostly Furniture (visible as the blue dots in the bottom-right quadrant of the scatter). Orders with discounts above 30% average a loss of ₹1,601 each, and Furniture is overrepresented in that segment.

The three problems compound. Low margin means there is no cushion for returns. High returns mean the already-thin margin gets eroded further. Aggressive discounting on the same products pushes individual orders into outright losses.

## What risks are visible

The Furniture pattern is the biggest risk, and it is a risk because it is structural rather than incidental. A 6.9% margin combined with a 7.67% return rate means the category is operating close to break-even on a sustained basis. A modest cost increase, a supplier issue, or a shift in customer behaviour could push it into loss territory.

The discount policy is the second risk. The 20% threshold is acting as a clear margin cliff in the scatter plot, and the data shows orders going through at 30% and 35% discount levels. Whether these are pricing errors, sales-driven concessions, or system defaults, they are producing losses that should not exist.

Standard Class shipping delays are a softer risk. They affect customer experience rather than profit directly, but if the Delayed bucket continues to grow as volume scales, it could feed back into returns and repeat-purchase rates.

## What opportunities are visible

The opportunities mirror the risks because the same underperforming areas are also the easiest to improve.

Fixing Furniture would move headline numbers visibly. The category is currently at 6.9% margin against a company average of 15.3%. Even closing half that gap would shift overall profitability meaningfully. Specific levers include reviewing supplier costs, removing the worst-performing sub-categories from the catalogue, and addressing whatever is causing the high return rates.

Capping discounts at 20% without approval would protect margin without affecting the bulk of orders. Most orders already sit below this threshold and remain profitable. The change would only constrain the ~200 orders per year that are currently producing losses.

The Home Office customer segment is a smaller opportunity but worth noting. It produces the highest margin of any segment at 15.51%. Understanding why and applying those patterns to Consumer and Corporate segments could produce incremental margin gains.

## Recommended business actions

1. Open a strategic review of the Furniture category covering pricing, sourcing, returns, and sub-category mix. The Bookcases line specifically should be on the table for discontinuation.

2. Change the discount approval policy. Discounts above 20% should require sign-off from a sales manager and discounts above 30% should require category-level approval.

3. Investigate Standard Class shipping delays. Identify whether the issue is regional, carrier-specific, or product-specific before committing to a fix.

4. Profile Home Office customer behaviour to identify what makes them more profitable, and test whether those patterns can be extended.

5. Replace month-to-month sales reporting with a 3-month rolling average to reduce the noise visible in the Sales Trend chart.

## Limitations of the dashboard

The dashboard is showing observed correlations, not causes. Furniture has a returns problem but the data does not explain why. The customer rating field is on every order but is not currently used in any view, and it might point at quality issues that drive returns. Adding a returns-vs-rating analysis would help.

The dataset is 24 months, which is enough to spot patterns but not enough to confirm seasonality. A third year of data would resolve whether the late-2025 sales peak is a trend or noise.

The Sales Trend chart does not separate sales by category or region, so it cannot show whether the upward drift is being driven by Technology specifically or by all categories equally. A version of this chart broken out by category would be a useful follow-up.

The Rajasthan state appears in both North and West in the source data. This was left as-is for accuracy but means region-level totals include a small amount of misclassified data.

## Suggested next analysis

1. **Furniture deep dive**: returns by reason code, returns vs customer rating, profit by product line within each sub-category, and supplier-level cost breakdown if available.

2. **Discount analysis at order level**: which products, which customers, and which sales channels are generating the high-discount loss orders. The campaign_channel field on each order is not currently used and may help identify whether specific channels (Email, Paid, Social) push high-discount behaviour.

3. **Shipping delay root cause**: split the Delayed bucket by region, state, and product category to find concentration points. Cross-reference with order volume per day to see whether delays correlate with peak volume periods.

4. **Customer retention**: the dataset has customer_id for every order but no view currently tracks repeat customers. Building a repeat-purchase view would add a customer-lifetime-value dimension to the existing margin-focused analysis.
