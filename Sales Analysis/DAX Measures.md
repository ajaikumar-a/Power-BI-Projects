## Calendar table measure
```
calendar = GENERATE(
    CALENDAR(
        DATE(2013, 1, 1), DATE(2014, 12, 31)
    ), 

    VAR CurrentDate = [Date]
    VAR Day = DAY(CurrentDate)
    VAR Month = MONTH(CurrentDate)
    VAR Quarter = QUARTER(CurrentDate)
    VAR Year = YEAR(CurrentDate)

    RETURN ROW(
        "Year", Year,
        "Quarter", Quarter,
        "Month", Month,
        "Day", Day
    )
)
```

## Sales metrics measures
```
Total Sales = CALCULATE(
    SUM(sales[Gross Sales])
)
```
```
Total Units Sold = CALCULATE(
    SUM(sales[Units Sold])
)
```
```
Total Selling Price = CALCULATE(
    SUM(sales[Sale Price])
)
```
```
Average Selling Price = DIVIDE(
    [Total Revenue],
    [Total Units Sold],
    0
)
```
```
Total Profit = CALCULATE(
    SUM(
        sales[Profit]
    )
)
```
```
Profit Margin = DIVIDE(
    [Total Sales] - CALCULATE(SUM(sales[COGS])),
    [Total Sales],
    0
)
```
```
Total Discounts = CALCULATE(
    SUM(
        sales[Discounts]
    )
)
```
```
Gross Margin = [Total Revenue] - CALCULATE(SUM(sales[COGS]))
```

## Previous month sales metrics measures
```
Previous Month Units Sold = CALCULATE(
    [Total Units Sold],
    PREVIOUSMONTH(
        LASTDATE(
            'calendar'[Date]
        )
    )
)
```
```
Previous Month Revenue = CALCULATE(
    [Total Revenue], 
    PREVIOUSMONTH(
        LASTDATE(
            'calendar'[Date]
        )
    )
)
```
```
Previous Month Profit Margin = CALCULATE(
    [Profit Margin],
    PREVIOUSMONTH(
        LASTDATE(
            'calendar'[Date]
        )
    )
)
```
```
Previous Month Gross Margin = CALCULATE(
    [Gross Margin],
    PREVIOUSMONTH(
        LASTDATE(
            'calendar'[Date]  
        )
    )
)
```
```
Previous Month Avg Selling Price = CALCULATE(
    [Average Selling Price],
    PREVIOUSMONTH(
        LASTDATE(
            'calendar'[Date]
        )
    )
)
```

## Discount analysis measures
```
Discount adjustment (%) = GENERATESERIES(-1, 1, 0.1)
```
```
Discount adjustment (%) Value = SELECTEDVALUE('Discount adjustment (%)'[Discount adjustment (%)], 0)
```
```
Adjusted discount = [Total Discounts] * 'Discount adjustment (%)'[Discount adjustment (%) Value]
```
```
Adjusted sales = [Total Sales] - 'Discount adjustment (%)'[Adjusted discount]
```
```
Adjusted Profit Margin = DIVIDE(
    [Adjusted sales] - CALCULATE(SUM(sales[COGS])),
    [Adjusted sales],
    0
)
```
