# Local Setup

## Requirements

- Power BI Desktop with support for opening PBIP projects.
- Windows, because the included Power Query partitions use a local Excel file
  path.
- The included `data/Sales_Star_Schema.xlsx` sample workbook.

## Open and refresh the project

1. Download or clone the project package.
2. Create `C:\RetailAnalytics` if it does not exist.
3. Copy `data\Sales_Star_Schema.xlsx` to
   `C:\RetailAnalytics\Sales_Star_Schema.xlsx`.
4. Open `Retail Analytics.pbip` in Power BI Desktop.
5. Refresh the semantic model. If Desktop reports an unavailable source, use
   **Transform data → Data source settings → Change Source** to point the
   workbook connector at the copy you saved locally, then refresh again.
6. Navigate across the five report pages and use the page-specific slicers.

The staging copy's M partitions reference
`C:\RetailAnalytics\Sales_Star_Schema.xlsx`. Changing that path in Power Query
is a local setup step; the separate source PBIP project was not changed by
this portfolio preparation.

## Publish for stakeholder viewing

To make the report available in Power BI Service, open the prepared PBIP in
Desktop and use **Home → Publish** to an authorized workspace. Then configure
data-source credentials and scheduled refresh (and a gateway if the source
remains on a local machine). Share the report with the intended audience or
publish a Power BI app.

Publishing requires appropriate workspace permissions and viewer licensing or
qualifying capacity. Configure row-level security if audience members should
not see the same rows. Do not use **Publish to web** for business data.

This repository package does not publish anything, configure a gateway, grant
stakeholder access, or claim a live report URL.
