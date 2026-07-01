# anushkaadubey_2511015_part4_tableau_dashboard# Executive Retail Performance Analytics Dashboard

## 1. Business Problem Summary
This analytics project addresses the strategic challenge of balancing top-line revenue growth with bottom-line net profit margin preservation within a multi-channel retail environment. While raw transaction metrics indicate healthy sales development, macro-level aggregates frequently hide systemic operational friction. 

This dashboard isolates and investigates four critical profit leaks:
* **Margin Compression:** Deconstruction of structural margin damage caused by aggressive seasonal promotional discounts.
* **Product Line Drag:** Financial and inventory disparities between high-margin Technology categories and low-margin, capital-intensive Furniture lines.
* **Logistics Friction:** Operational bottlenecks across delivery modes and their impact on net customer satisfaction.
* **Reverse Logistics Overhead:** Elevated product return patterns that generate unnecessary overhead costs.

## 2. Dataset Description
The underlying dataset (`dashboard_sales_data.xlsx`) consists of 4,200 transactional records documenting retail operations across multiple fulfillment hubs. 

The data architecture is structured across the following fields:
* **Order Identifiers:** `order_id` (Unique transaction key), `order_date` (Placement date), `ship_date` (Fulfillment date).
* **Geographic Demographics:** `region` (Territory classification), `state` (State-level boundary), `city` (Local municipality).
* **Customer Segments:** `customer_id` (Unique buyer key), `customer_segment` (Consumer, Corporate, Home Office), `customer_rating` (Satisfaction score), `campaign_channel` (Sourcing path: Organic, Paid, Email, Social, Referral).
* **Merchandise Categories:** `category` (Macro department), `sub_category` (Product group), `product_name` (SKU label).
* **Commercial Measures:** `sales` (Gross revenue value), `quantity` (Units sold per line item), `discount` (Promotional markdown rate), `profit` (Net margin return), `return_flag` (Binary field: 1 = Returned, 0 = Retained), `delivery_days` (Fulfillment transit time).

## 3. Tableau Workbook Description
The packaged Tableau workbook (`tableau/executive_dashboard.twbx`) contains the underlying analytical views sheeted systematically to build a clean dashboard interface:
* **Sales Trend View:** A temporal line visualization mapping seasonal demand and revenue cycles across consecutive quarters.
* **Regional Performance View:** A geographic choropleth map isolating state-level margin contributions and volume concentration.
* **Category Profitability View:** A sorted horizontal bar chart breaking down macro categories into sub-category tiers to isolate underperforming merchandise.
* **Customer Segment View:** A side-by-side comparative matrix tracking transactional volume and buying power across core user groups.
* **Shipping Performance View:** An operational distribution breakdown mapping delivery buckets against core shipping modes.
* **Discount vs Profit View:** A continuous scatter matrix illustrating the correlation between promotional markdown percentages and profit degradation thresholds.
* **Return Analysis View:** A multi-dimensional risk matrix tracking reverse logistics patterns across inventory nodes and regions.

## 4. Calculated Fields Created
To support advanced analytical views, the following calculated fields were constructed within the Tableau environment:

1. **Profit Margin**
   * *Logic:* `SUM([profit]) / SUM([sales])`
   * *Formatting:* Set via default properties to standard Percentage with 2 decimal places. Evaluates to an overall baseline of **15.35%**.
2. **Cost**
   * *Logic:* `[sales] - [profit]`
   * *Formatting:* Continuous currency format tracking base operational and manufacturing overhead before retail markup.
3. **Average Order Value (AOV)**
   * *Logic:* `SUM([sales]) / COUNTD([order_id])`
   * *Formatting:* Currency format. Ensures multi-line item orders sharing a single identifier do not skew customer spend averages. Evaluates to a mean of **$51,670.87**.
4. **Return Rate**
   * *Logic:* `SUM([return_flag]) / COUNTD([order_id])`
   * *Formatting:* Percentage format. Tracks structural distribution of order returns against total unique orders. Resolves to a company baseline of **4.55%**.
5. **Shipping Delay Bucket**
   * *Logic:* 
     ```tableau
     IF [delivery_days] <= 2 THEN "0-2 Days (Fast)"
     ELSEIF [delivery_days] <= 5 THEN "3-5 Days (Standard)"
     ELSE "6+ Days (Delayed)"
     END
     ```
   * *Formatting:* Categorical dimension bucket sorting fulfillment speeds into operational performance bands.

## 5. Dashboard Components, Filters, and Interactions
The executive workspace relies on interactive design components to support leadership decision-making:
* **Global KPI Summary Cards:** Positioned at the top of the viewport to track high-level corporate benchmarks (Total Sales, Total Profit, Margin, and AOV).
* **Geographic Target Filtering:** The *Regional Performance View* is configured as an interactive filter action. Selecting any state dynamically adjusts all connected charts to display data *only* for that specific market.
* **Global Context Dropdowns:** Exposed quick-filter controls for `category`, `customer_segment`, and `campaign_channel` provide deep segmentation across all worksheets.
* **Color Optimization:** Applied a monochromatic blue palette for revenue scale and a high-contrast diverging *Orange-Blue* palette to immediately flag negative profit margins in deep orange.

## 6. Key Business Insights Summary
* **Seasonal Margin Contraction:** Sales volumes regularly peak during Q4 periods ($27.94M in 2024 and $29.40M in 2025). However, net profit margins contract during these spikes due to heavy year-end promotional activities.
* **Structural Inventory Drag:** Technology sub-categories drive premium results (Phones lead at an 18.49% margin; Accessories at 18.33%). Conversely, Furniture lines create financial drag, with Tables pulling in a low 5.67% margin.
* **Discount Deficit Thresholds:** Pricing promotions up to 15% maintain healthy margins. However, aggressive promotions at or above 35% trigger a drop into net losses, with a negative profit margin of -4.95%.
* **Reverse Logistics Concentration:** Product returns are tied to product types rather than transit delays. Furniture displays an elevated return rate of 7.67%, whereas long shipping delays (6+ days) show almost no correlation with higher returns.

## 7. Assumptions and Limitations
* **Order-Level Consolidation:** Multi-line items are aggregated under a distinct `order_id` to evaluate transaction-level averages accurately.
* **Return Completeness:** A true `return_flag` on any line item assumes a complete reverse logistics cycle for that transaction line, without accounting for partial partial recoveries or refurbishments.
* **Fulfillment Mapping:** Analysis assumes that the `delivery_days` pre-calculated variable matches standard working calendars without accounting for local holiday distribution pauses.

## 8. Included Screenshots Reference
The following visual proofs are saved within the repository to document complete dashboard compliance:
* `screenshots/full_dashboard.png`: Showcases the complete layout and arranged containers.
* `screenshots/sales_trend_view.png`: Details chronological revenue and profit fluctuations.
* `screenshots/regional_performance_view.png`: Illustrates geographic distribution and regional margin health.
* `screenshots/category_profitability_view.png`: Displays sorted horizontal charts mapping product performance.
* `screenshots/filter_interaction_view.png`: Captures active interactive actions when a specific region is isolated.
