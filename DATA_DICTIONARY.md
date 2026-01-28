# Data Dictionary — Superstore Power BI Dashboard

This project uses a simple model:
- **superstore**: transactional order line data (fact-like table)
- **dim_date**: calendar table generated in Power BI (date dimension)

---

## Table: `superstore`

**Grain:** 1 row = 1 order line (a single product line within an order).

| Column | Type | Description |
|---|---:|---|
| Row ID | Whole number | Unique row identifier (order line). |
| Order ID | Text | Order identifier. Multiple rows can share the same Order ID. |
| Order Date | Date | Date the order was placed. Used for time-based analysis (linked to `dim_date`). |
| Ship Date | Date | Date the order shipped. |
| Ship Mode | Text | Shipping method (e.g., Standard Class). |
| Customer ID | Text | Customer identifier. |
| Customer Name | Text | Customer name (display field; not unique). |
| Segment | Text | Customer segment (e.g., Consumer, Corporate, Home Office). |
| Country | Text | Country of shipping address. |
| City | Text | City of shipping address. |
| State | Text | State/Province of shipping address. |
| Postal Code | Whole number | Postal code. |
| Region | Text | Region (e.g., West, East, Central, South). |
| Product ID | Text | Product identifier. |
| Category | Text | High-level product category (e.g., Technology). |
| Sub-Category | Text | More detailed product category. |
| Product Name | Text | Product display name. |
| Sales | Decimal number | Revenue for the order line. |
| Quantity | Whole number | Units sold for the order line. |
| Discount | Decimal number | Discount rate applied (0–1). |
| Profit | Decimal number | Profit for the order line. |

**Notes**
- KPIs such as **Orders** and **Customers** are computed via distinct counts.
- The dashboard uses **Order Date** as the primary date for trend metrics.

---

## Table: `dim_date` (Power BI calculated table)

**Grain:** 1 row = 1 calendar day from min(Order Date) to max(Order Date).

| Column | Type | Description |
|---|---:|---|
| Date | Date | Calendar day (primary key for the date table). |
| Year | Whole number | Year extracted from Date. |
| MonthNum | Whole number | Month number (1–12). |
| YearMonth | Text | Year-month label formatted `YYYY-MM` (display). |
| YearMonthSort | Whole number | Numeric sort key for YearMonth (`YYYY*100 + MM`). |
| MonthStart | Date | First day of the month for each Date (used for monthly comparisons). |

---

## Relationships

| From | To | Type | Active | Notes |
|---|---|---|---|---|
| `superstore[Order Date]` | `dim_date[Date]` | Many-to-one | Yes | Filters flow from `dim_date` to `superstore`. |
