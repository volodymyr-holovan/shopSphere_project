# ShopSphere: Marketplace Performance Analytics

A full-cycle analytics project on ShopSphere, a global marketplace selling seven product categories across five regions. The analysis covers marketing efficiency, category profitability, customer economics, and a live checkout A/B experiment, built on order, customer, product and campaign data from 2022-2024.

**Live dashboard (Tableau Public):** https://public.tableau.com/views/ShopSphereBusinessAnalysis/Dashboard1?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

**Author:** Volodymyr Holovan

## Repository contents

| File / folder | Description |
| :---- | :---- |
| `data/` | Five source CSV files (see Data below) |
| `Shopsphere_analysis.ipynb` | Full reproducible analysis in pandas: data validation, marketing ROI, customer LTV, category profitability, discount behavior, revenue concentration, and the A/B test with subgroup analysis |
| `reports/` | Full written reports — business context, all findings with supporting numbers and charts, and final recommendations |
| `README.md` | This file |

## Data

Five related tables, joined on `customer_id`, `product_id` and `order_id`.

| Table | Rows | Key columns |
| :---- | :---- | :---- |
| `shopsphere_customers.csv` | 3,000 | customer_id, region, country, age, gender, acquisition_channel, signup_date |
| `shopsphere_products.csv` | 250 | product_id, category, product_name, price, cost, margin_pct |
| `shopsphere_orders.csv` | ~12,300 | order_id, customer_id, order_date, device, channel, discount_pct, net_amount, ab_variant, is_returned |
| `shopsphere_order_items.csv` | ~26,000 | item_id, order_id, product_id, category, quantity, unit_price, line_total |
| `shopsphere_marketing.csv` | 216 | campaign_id, year, month, channel, budget, impressions, clicks, conversions, attributed_revenue |

Regions: North America, Europe, Southeast Asia, Latin America, Middle East.
Categories: Electronics, Clothing, Beauty, Home & Kitchen, Sports, Books, Toys.
Marketing channels: Organic, Paid Search, Social Ads, Influencer, Email, Referral.

The checkout A/B experiment (`ab_variant` = A/B) ran from June 1, 2024 onward; earlier orders are outside the experiment and have no variant assigned.

## Method

- **Data preparation:** SQL (joins, aggregation, subqueries) and pandas, cross-validated against each other.
- **Visualization and dashboard:** Tableau Public — six chart types (line, bar, dual-axis, bubble, KPI cards) assembled into one interactive dashboard with cross-filtering.
- **Statistical testing:** Welch's t-test and Mann-Whitney U test for the A/B experiment, with subgroup analysis to check for effect masking.

## Key findings

- Paid Search takes 46% of the marketing budget but returns the lowest ROI (1.3x) and the lowest customer LTV ($648) of any channel; Influencer and Referral outperform on long-term value while being comparatively underfunded.
- Electronics drives 57% of category revenue but carries the lowest margin (12%) and one of the highest return rates — a volume-illusion category. Beauty is the opposite: 55% margin on a small revenue base — a hidden diamond.
- The top 5% of customers generate 35% of total revenue; customers above the average spend level (29% of the base) generate 73%.
- The checkout redesign shows no significant effect in the overall average (p = 0.51), but a 22.8% lift in average order value for new customers specifically (p = 0.003) — masked by the larger, unaffected repeat-customer segment.

Full evidence, charts, and the complete recommendation set are in `ShopSphere Project.docx`.

## Reproducing the analysis

The notebook loads data directly from this repository, so it runs standalone:

```python
customers = pd.read_csv('https://raw.githubusercontent.com/volodymyr-holovan/shopSphere_project/main/data/shopsphere_customers.csv')
products = pd.read_csv('https://raw.githubusercontent.com/volodymyr-holovan/shopSphere_project/main/data/shopsphere_products.csv')
orders = pd.read_csv('https://raw.githubusercontent.com/volodymyr-holovan/shopSphere_project/main/data/shopsphere_orders.csv')
order_items = pd.read_csv('https://raw.githubusercontent.com/volodymyr-holovan/shopSphere_project/main/data/shopsphere_order_items.csv')
marketing = pd.read_csv('https://raw.githubusercontent.com/volodymyr-holovan/shopSphere_project/main/data/shopsphere_marketing.csv')
```

Requires `pandas`, `numpy`, `matplotlib`, and `scipy`.
