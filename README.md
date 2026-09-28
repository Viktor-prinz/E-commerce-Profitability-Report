<p align="center">
  <img src="Screenshots/nordhaven_logo1.png" alt="Nordhaven logo" width="120">
</p>

<h1 align="center">Nordhaven: E-commerce Profitability Analytics</h1>

<p align="center">
  A 3-page interactive Power BI report that follows every euro from gross sales down to contribution margin.<br>
  Built for the ZoomCharts Power BI Challenge, September 2026.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Power_BI-F2C811?logo=powerbi&logoColor=black" alt="Power BI">
  <img src="https://img.shields.io/badge/DAX-0B2E5C" alt="DAX">
  <img src="https://img.shields.io/badge/Power_Query-0B2E5C" alt="Power Query">
  <img src="https://img.shields.io/badge/ZoomCharts-Drill_Down_PRO-F2A900" alt="ZoomCharts Drill Down PRO">
</p>

<p align="center"><b><a href="INSERT_REPORT_LINK">View the interactive report</a></b></p>

![Growth and Profitability page](screenshots/01_growth_and_profitability.png)

---

## Business problem

Revenue is the number most dashboards lead with, and it is also the easiest one to misread. In e-commerce a sale can be discounted, partly refunded, returned, shipped at a loss, or never fulfilled because the item was out of stock. The question that matters to leadership is simpler: of every euro sold, how much did the business keep, and where did the rest go?

This report is written for marketing and operations leads deciding which categories, channels and promotions deserve more investment, and which ones quietly erode margin.

## Key findings

All figures are for the full 2024 to 2025 period.

| Question | What the data shows |
|---|---|
| Where does the money go? | Of €789.19K gross sales, €640.42K (81%) survives discounts and refunds. After product, fulfillment, shipping, marketing and payment costs, €127.65K remains as contribution margin: 16.2% of gross sales, or 19.4% of contribution revenue. |
| Which channel earns its keep? | Owned commerce brings in 73.5% of net sales but 83.4% of contribution margin. It keeps about €0.23 of margin per €1 of net sales, against roughly €0.12 for marketplace and €0.13 for social commerce. |
| Which brand tier carries the margin? | Core brands deliver 55.6% of contribution margin, Premium 35.3% and Value 9.1%. |
| Do customers come back? | 74.7% of the 2,200 customers ordered more than once (1,643 customers). Each customer averages €291.10 in net sales and €58.02 in contribution margin. |
| What drives returns? | 10.0% of fulfilled units come back (1,099 of 10,951), costing €102.90K, or €93.63 per returned unit. Fit or preference and product quality account for 81.3% of that loss. Fulfillment issues account for only 1.8%. |
| Is delivery meeting expectations? | 92.8% of deliveries are on time, but 5 of 6 regions sit below their own benchmark. Southern Europe is furthest behind at 84.0% against an expected 87%. |
| Where is demand being lost? | Stock gaps cost an estimated €36.71K in lost sales, and about half of it (€18.69K) sits in Electronics. |
| How dependent is revenue on promotions? | 63.2% of net sales (€404.9K) were sold under a promotion. |

## Dataset

The dataset was provided by ZoomCharts for the challenge: a European e-commerce dataset of 9,109 order lines from 2024 to 2025, in a single currency (EUR). The source workbook is not redistributed in this repository and can be downloaded from the challenge page.

| Table | Grain | Rows |
|---|---|---|
| `FactOrderLine` | One row per order line | 9,109 |
| `DimDate` | One row per day | 1,096 |
| `DimProduct` | One row per SKU | 60 |
| `DimCustomer` | One row per customer | 2,200 |
| `DimGeography` | One row per city (12 countries, 6 regions) | 24 |
| `DimPromotion` | One row per promotion | 10 |
| `DimFulfillment` | One row per fulfillment setup | 6 |
| `DimSalesChannel` | One row per channel | 5 |
| `DimReturnReason` | One row per return reason | 7 |
| `DimCohortAge` | Months since first purchase (0 to 24) | 25 |

## Data model

A star schema with one fact table, nine dimensions and twelve relationships.

- `OrderDateKey` is the active relationship to `DimDate`. `ShipDateKey`, `DeliveryDateKey` and `ReturnDateKey` are modelled as inactive role-playing relationships.
- `CityCoordinates`, a 24-row lookup of latitude and longitude, was merged into `DimGeography` in Power Query because the map visual needs explicit coordinates.
- `WaterfallSteps` is a disconnected table that supplies the step names and order for the gross-sales-to-margin bridge.
- All measures live in a dedicated `1_Calculations` table.

**Data quality notes.** `DimDate` runs into 2026 while the fact table stops at 2025, and the inactive ship, delivery and return keys are null for orders that never shipped or were never returned. Power BI turns those nulls into a blank row in the date table, so 2026 and the blank were filtered out of the year slicer rather than left as dead ends.

## Report walkthrough

### Page 1: Growth and Profitability

*How do gross sales turn into profit?*

![Growth and Profitability](screenshots/01_growth_and_profitability.png)

- **Trend** (Timeline PRO): net sales as bars and contribution margin % as a line, drillable from year down to day.
- **Contribution margin bridge** (Waterfall PRO): gross sales through discounts, refunds, shipping revenue, product cost, fulfillment and shipping cost, and marketing and payment cost, landing on contribution margin. Drills by department and category.
- **Category view** (Combo Bar PRO): net sales set against contribution margin for each category, so volume and profit can be compared side by side.
- **Brand tier** (Donut PRO): contribution margin split by Value, Core and Premium tiers, drilling into brand and product.

