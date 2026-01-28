# Metrics — Superstore Power BI Dashboard

This file defines each KPI shown in the dashboard in plain English and clarifies how comparisons are calculated.

---

## Core KPIs

### Total Revenue
**Definition:** Sum of sales amount across all order lines in the current filter context.  
**DAX:** `SUM(superstore[Sales])`

### Total Profit
**Definition:** Sum of profit across all order lines in the current filter context.  
**DAX:** `SUM(superstore[Profit])`

### Profit Margin %
**Definition:** Profit divided by revenue for the current filter context.  
**Formula:** `Total Profit / Total Revenue`  
**Interpretation:** 0.1247 = 12.47% margin.

### Orders
**Definition:** Count of unique orders in the current filter context.  
**DAX:** `DISTINCTCOUNT(superstore[Order ID])`  
**Note:** Multiple order lines can belong to the same order; this avoids double counting.

### Customers
**Definition:** Count of unique customers in the current filter context.  
**DAX:** `DISTINCTCOUNT(superstore[Customer ID])`

### AOV (Average Order Value)
**Definition:** Revenue per order in the current filter context.  
**Formula:** `Total Revenue / Orders`  
**Note:** This is an average across orders (not order lines).

---

## Trend and breakdown visuals (what they mean)

### Revenue trend (monthly)
**Definition:** Total Revenue aggregated by month (using the date table).  
**Purpose:** Shows seasonality and overall revenue trajectory.

### Revenue by category
**Definition:** Total Revenue grouped by `superstore[Category]`.  
**Purpose:** Identifies which categories drive revenue.

### Profit by region
**Definition:** Total Profit grouped by `superstore[Region]`.  
**Purpose:** Identifies where profit is concentrated geographically.

### Top products table
**Definition:** Products ranked by Total Revenue (Top N).  
**Fields shown:** Product Name, Total Revenue, Total Profit.

---

## Latest-month comparisons

These KPIs are designed to show the change in revenue for the **latest month available in the current filters** (e.g., after filtering Region/Segment/Year).

### Latest Month
**Definition:** The maximum month (MonthStart) available in the current filter context.  
**Use:** Defines the “current month” for MoM and YoY.

### Revenue (Latest Month)
**Definition:** Total Revenue in the latest month (MonthStart = Latest Month).

### Revenue (Prev Month)
**Definition:** Total Revenue in the month immediately before the latest month.

### Revenue MoM % (Latest)
**Definition:** Month-over-month % change for the latest month.  
**Formula:** `(Revenue Latest Month - Revenue Prev Month) / Revenue Prev Month`

### Revenue (Prev Year Same Month)
**Definition:** Total Revenue for the same month one year earlier (Latest Month minus 12 months).

### Revenue YoY % (Latest)
**Definition:** Year-over-year % change for the latest month.  
**Formula:** `(Revenue Latest Month - Revenue Prev Year Same Month) / Revenue Prev Year Same Month`

**Important behaviour**
- These comparison metrics **respect slicers** (Year/Region/Segment/Category etc.) and compute “latest month” **within the filtered data**.
- If the previous month (or previous year month) has no revenue, the % result may be blank due to division by zero.

