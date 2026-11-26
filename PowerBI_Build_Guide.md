# Executive KPI Dashboard - Power BI Build Guide

## Files
| File | Use |
|---|---|
| csv/Raw_Sales.csv | 512 dirty rows (12 duplicates, mixed dates, wrong casing, missing values) |
| csv/Fact_Sales.csv | 491 rows, cleaned with pandas (reference answer) |
| csv/Dim_*.csv, Fact_Targets.csv | Product, Customer, Region, SalesRep, Date, monthly Targets |
| Executive_KPI_Dataset.xlsx | All sheets + Cleaning_Log + one Excel chart per sheet |
| PowerQuery_Queries.m.txt | M code for every table (Fact_Sales cleans Raw_Sales inside Power Query) |
| DAX_Measures.dax.txt | Columns and measures: CALCULATE/FILTER, context transition, YTD/MTD/QTD, YoY, dynamic |

## Steps (15 mins)
1. Copy the `csv` folder to e.g. `C:\PowerBI\csv\`.
2. Power BI Desktop > Transform data > Manage Parameters > New: `pDataFolder` (Text) = that path with trailing `\`.
3. For each `// QUERY:` block in PowerQuery_Queries.m.txt: New Source > Blank Query > Advanced Editor > paste > rename query. Create in this order: Dim_Product, Dim_SalesRep, Dim_Customer, Dim_Region, Dim_Date, Fact_Targets, Fact_Sales.
4. Close & Apply. Model view: build relationships (see header of DAX file), Dim_Date > Mark as date table.
5. Paste calculated columns, then measures into a `_Measures` table.
6. Create `Metric Selector`, `Period Selector`, `Top N` (What-if) tables for the dynamic page.

## Charts - one per file
| Table | Visual | Fields |
|---|---|---|
| Fact_Sales | Line chart | Axis Dim_Date[YearMonth], Values [Net Sales], [Sales PY] |
| Fact_Sales | Donut | Legend Channel, Values [Net Sales] |
| Fact_Sales | Cards | [Net Sales], [Gross Profit], [Profit Margin %], [Orders], [Avg Order Value], [YoY Growth %] |
| Dim_Date | Clustered column | Axis Quarter, Legend Year, Values [Net Sales]; plus [Sales YTD] line |
| Dim_Product | Bar | Axis Category, Values [Net Sales]; table of ProductName with [Top N Product Sales] |
| Dim_Customer | Pie/Donut + Table | Segment by [Net Sales]; CustomerName, Customer Tier, Customer Sales |
| Dim_Region | Filled map or Bar | Region, [Net Sales], [Region Share %] |
| Dim_SalesRep | Bar (Top 10) | RepName, [Net Sales] |
| Fact_Targets | Clustered column / Gauge | Region: [Net Sales] vs [Target Revenue]; Gauge [Achievement %] |

## Pages
1. **Executive Overview** - cards, monthly trend, region bar, KPI Status with conditional formatting (Field value = [KPI Color]).
2. **Time Intelligence** - MTD/QTD/YTD cards, YoY % column chart, Rolling 3M line.
3. **Dynamic** - Metric Selector slicer driving [Selected Metric], [Dynamic Title] as visual title.
4. **Context demo** - table showing [Net Sales], [Sales All Regions], [Region Share %], Customer Tier.
Slicers: Year, Region, Channel, Segment (sync across pages).
