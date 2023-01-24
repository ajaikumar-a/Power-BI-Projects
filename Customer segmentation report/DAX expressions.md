## DAX Measures created for the analysis

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

