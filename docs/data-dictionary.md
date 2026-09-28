# Data Dictionary

The source is the fictional/sample Excel workbook. Column names below come
from its named tables. Dollar amounts are USD in the sample; the fact table
also carries a `Currency` field.

## Fact table

### `tblFactSales` — 1,000 transaction lines

| Column | Meaning |
| --- | --- |
| `TransactionID` | Unique sample transaction-line identifier |
| `DateKey`, `OrderDate`, `OrderTimestamp` | Date dimension key and source order date/time |
| `CustomerID`, `ProductID`, `StoreID`, `SalesRepID`, `PromotionID` | Dimension foreign keys |
| `OrderNumber` | Order-level identifier; repeated across lines in an order |
| `OrderLineStatus`, `OrderStatus` | Source line and order status labels |
| `OrderType`, `SalesChannel`, `FulfillmentMethod`, `PaymentMethod` | Transaction channel and fulfillment/payment descriptors |
| `Quantity`, `UnitPrice`, `UnitCost` | Ordered quantity and unit price/cost |
| `DiscountPct`, `GrossSales`, `DiscountAmount`, `NetSales` | Recorded list value, discount, and sales after discount before returns |
| `TotalCost`, `ShippingCost`, `TaxRate`, `TaxAmount`, `Profit` | Recorded financial amounts; tax is separate from source profit |
| `IsReturned`, `ReturnQuantity`, `ReturnAmount`, `NetQuantity` | Return flag, returned quantity/amount, and source net quantity |
| `ShipDays` | Recorded shipping duration in days |
| `DeviceType`, `OrderSource` | Device and acquisition/source labels captured on the line |
| `CustomerType`, `LoyaltyStatus` | New/Returning/VIP and loyalty labels carried on the line |
| `Currency`, `StoreRegion`, `OrderQuarter` | Currency and denormalized region/quarter descriptors |

## Dimensions

| Table | Rows | Columns |
| --- | ---: | --- |
| `tblDimDate` | 1,096 | `DateKey`, `Date`, `Year`, `QuarterLabel`, `MonthNumber`, `MonthName`, `MonthShort`, `QuarterNumber`, `CalendarQuarter`, `DayName`, `DayShort`, `WeekNumber`, `DayOfYear`, `FiscalYear`, `FiscalMonthNumber`, `FiscalPeriod`, `DayType`, `IsNewYear`, `IsPeakSeason` |
| `tblDimCustomer` | 180 | `CustomerID`, `CustomerName`, `Segment`, `City`, `StateCode`, `StateName`, `Region`, `AreaType`, `SignupDate`, `AcquisitionChannel`, `CustomerTier`, `EmailOptIn`, `SMSOptIn` |
| `tblDimProduct` | 80 | `ProductID`, `ProductName`, `Category`, `SubCategory`, `Brand`, `PriceBand`, `IsPrivateLabel`, `ListPrice`, `StandardCost`, `ProductStatus`, `SourcingType` |
| `tblDimStore` | 50 | `StoreID`, `StoreName`, `City`, `StateCode`, `StateName`, `Region`, `Channel`, `StoreType`, `OpenDate`, `StoreSize`, `SalesIndex` |
| `tblDimSalesRep` | 30 | `SalesRepID`, `SalesRepName`, `JobTitle`, `Region`, `StateCode`, `Territory`, `HireDate`, `EmployeeStatus` |
| `tblDimPromotion` | 10 | `PromotionID`, `PromotionName`, `PromotionType`, `StandardDiscountPct`, `Eligibility` |
| `tblDimPaymentMethod` | 6 | `PaymentMethod`, `PaymentCategory`, `FeeRate`, `DigitalPaymentFlag` |
| `tblDimOrderType` | 4 | `OrderType`, `ChannelGroup`, `TypicalFulfillment` |
| `tblDimDevice` | 4 | `DeviceType`, `DeviceGroup` |
| `tblDimCustomerType` | 3 | `CustomerType`, `Description` |
| `tblDimLoyaltyStatus` | 4 | `LoyaltyStatus`, `Description` |

## Interpretation notes

- Customer and sales-representative names are present in the workbook. Its
  ReadMe identifies these records as fictional/sample; do not substitute live
  personal information into a public fork.
- `NetSales` is before recorded returns. `[Net Sales After Returns]` subtracts
  the separate `ReturnAmount` measure.
- `Return Rate %` is a unit rate, not a percentage of orders returned.
- `Cancelled Orders` counts distinct order numbers; cancelled gross value is
  not treated as recognized sales.
- The inventory-related measure is based on source net quantity; the model
  does not provide an inventory-on-hand fact.
