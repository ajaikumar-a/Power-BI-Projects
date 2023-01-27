## DAX expression used for creating a calender table

```
calender = 
GENERATE(
    CALENDAR(
        DATE(2010, 1, 1), DATE(2011, 12, 31)
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


## DAX expressions used for creating measures

```DAX
Total orders = CALCULATE(
    DISTINCTCOUNT(
        orders[InvoiceNo]
    ),
    orders[Status] = "Ordered"
)
```

```
Total ordered sales = CALCULATE(
    SUMX(
        orders,
        orders[Quantity] * orders[UnitPrice]
    ),
    orders[Status] = "Ordered"
)
```

```
Total ordered quantity = CALCULATE(
    SUM(orders[Quantity]), 
    orders[Status] = "Ordered"
)
```

```
Total cancelled sales = CALCULATE(
    SUMX(
        orders,
        orders[Quantity] * orders[UnitPrice]
    ),
    orders[Status] = "Cancelled"
)
```

```
Total cancelled quantity = CALCULATE(
    SUM(orders[Quantity]), 
    orders[Status] = "Cancelled"
)
```

```
Total cancelled orders = CALCULATE(
    DISTINCTCOUNT(
        orders[InvoiceNo]
    ), 
    orders[Status] = "Cancelled"
)
```

```
No.of wholesalers = COUNT(customers[CustomerID])
```

```
Previous month sales = calculate(
    [Total ordered sales],
    DATEADD(
        calender[Date],
        -1,
        MONTH
    )
)
```

```
Previous month orders = CALCULATE(
    [Total orders],
    DATEADD(
        calender[Date],
        -1,
        MONTH
    )
)
```

```
Previous month cancelled sales = CALCULATE(
    [Total cancelled sales],
    DATEADD(
        calender[Date],
        -1,
        MONTH
    )
)
```

```
Previous month cancelled orders = CALCULATE(
    [Total cancelled orders],
    DATEADD(
        calender[Date],
        -1,
        MONTH
    )
)
```

```
Average ordered quantity = CALCULATE(
    AVERAGE(orders[Quantity])
)
```

```
Average unit price = CALCULATE(
    AVERAGE(orders[UnitPrice])
)
```

```
Cancellation rate = DIVIDE(
    [Total cancelled orders],
    [Total orders],
    0
)
```

```
Last order date = CALCULATE(
    CALCULATE(
    MAX(
        orders[InvoiceDate]
    ), 
    orders[Status] = "Ordered"
    ),
    FILTER(
        ALL(orders),
        orders[CustomerID] = SELECTEDVALUE(orders[CustomerID])
    )
)
```

```
Last cancelled date = CALCULATE(
    CALCULATE(
    MAX(
        orders[InvoiceDate]
    ), 
    orders[Status] = "Cancelled"
    ),
    FILTER(
        ALL(orders),
        orders[CustomerID] = SELECTEDVALUE(orders[CustomerID])
    )
)
```

```
Last transaction date = MAXX(
    ALL(orders),
    orders[InvoiceDate]
)
```

```
Order F value = CALCULATE(
    DISTINCTCOUNT(
        orders[InvoiceNo]
    ), 
    orders[Status] = "Ordered"
)
```

```
Order M value = DIVIDE(
    [Total ordered sales], 
    [Total ordered quantity], 
    0
)
```

```
Order R value = DATEDIFF(
    [Last order date], 
    [Last transaction date], 
    DAY
)
```

```
Cancelled F value = CALCULATE(
    DISTINCTCOUNT(
        orders[InvoiceNo]
    ), 
    orders[Status] = "Cancelled"
)
```

```
Cancelled M value = DIVIDE(
    [Total cancelled sales], 
    [Total cancelled quantity], 
    0
)
```

```
Cancelled R value = DATEDIFF(
    [Last cancelled date], 
    [Last transaction date], 
    DAY
)
```

## DAX expression used for creating the RFM table

```
RFM table = SUMMARIZE(
    orders, 
    customers[CustomerID],
    "Order R value", [Order R value],
    "Order F value", [Order F value],
    "Order M value", [Order M value],
    "Cancelled R value", [Cancelled R value],
    "Cancelled F value", [Cancelled F value],
    "Cancelled M value", [Cancelled M value]
)
```

## DAX expressions used for creating calculated columns

```
Order R score = SWITCH(
    TRUE(),
    'RFM table'[Order R value] <= PERCENTILE.INC('RFM table'[Order R value], 0.20), 5,
    'RFM table'[Order R value] <= PERCENTILE.INC('RFM table'[Order R value], 0.40), 4,
    'RFM table'[Order R value] <= PERCENTILE.INC('RFM table'[Order R value], 0.60), 3,
    'RFM table'[Order R value] <= PERCENTILE.INC('RFM table'[Order R value], 0.80), 2,
    1
)
```

```
Order M score = SWITCH(
    TRUE(),
    'RFM table'[Order M value] <= PERCENTILE.INC('RFM table'[Order M value], 0.20), 1,
    'RFM table'[Order M value] <= PERCENTILE.INC('RFM table'[Order M value], 0.40), 2,
    'RFM table'[Order M value] <= PERCENTILE.INC('RFM table'[Order M value], 0.60), 3,
    'RFM table'[Order M value] <= PERCENTILE.INC('RFM table'[Order M value], 0.80), 4,
    5
)
```

```
Order F score = SWITCH(
    TRUE(),
    'RFM table'[Order F value] <= PERCENTILE.INC('RFM table'[Order F value], 0.20), 1,
    'RFM table'[Order F value] <= PERCENTILE.INC('RFM table'[Order F value], 0.40), 2,
    'RFM table'[Order F value] <= PERCENTILE.INC('RFM table'[Order F value], 0.60), 3,
    'RFM table'[Order F value] <= PERCENTILE.INC('RFM table'[Order F value], 0.80), 4,
    5
)
```

```
Order RFM score = 'RFM table'[Order R score] & 'RFM table'[Order F score] & 'RFM table'[Order M score]
```
