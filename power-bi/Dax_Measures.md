# DAX Measures Library
> Complete collection of calculated measures for the Executive Finance Dashboard

## 📊 Core Revenue Measures

### **Basic Revenue Calculations**
```dax
// Primary revenue measure
Total Revenue = SUM(Fact_Orders[Revenue])

// Total cost calculation
Total Cost = SUM(Fact_Orders[Cost])

// Gross profit calculation
Total Gross Profit = SUM(Fact_Orders[GrossProfit])

// Gross margin percentage
Gross Margin % = 
DIVIDE(
    [Total Gross Profit],
    [Total Revenue],
    0
)

// Order count
Total Orders = COUNT(Fact_Orders[OrderID])

// Average order value
Average Order Value = 
DIVIDE(
    [Total Revenue],
    [Total Orders],
    0
)
// Previous year revenue
Revenue PY = 
CALCULATE(
    [Total Revenue],
    SAMEPERIODLASTYEAR(Dim_Date[Date])
)

// Year-over-year growth percentage
Revenue YoY Growth = 
VAR CurrentRevenue = [Total Revenue]
VAR PreviousRevenue = [Revenue PY]
RETURN
    DIVIDE(
        CurrentRevenue - PreviousRevenue,
        PreviousRevenue,
        0
    )

// Year-over-year growth absolute
Revenue YoY Growth $ = [Total Revenue] - [Revenue PY]

// Previous year orders
Orders PY = 
CALCULATE(
    [Total Orders],
    SAMEPERIODLASTYEAR(Dim_Date[Date])
)

// Order growth percentage
Orders YoY Growth = 
DIVIDE(
    [Total Orders] - [Orders PY],
    [Orders PY],
    0
)
// Previous quarter revenue
Revenue PQ = 
CALCULATE(
    [Total Revenue],
    PREVIOUSQUARTER(Dim_Date[Date])
)

// Quarter-over-quarter growth
Revenue QoQ Growth = 
DIVIDE(
    [Total Revenue] - [Revenue PQ],
    [Revenue PQ],
    0
)

// Previous month revenue
Revenue PM = 
CALCULATE(
    [Total Revenue],
    PREVIOUSMONTH(Dim_Date[Date])
)

// Month-over-month growth
Revenue MoM Growth = 
DIVIDE(
    [Total Revenue] - [Revenue PM],
    [Revenue PM],
    0
)
// Year-to-date revenue
Revenue YTD = 
TOTALYTD(
    [Total Revenue],
    Dim_Date[Date]
)

// Previous year YTD
Revenue YTD PY = 
CALCULATE(
    [Revenue YTD],
    SAMEPERIODLASTYEAR(Dim_Date[Date])
)

// YTD growth
Revenue YTD Growth = 
DIVIDE(
    [Revenue YTD] - [Revenue YTD PY],
    [Revenue YTD PY],
    0
)

// Quarter-to-date revenue
Revenue QTD = 
TOTALQTD(
    [Total Revenue],
    Dim_Date[Date]
)

// Month-to-date revenue
Revenue MTD = 
TOTALMTD(
    [Total Revenue],
    Dim_Date[Date]
)
// Rolling 12-month revenue
Revenue R12M = 
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(
        Dim_Date[Date],
        LASTDATE(Dim_Date[Date]),
        -12,
        MONTH
    )
)

// Rolling 3-month revenue
Revenue R3M = 
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(
        Dim_Date[Date],
        LASTDATE(Dim_Date[Date]),
        -3,
        MONTH
    )
)

// Rolling 6-month average
Revenue R6M Average = 
DIVIDE(
    CALCULATE(
        [Total Revenue],
        DATESINPERIOD(
            Dim_Date[Date],
            LASTDATE(Dim_Date[Date]),
            -6,
            MONTH
        )
    ),
    6,
    0
)
// Unique customer count
Customer Count = DISTINCTCOUNT(Fact_Orders[CustomerID])

// Previous year customer count
Customer Count PY = 
CALCULATE(
    [Customer Count],
    SAMEPERIODLASTYEAR(Dim_Date[Date])
)

// Customer growth
Customer Growth = 
DIVIDE(
    [Customer Count] - [Customer Count PY],
    [Customer Count PY],
    0
)

// New customers (first-time buyers)
New Customers = 
CALCULATE(
    [Customer Count],
    FILTER(
        ALL(Dim_Date),
        Dim_Date[Date] = 
        CALCULATE(
            MIN(Fact_Orders[OrderDate]),
            ALL(Dim_Date)
        )
    )
)

// Revenue per customer
Revenue per Customer = 
DIVIDE(
    [Total Revenue],
    [Customer Count],
    0
)
// Customer lifetime value
Customer LTV = 
AVERAGEX(
    VALUES(Dim_Customer[CustomerID]),
    CALCULATE(
        SUM(Fact_Orders[Revenue]),
        ALL(Dim_Date)
    )
)

// Average customer tenure (in days)
Average Customer Tenure = 
AVERAGEX(
    VALUES(Dim_Customer[CustomerID]),
    DATEDIFF(
        CALCULATE(MIN(Fact_Orders[OrderDate]), ALL(Dim_Date)),
        CALCULATE(MAX(Fact_Orders[OrderDate]), ALL(Dim_Date)),
        DAY
    )
)
// Product count
Product Count = DISTINCTCOUNT(Fact_Orders[ProductID])

// Top performing product
Top Product = 
CALCULATE(
    MAX(Dim_Product[ProductName]),
    TOPN(
        1,
        SUMMARIZE(
            Dim_Product,
            Dim_Product[ProductName],
            "Revenue", [Total Revenue]
        ),
        [Revenue],
        DESC
    )
)

// Product mix percentage
Product Mix % = 
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        ALL(Dim_Product[ProductName])
    ),
    0
)

// Revenue rank by product
Product Revenue Rank = 
RANKX(
    ALL(Dim_Product[ProductName]),
    [Total Revenue],
    ,
    DESC
)
// Top category by revenue
Top Category = 
CALCULATE(
    MAX(Dim_Product[Category]),
    TOPN(
        1,
        SUMMARIZE(
            Dim_Product,
            Dim_Product[Category],
            "Revenue", [Total Revenue]
        ),
        [Revenue],
        DESC
    )
)

// Category concentration (% of top category)
Category Concentration = 
VAR TopCategoryRevenue = 
    CALCULATE(
        [Total Revenue],
        Dim_Product[Category] = [Top Category]
    )
RETURN
    DIVIDE(
        TopCategoryRevenue,
        CALCULATE([Total Revenue], ALL(Dim_Product[Category])),
        0
    )
// Top region by revenue
Top Region = 
CALCULATE(
    MAX(Dim_Geography[Region]),
    TOPN(
        1,
        SUMMARIZE(
            Dim_Geography,
            Dim_Geography[Region],
            "Revenue", [Total Revenue]
        ),
        [Revenue],
        DESC
    )
)

// Regional market share
Regional Market Share = 
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        ALL(Dim_Geography[Region])
    ),
    0
)

// Revenue per geography
Revenue per Geography = 
DIVIDE(
    [Total Revenue],
    DISTINCTCOUNT(Fact_Orders[GeographyID]),
    0
)
// Active sales reps
Active Sales Reps = DISTINCTCOUNT(Fact_Orders[SalesRepID])

// Revenue per sales rep
Revenue per Rep = 
DIVIDE(
    [Total Revenue],
    [Active Sales Reps],
    0
)

// Top performing rep
Top Sales Rep = 
CALCULATE(
    MAX(Dim_SalesRep[RepName]),
    TOPN(
        1,
        SUMMARIZE(
            Dim_SalesRep,
            Dim_SalesRep[RepName],
            "Revenue", [Total Revenue]
        ),
        [Revenue],
        DESC
    )
)

// Sales rep ranking
Sales Rep Rank = 
RANKX(
    ALL(Dim_SalesRep[RepName]),
    [Total Revenue],
    ,
    DESC
)
// Channel effectiveness (Revenue/Cost ratio)
Channel Effectiveness = 
AVERAGEX(
    VALUES(Dim_Channel[ChannelName]),
    DIVIDE(
        [Total Revenue],
        [Total Cost],
        0
    )
)

// Channel mix percentage
Channel Mix % = 
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        ALL(Dim_Channel[ChannelName])
    ),
    0
)

// Most effective channel
Top Channel = 
CALCULATE(
    MAX(Dim_Channel[ChannelName]),
    TOPN(
        1,
        SUMMARIZE(
            Dim_Channel,
            Dim_Channel[ChannelName],
            "Effectiveness", [Channel Effectiveness]
        ),
        [Effectiveness],
        DESC
    )
)
// Revenue target (example: 10% growth)
Revenue Target = [Revenue PY] * 1.10

// Revenue vs target
Revenue vs Target = [Total Revenue] - [Revenue Target]

// Target achievement percentage
Target Achievement % = 
DIVIDE(
    [Total Revenue],
    [Revenue Target],
    0
)

// Traffic light indicator for targets
Revenue Status = 
SWITCH(
    TRUE(),
    [Target Achievement %] >= 1.05, "🟢 Exceeding",
    [Target Achievement %] >= 0.95, "🟡 On Track", 
    "🔴 Below Target"
)
// Simple trend-based forecast
Revenue Forecast = 
VAR CurrentPeriodRevenue = [Total Revenue]
VAR GrowthRate = [Revenue YoY Growth]
VAR ForecastRevenue = CurrentPeriodRevenue * (1 + GrowthRate)
RETURN
    IF(
        ISBLANK(CurrentPeriodRevenue),
        ForecastRevenue,
        CurrentPeriodRevenue
    )

// Forecast accuracy (for historical comparison)
Forecast Accuracy = 
1 - ABS(
    DIVIDE(
        [Revenue Forecast] - [Total Revenue],
        [Total Revenue],
        0
    )
)
// Revenue formatted for display
Revenue Display = 
VAR Revenue = [Total Revenue]
RETURN
    SWITCH(
        TRUE(),
        Revenue >= 1000000000, FORMAT(Revenue/1000000000, "$0.0") & "B",
        Revenue >= 1000000, FORMAT(Revenue/1000000, "$0.0") & "M",
        Revenue >= 1000, FORMAT(Revenue/1000, "$0") & "K",
        FORMAT(Revenue, "$0")
    )

// Growth indicator with arrow
Growth Indicator = 
VAR Growth = [Revenue YoY Growth]
RETURN
    IF(
        Growth > 0,
        "↗️ " & FORMAT(Growth, "0.0%"),
        "↘️ " & FORMAT(Growth, "0.0%")
    )

// Dynamic title based on selection
Dynamic Title = 
VAR SelectedCustomer = SELECTEDVALUE(Dim_Customer[CustomerName], "All Customers")
VAR SelectedProduct = SELECTEDVALUE(Dim_Product[Category], "All Products")
RETURN
    "Revenue Performance: " & SelectedCustomer & " - " & SelectedProduct
