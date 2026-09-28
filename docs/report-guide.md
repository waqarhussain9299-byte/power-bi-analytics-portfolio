# Report Guide

## Shared interaction and reading workflow

All five pages are 1920 × 1080, use the same light canvas and teal/gold visual
identity, and have top-row page navigation, date-range filtering, and five KPI
cards with monthly mini-trends. Slicers vary by page and include the relevant
region, category, channel/order type, state, segment, fiscal year, brand, or
fulfillment method fields.

Use the date range and page-specific slicers to narrow the sample, then select
marks in charts to cross-filter related visuals. The PBIR definitions include
standard report visual interactions. Filter behavior should be confirmed in
Power BI Desktop when adapting the project. The project has no verified
dedicated drill-through page.

Currency is displayed in USD. Profit Margin % uses profit divided by net sales
before returns. Return Rate % is a unit rate (returned units divided by units
sold), not an order return rate.

## 1. Sales & Profitability

**Audience:** Executive, finance, and sales leadership.

**Questions:** How much are we selling? How much profit is recorded? Which
categories and promotions contribute to sales? Where might product mix or
discounts merit review?

**Visuals and KPIs:** Net Sales, Gross Sales, Total Profit, Profit Margin %,
and Average Order Value cards; monthly sales/profit trend; category sales and
profit comparison; promotion discount and promoted-sales comparison; a
decomposition tree for net sales; and a US state sales map.

**Analysis workflow:** Start with net sales and margin, compare category
revenue with profit, then inspect promotion discount amounts and use the
decomposition tree to break down net sales by available attributes. Treat
discount amounts as recorded spend, not proof of causal promotion lift.

**Design note:** The Copilot AI narrative panel was removed because Desktop
required an eligible workspace/capacity and permissions. The available state
sales map is shown instead. This is a local report visual, not an AI-generated
summary.

## 2. Geographic Analytics

**Audience:** Regional directors and store managers.

**Questions:** Which regions and states contribute sales? How do stores and
channels compare?

**Visuals and KPIs:** Net Sales, Total Profit, Profit Margin %, Active Selling
Stores, and Sales per Selling Store cards; state map; regional sales ranking;
state heat-map visual; store ranking; and channel mix donut.

**Analysis workflow:** Compare regions first, then inspect state and store
marks. Use the store list to find locations for follow-up and the channel chart
to compare channel composition.

**Rendering caveat:** In the most recent Desktop review, the state heat-map
visual showed a basemap without visible heat intensity. Do not interpret it as
a validated heat layer until its data rendering is confirmed in the target
Power BI environment.

## 3. Time Intelligence

**Audience:** Executives and finance teams.

**Questions:** How do monthly and quarterly sales change? How does the current
period compare with prior periods and the fiscal calendar?

**Visuals and KPIs:** Sales YTD, Sales Growth %, Sales MoM %, Profit Margin %,
and Sales QTD cards; monthly sales/profit; quarterly sales/profit; fiscal-year
sales; year-by-month comparison; and month-over-month trend.

**Analysis workflow:** Set the date range, read the monthly or quarterly trend,
then compare fiscal-year totals and the year-over-year/month-over-month
measures. Growth measures can be blank when the comparison period is
unavailable; do not replace blanks with assumed zero growth.

## 4. Customer & Marketing

**Audience:** Marketing, CRM, and commercial leadership.

**Questions:** Which recorded customer segments contribute sales? How do
New, Returning, and VIP classifications compare? Which acquisition channels
and promotions are associated with sales?

**Visuals and KPIs:** Net Sales, Customers with Sales, Repeat Purchase Rate %,
Loyalty Member Sales, and Promotion Discount Impact cards; segment bars;
customer-type comparison; acquisition-channel ranking; Key Influencers visual;
and promotion sales/discount comparison.

**Analysis workflow:** Compare segments and customer-type sales, then inspect
acquisition channel and promotion summaries. Do not interpret the repeat
purchase rate as cohort retention or the discount as campaign ROI.

**Rendering caveat:** The Key Influencers visual rendered “No influencers
found” in the latest Desktop review. It is present in PBIR, but it has not
produced a usable explanatory result for this sample/model configuration.

## 5. Operations & Returns

**Audience:** Operations and fulfillment managers.

**Questions:** How many units and dollars are recorded as returned? How do
shipping duration, fulfillment methods, and cancellations compare? What is
sales after recorded returns?

**Visuals and KPIs:** Return Rate %, Return Quantity, Return Amount, Shipping
Cost, and Cancelled Orders cards; category return-rate ranking; monthly return
trend; fulfillment shipping-duration comparison; regional net sales after
returns waterfall; and cancellations by fulfillment method.

**Analysis workflow:** Read return units and return amount separately, compare
fulfillment metrics, then inspect cancellations and regional after-return
sales. Cancelled order value is not treated as recognized sales.

## Report limitations

- This is an imported local PBIP project backed by an Excel workbook; the
  report is not a published Power BI Service artifact.
- Service refresh requires a reachable data source, credentials, and possibly
  an on-premises data gateway. A local workstation path is not a service
  refresh configuration.
- The state heat map and Key Influencers visual have the rendering limitations
  described above.
- No customer lifetime value, campaign ROI, or causal promotion-effect claim is
  supported by the available data.
