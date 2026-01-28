# DAX Measures

## Core KPIs
```DAX
Total Revenue = SUM(superstore[Sales])
Total Profit = SUM(superstore[Profit])
Orders = DISTINCTCOUNT(superstore[Order ID])
Customers = DISTINCTCOUNT(superstore[Customer ID])
AOV = DIVIDE([Total Revenue], [Orders])
Profit Margin % = DIVIDE([Total Profit], [Total Revenue])


dim_date =
ADDCOLUMNS(
    CALENDAR(MIN(superstore[Order Date]), MAX(superstore[Order Date])),
    "Year", YEAR([Date]),
    "MonthNum", MONTH([Date]),
    "YearMonth", FORMAT([Date], "YYYY-MM"),
    "YearMonthSort", YEAR([Date]) * 100 + MONTH([Date]),
    "MonthStart", DATE(YEAR([Date]), MONTH([Date]), 1)
)

Latest Month = MAX(dim_date[MonthStart])

Revenue (Latest Month) =
VAR lm = [Latest Month]
RETURN
    CALCULATE(
        [Total Revenue],
        FILTER(ALL(dim_date[MonthStart]), dim_date[MonthStart] = lm)
    )

Revenue (Prev Month) =
VAR lm = [Latest Month]
VAR pm = EDATE(lm, -1)
RETURN
    CALCULATE(
        [Total Revenue],
        FILTER(ALL(dim_date[MonthStart]), dim_date[MonthStart] = pm)
    )

Revenue MoM % (Latest) =
DIVIDE(
    [Revenue (Latest Month)] - [Revenue (Prev Month)],
    [Revenue (Prev Month)]
)

Revenue (Prev Year Same Month) =
VAR lm = [Latest Month]
VAR py = EDATE(lm, -12)
RETURN
    CALCULATE(
        [Total Revenue],
        FILTER(ALL(dim_date[MonthStart]), dim_date[MonthStart] = py)
    )

Revenue YoY % (Latest) =
DIVIDE(
    [Revenue (Latest Month)] - [Revenue (Prev Year Same Month)],
    [Revenue (Prev Year Same Month)]
)