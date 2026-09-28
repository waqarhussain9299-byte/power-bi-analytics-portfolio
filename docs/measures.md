# DAX Measures

The following definitions are in the PBIP semantic model and grouped into five
measure tables. All amounts use the workbook's recorded values. Most sales,
cost, tax, shipping, and return measures exclude cancelled rows; cancellation
measures intentionally filter to cancelled orders.

## `_Sales Measures` — 11

| Measure | Definition |
| --- | --- |
| Gross Sales | `CALCULATE(SUM('tblFactSales'[GrossSales]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Net Sales | `CALCULATE(SUM('tblFactSales'[NetSales]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Total Discount | `CALCULATE(SUM('tblFactSales'[DiscountAmount]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Total Cost | `CALCULATE(SUM('tblFactSales'[TotalCost]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Total Profit | `CALCULATE(SUM('tblFactSales'[Profit]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Profit Margin % | `DIVIDE([Total Profit], [Net Sales])` |
| Units Sold | `CALCULATE(SUM('tblFactSales'[Quantity]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Distinct Orders | `CALCULATE(DISTINCTCOUNT('tblFactSales'[OrderNumber]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Average Order Value | `DIVIDE([Net Sales], [Distinct Orders])` |
| Average Discount % | `DIVIDE([Total Discount], [Gross Sales])` |
| Tax Amount | `CALCULATE(SUM('tblFactSales'[TaxAmount]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |

## `_Geography Measures` — 6

| Measure | Definition |
| --- | --- |
| Active Selling Stores | `CALCULATE(DISTINCTCOUNT('tblFactSales'[StoreID]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Sales per Selling Store | `DIVIDE([Net Sales], [Active Selling Stores])` |
| Store Sales Contribution % | `DIVIDE([Net Sales], CALCULATE([Net Sales], REMOVEFILTERS('tblDimStore'), REMOVEFILTERS('tblFactSales'[StoreID]), REMOVEFILTERS('tblFactSales'[StoreRegion])))` |
| Channel Sales Contribution % | `DIVIDE([Net Sales], CALCULATE([Net Sales], REMOVEFILTERS('tblDimOrderType'), REMOVEFILTERS('tblFactSales'[OrderType]), REMOVEFILTERS('tblFactSales'[SalesChannel])))` |
| State Sales Rank | `RANKX(ALL('tblDimStore'[StateName]), [Net Sales], , DESC, DENSE)` |
| Region Sales Rank | `RANKX(ALL('tblDimStore'[Region]), [Net Sales], , DESC, DENSE)` |

## `_Time Intelligence Measures` — 7

| Measure | Definition |
| --- | --- |
| Sales YTD | `VAR _Year = MAX('tblDimDate'[Year]) VAR _DateKey = MAX('tblDimDate'[DateKey]) RETURN CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), FILTER(ALL('tblDimDate'), 'tblDimDate'[Year] = _Year && 'tblDimDate'[DateKey] <= _DateKey))` |
| Sales MTD | `VAR _Year = MAX('tblDimDate'[Year]) VAR _Month = MAX('tblDimDate'[MonthNumber]) VAR _DateKey = MAX('tblDimDate'[DateKey]) RETURN CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), FILTER(ALL('tblDimDate'), 'tblDimDate'[Year] = _Year && 'tblDimDate'[MonthNumber] = _Month && 'tblDimDate'[DateKey] <= _DateKey))` |
| Sales QTD | `VAR _Year = MAX('tblDimDate'[Year]) VAR _Quarter = MAX('tblDimDate'[QuarterNumber]) VAR _DateKey = MAX('tblDimDate'[DateKey]) RETURN CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), FILTER(ALL('tblDimDate'), 'tblDimDate'[Year] = _Year && 'tblDimDate'[QuarterNumber] = _Quarter && 'tblDimDate'[DateKey] <= _DateKey))` |
| Sales Growth % | `VAR _Year = MAX('tblDimDate'[Year]) VAR _DateKey = MAX('tblDimDate'[DateKey]) VAR _Month = MAX('tblDimDate'[MonthNumber]) VAR _Day = MOD(_DateKey, 100) VAR _PriorYearYTD = CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), FILTER(ALL('tblDimDate'), 'tblDimDate'[Year] = _Year - 1 && ('tblDimDate'[MonthNumber] < _Month || ('tblDimDate'[MonthNumber] = _Month && MOD('tblDimDate'[DateKey], 100) <= _Day)))) RETURN DIVIDE([Sales YTD] - _PriorYearYTD, _PriorYearYTD)` |
| Sales YoY % | `VAR _PriorYearDateKeys = SELECTCOLUMNS(VALUES('tblDimDate'[DateKey]), "DateKey", 'tblDimDate'[DateKey] - 10000) VAR _PriorYearSales = CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), TREATAS(_PriorYearDateKeys, 'tblDimDate'[DateKey])) RETURN IF(HASONEVALUE('tblDimDate'[Year]), DIVIDE([Net Sales] - _PriorYearSales, _PriorYearSales))` |
| Sales MoM % | `VAR _MonthIndex = MAX('tblDimDate'[Year]) * 12 + MAX('tblDimDate'[MonthNumber]) VAR _CurrentMonthSales = CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), FILTER(ALL('tblDimDate'), 'tblDimDate'[Year] * 12 + 'tblDimDate'[MonthNumber] = _MonthIndex)) VAR _PriorMonthSales = CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), FILTER(ALL('tblDimDate'), 'tblDimDate'[Year] * 12 + 'tblDimDate'[MonthNumber] = _MonthIndex - 1)) RETURN DIVIDE(_CurrentMonthSales - _PriorMonthSales, _PriorMonthSales)` |
| Sales Fiscal YTD | `VAR _FiscalYear = MAX('tblDimDate'[FiscalYear]) VAR _FiscalMonth = MAX('tblDimDate'[FiscalMonthNumber]) VAR _DateKey = MAX('tblDimDate'[DateKey]) RETURN CALCULATE([Net Sales], REMOVEFILTERS('tblDimDate'), FILTER(ALL('tblDimDate'), 'tblDimDate'[FiscalYear] = _FiscalYear && 'tblDimDate'[FiscalMonthNumber] <= _FiscalMonth && 'tblDimDate'[DateKey] <= _DateKey))` |

The TMDL definitions in
`Retail Analytics.SemanticModel/definition/tables/_Time Intelligence Measures.tmdl`
are the executable source of truth; this documentation lists the same
expressions for reference.

## `_Customer & Marketing Measures` — 11

| Measure | Definition |
| --- | --- |
| Customers with Sales | `CALCULATE(DISTINCTCOUNT('tblFactSales'[CustomerID]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Customers by Acquisition Channel | `DISTINCTCOUNT('tblDimCustomer'[CustomerID])` |
| New Customer Sales | `CALCULATE([Net Sales], KEEPFILTERS('tblFactSales'[CustomerType] = "New"))` |
| Returning Customer Sales | `CALCULATE([Net Sales], KEEPFILTERS('tblFactSales'[CustomerType] = "Returning"))` |
| VIP Customer Sales | `CALCULATE([Net Sales], KEEPFILTERS('tblFactSales'[CustomerType] = "VIP"))` |
| Repeat Purchase Customers | `COUNTROWS(FILTER(VALUES('tblFactSales'[CustomerID]), CALCULATE(DISTINCTCOUNT('tblFactSales'[OrderNumber]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled")) > 1))` |
| Repeat Purchase Rate % | `DIVIDE([Repeat Purchase Customers], [Customers with Sales])` |
| Loyalty Member Customers | `CALCULATE(DISTINCTCOUNT('tblFactSales'[CustomerID]), KEEPFILTERS('tblFactSales'[LoyaltyStatus] = "Loyalty Member"), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Loyalty Member Sales | `CALCULATE([Net Sales], KEEPFILTERS('tblFactSales'[LoyaltyStatus] = "Loyalty Member"))` |
| Promoted Sales | `CALCULATE([Net Sales], KEEPFILTERS('tblDimPromotion'[PromotionID] <> "PR000"))` |
| Promotion Discount Impact | `CALCULATE([Total Discount], KEEPFILTERS('tblDimPromotion'[PromotionID] <> "PR000"))` |

## `_Operations & Returns Measures` — 10

| Measure | Definition |
| --- | --- |
| Return Quantity | `CALCULATE(SUM('tblFactSales'[ReturnQuantity]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Return Rate % | `DIVIDE([Return Quantity], [Units Sold])` |
| Return Amount | `CALCULATE(SUM('tblFactSales'[ReturnAmount]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Net Sales After Returns | `[Net Sales] - [Return Amount]` |
| Shipping Cost | `CALCULATE(SUM('tblFactSales'[ShippingCost]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Average Ship Days per Order | `AVERAGEX(VALUES('tblFactSales'[OrderNumber]), CALCULATE(AVERAGE('tblFactSales'[ShipDays]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled")))` |
| Cancelled Orders | `CALCULATE(DISTINCTCOUNT('tblFactSales'[OrderNumber]), KEEPFILTERS('tblFactSales'[OrderStatus] = "Cancelled"))` |
| Cancelled Gross Value | `CALCULATE(SUM('tblFactSales'[GrossSales]), KEEPFILTERS('tblFactSales'[OrderStatus] = "Cancelled"))` |
| Net Units After Returns | `CALCULATE(SUM('tblFactSales'[NetQuantity]), KEEPFILTERS('tblFactSales'[OrderStatus] <> "Cancelled"))` |
| Processing Orders | `CALCULATE(DISTINCTCOUNT('tblFactSales'[OrderNumber]), KEEPFILTERS('tblFactSales'[OrderStatus] = "Processing"))` |

## Business definitions and boundaries

- **Net Sales** is after discounts but before returns. **Net Sales After
  Returns** subtracts the separate recorded return amount. Tax is separate.
- **Return Rate %** is a unit return rate, not an order-return percentage.
- **Distinct Orders** and **Cancelled Orders** count distinct `OrderNumber`
  values, not transaction lines.
- **Promotion Discount Impact** is recorded discount spend, not incremental
  revenue, campaign ROI, or causal sales lift.
- **Repeat Purchase Rate %** is an observed repeat-order share in the current
  filter context, not a cohort-retention calculation or lifetime value.
- **Net Units After Returns** is not an inventory-on-hand measure.

All percentage measures use safe `DIVIDE` logic in the model; currency,
percentage, and count format strings are defined on the measures.