### Page 2: Customers and Markets

*Who buys, who stays, and where?*

![Customers and Markets](screenshots/02_customers_and_markets.png)

- **Retention by cohort age** (Bubble PRO): retention rate against months since first purchase, sized by active customers and coloured by lifecycle stage (Acquisition, Early repeat, Developing, Mature).
- **Customer segment** (Combo PRO): net sales and contribution margin by segment, drilling into loyalty tier.
- **Sales channel** (Combo Bar PRO): the same pairing by channel group and channel.
- **Market map** (Map PRO): net sales by city, with zoom moving from regions to countries to individual cities.

### Page 3: Operational Performance

*Where is value leaking?*

![Operational Performance](screenshots/03_operational_performance.png)

- **Promotions** (Combo PRO): net sales and contribution margin by promotion objective, drilling into campaign.
- **Lost sales** (Combo Bar PRO): estimated lost sales value by category and product.
- **Delivery** (Combo PRO): actual on-time rate against each market's expected rate, by region and country.
- **Returns** (Donut PRO): return loss by reason group and reason.

## How to use the report

- Click any bar, slice or map pin to drill into it, and use **Back** to step out.
- The year slicer filters every page and stays in sync as you move between them.
- **Reset** returns the current page to its default state, including any drill-downs.
- The **(i)** button opens a short usage guide over the page.

## Technical highlights

<details>
<summary><b>Selected DAX measures</b></summary>

```dax
// Order lines repeat OrderID, so orders are a distinct count, not a row count
Orders = DISTINCTCOUNT(FactOrderLine[OrderID])

Contribution Margin % =
DIVIDE([Contribution Margin], [Contribution Revenue])

// Drives the bridge chart from the disconnected WaterfallSteps table
Waterfall Value =
SWITCH(
    SELECTEDVALUE(WaterfallSteps[Step]),
    "Gross Sales", [Gross Sales],
    "Discounts", -[Discounts],
    "Refunds", -[Refunds],
    "Shipping Revenue", [Shipping Revenue],
    "Product Cost", -[Product Cost],
    "Fulfillment & Shipping Cost", -[Fulfillment & Shipping Cost],
    "Marketing & Payment Cost", -[Marketing & Payment Cost],
    "Contribution Margin", [Contribution Margin]
)

// Retention: customers active at each cohort age, relative to age 0
Cohort Base Customers =
CALCULATE([Active Customers], ALL(DimCohortAge), DimCohortAge[MonthsSinceFirstPurchase] = 0)

Retention Rate = DIVIDE([Active Customers], [Cohort Base Customers])

// On-time only means something for orders that were actually delivered
On-Time Delivery % =
CALCULATE(AVERAGE(FactOrderLine[OnTimeFlag]), FactOrderLine[OrderStatus] = "Delivered")

Expected On-Time Rate = AVERAGE(DimGeography[ExpectedOnTimeRate])

Return Rate =
DIVIDE(SUM(FactOrderLine[ReturnedQuantity]), SUM(FactOrderLine[FulfilledQuantity]))

// KPI context line under the Net Sales card, live with every filter
Net Sales Context = FORMAT([Net Sales % of Gross], "0%") & " of gross sales retained"
```

Before any visual was built, the bridge was reconciled in a check table: gross sales less discounts and refunds equals net sales, plus shipping revenue equals contribution revenue, less all variable costs equals contribution margin.

</details>

**Design decisions**

- **Brand.** A fictional retailer, Nordhaven, with a vector compass logo and a custom theme file. Navy marks totals, gold marks highlights, and coral is reserved for negative movement.
- **Layout.** The page background (navigation rail, header, card frames, shadows) was designed in PowerPoint at 1920 x 1080 and imported as the canvas background. Every visual sits on top with a transparent fill, so one background file serves all three pages.
- **Navigation.** A custom left rail with an active-page state, built from page-navigation buttons.
- **KPI cards.** Three layers: a label, the figure, and a DAX-driven sentence that cross-references a related real number instead of an invented trend, so it updates with the slicer.
- **Interactivity.** Twelve ZoomCharts Drill Down PRO visuals across the report, with Reset and Info buttons built from bookmarks and the Selection pane.

## Reflections and next iteration

After submission the report was validated for participation and came back with useful refinements. The three I am carrying into the next build:

1. Run a full alignment pass with Power BI's align and distribute tools on every page before submitting, instead of positioning by eye.
2. Turn on breadcrumbs for drillable ZoomCharts visuals so users always see where they are in a hierarchy.
3. Restyle default controls, such as the visuals' built-in help icons and the year buttons, so they belong to the same design system as the custom elements.

## Repository structure

```
nordhaven-ecommerce-profitability/
├── README.md
├── powerbi/
│   └── Nordhaven_Ecommerce_Profitability.pbix
├── screenshots/
│   ├── 01_growth_and_profitability.png
│   ├── 02_customers_and_markets.png
│   └── 03_operational_performance.png
└── assets/
    └── nordhaven_logo.png
```

## Tools

Power BI Desktop, DAX, Power Query, ZoomCharts Drill Down PRO visuals, PowerPoint (layout design), Excel (source data).

## Author

**Emeka Victor Prince**, data analyst in training at Attueyi Coding Academy (ACA), Nigeria.
[LinkedIn](INSERT_LINKEDIN_URL)

Dataset and challenge by [ZoomCharts](https://zoomcharts.com).
