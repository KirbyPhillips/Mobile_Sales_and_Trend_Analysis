# Mobile Sales and Trends Analysis

## Project Overview

In 2024, a mobile phone retailer operating across 4 countries (India, Turkey, Bangladesh, and Pakistan) lacked a centralised view of its sales performance. Key data such as revenue, product mix, customer demographics, and channel performance, existed in raw transactional formats, making it difficult to extract meaningful insights or support decision-making.

As a result, stakeholders were unable to answer critical business questions, such as:

* Which markets were driving the most revenue?
* What products performed best across different regions?
* Which sales channels were most effective?

This project was built to bridge that gap by transforming a partially modelled Excel dataset into a structured, end-to-end Power BI analytics solution. The goal was to enable clear visibility into performance, support data-driven decision-making, and uncover actionable insights across markets, products, and customer segments.


<img width="601" height="335" alt="{37C71874-F5A4-4873-9803-8197D5A909D1}" src="https://github.com/user-attachments/assets/ba96ac69-e171-435f-a845-181cb2288e2f" />


---

## Tools and Technologies

The following tools and technologies were used to complete this analysis:

| Tool | Purpose |
|---|---|
| Microsoft Excel | Dataset inspection and preliminary diagnosis |
| Power BI Desktop | Data modelling, DAX measures, and report building |
| Power Query | Data transformation and normalisation; Calendar table generation |
| DAX | 65 measures across 7 display folders |
| Figma | Wireframing and report background design |
| JSON | Custom Emerald Tide theme applied across all 26 visual types |
| Claude.ai | Structure report writing |

---

## The SCAN Framework

This analysis was planned and executed using the SCAN Framework, a personal structured methodology developed to bring clarity to every stage of the process.

| Step | Description |
|---|---|
| S - Scope the Situation | Defined the business problem and cost of inaction |
| C - Confirm the Core Metrics | Established the North Star, 3 Catalysts, and Indicators |
| A - Build the Architecture | Designed the data model and measure library |
| N - Narrate the Story | Applied CARE design principles to the report |

---

## Project Phases

A structured, end-to-end workflow was followed to transform raw transactional data into a scalable and insight-driven Power BI solution. The project spans data preparation, modelling, optimisation, and dashboard design, with each phase building toward actionable business insights.

### Phase 1: Data Preparation (Excel)

- Reviewed the raw `Mobile_Sales.xlsx` file across 4 sheets: Fact\_Sales, Dim\_Products, Dim\_Locations, and Data Dictionary
- Rebinned `Customer_Age_Group` from 6 bands to 4 equal 12-year bands: 18-29, 30-41, 42-53, 54-65
- Normalised `Fact_Sales` by removing 7 denormalised columns: Brand, Operating\_System, Color, Storage\_Size, Country, Latitude, Longitude
- Replaced GUID-based `Transaction_ID` with sequential integers 1 to 366
- Created composite `Product_Key` (P001 to P274) in both Dim\_Products and Fact\_Sales to enable a valid one-to-many relationship

**Data prep image:**

<img width="926" height="317" alt="{BFC24286-5D4C-4872-96A4-F830934EC253}" src="https://github.com/user-attachments/assets/62efa2ca-a88c-4ba0-8c3c-b01c100d9bc8" />

### Phase 2: Data Model Setup (Power BI)

- Loaded 3 tables into Power BI via Power Query, fixing a promoted headers issue on Dim\_Products
- Disabled auto-detect relationships for complete manual control
- Built a star schema with 3 active relationships: DIM_Products, DIM_Locations, and DIM_Calendar each connected one-to-many to FACT_Sales.
- Created DIM\_Calendar in Power Query with 11 columns covering all 366 days of 2024
- Marked DIM\_Calendar as the official date table

#### Data Model (Star Schema)

This is a logical star schema design illustrating the relationship between the central FACT_Sales table and supporting dimension tables, structured to enable scalable and efficient analytical reporting.

<img width="393" height="299" alt="{C95AED0F-D69A-4A69-A9BF-9D7724D8CC7D}" src="https://github.com/user-attachments/assets/d555194c-5135-4205-a1c2-83149616bf2a" />

![Data Model](assets/db.png) 

### Phase 3: Model Optimisation

- Created 65 DAX measures organised into 7 display folders in a dedicated `_Measures` table
- Standardised all column naming conventions to underscore format
- Hidden all foreign key and join columns from report view
- Added descriptions to all 79 model objects including tables, columns, and measures
- Built 3 user hierarchies: Date, Geography, and Product
- Corrected data types: Transaction\_ID changed to Text, date columns changed from DateTime to Date

#### Measures Organisation and Model Optimisation

<img width="209" height="150" alt="{82828FA6-3B62-4179-A2D7-79D704EAA229}" src="https://github.com/user-attachments/assets/7dcb22c8-2e9b-48cb-9039-5002c9eceec1" />

### Phase 4: Report Building (Power BI)

Built 6 report pages following the North Star to Catalyst to Indicator hierarchy:

| Page | Name | Role |
|---|---|---|
| 1 | Executive Overview | North Star |
| 2 | The 3 Catalysts | Catalysts |
| 3 | Product Performance | Product Indicators |
| 4 | Geographic Analysis | Geographic Indicators |
| 5 | Customer Insights | Customer Indicators |
| 6 | Channel and Payment | Channel Indicators |

### Phase 5: Design and Theme

- Designed all 6 report page backgrounds in Figma, exported as PNG images, and imported as canvas backgrounds in Power BI
- Applied the custom Emerald Tide JSON theme across all 26 visual types
- Applied CARE design principles throughout: Cohesion, Aesthetic, Rhythm, Emphasis

---

## Data Model

### Star Schema

```
FACT_Sales (366 rows, 13 columns)
    |
    |-- DIM_Products (274 rows, 6 columns)      [via Product_Key]
    |-- DIM_Locations (25 rows, 4 columns)      [via City]
    |-- DIM_Calendar (366 rows, 11 columns)     [via Transaction_Date]
```

### Tables

| Table | Rows | Columns | Description |
|---|---|---|---|
| FACT\_Sales | 366 | 13 | Central fact table — transactions, measures, and foreign keys |
| DIM\_Products | 274 | 6 | Product variants by model, brand, OS, storage, and colour |
| DIM\_Locations | 25 | 4 | City, country, latitude, and longitude |
| DIM\_Calendar | 366 | 11 | Date table built in Power Query |
| \_Measures | 0 | — | Dedicated measures table with 65 DAX measures |

### Relationships

| From | To | Column | Cardinality |
|---|---|---|---|
| DIM_Products | FACT_Sales | Product_Key | One to Many |
| DIM_Locations | FACT_Sales | City | One to Many |
| DIM_Calendar | FACT_Sales | Transaction_Date | One to Many |

---

## Measure Library

65 DAX measures organised across 6 display folders:

| Folder | Count | Key Measures |
|---|---|---|
| Core Measures | 5 | Total Revenue, Total Units Sold, Total Transactions, Average Selling Price, Average Units per Transaction |
| Time Intelligence | 23 | PM, MoM Change, MoM %, and YTD variants for all core measures, plus vs Last Month comparisons |
| Product Analysis | 6 | Revenue by Brand, Revenue by Model, Revenue by OS, Units by Storage, Units by Color, Avg Revenue per Brand |
| Customer Analysis | 6 | Revenue and Units by Age Group and Gender, Avg Selling Price by Age Group and Gender |
| Geographic Analysis | 5 | Revenue and Units by Country and City, Avg Selling Price by Country |
| Channel Analysis | 5 | Revenue, Units, and Transactions by Sales Channel and Payment Type |
| Conditional Formatting | 15 | CF Revenue by Brand, CF Bars, Marker Color, MoM Revenue, CF MoM Revenue, and supporting measures |

---

## Key Findings

| Finding | Detail |
|---|---|
| Total Revenue | 14,525,413 across 366 transactions in 2024 |
| Top Market | India at 6.97M, accounting for 48% of total revenue |
| Top Brand | Apple at 3.64M, followed by Samsung at 3.48M |
| Average Selling Price | 784.73 per device across all brands and markets |
| September Dip | The only month below 1M in revenue at 988K, driven by product mix not demand |
| Pakistan Pricing | Average selling price of 619, which is 27% below the overall average |
| Online Channel | Accounts for 62.3% of total revenue across 213 transactions |
| Age and Brand Correlation | Spending increases with age — 18-29 group averages 787, 54-65 group averages 831 |

---

## Recommendations

1. Prioritise India as the primary growth market — a 10% revenue increase would add approximately 697K to the top line
2. Increase availability of Apple and Samsung products — together they generate 49% of revenue from just 2 of 5 brands
3. Launch a targeted campaign in August and September to address the annual mid-year revenue dip
4. Conduct a strategic review of the Pakistan market, focusing on product range realignment and pricing optimisation
5. Continue investing in the Online channel — it generates the highest average units per transaction at 52.7
6. Develop targeted marketing for the 54 to 65 age segment — the highest value customer cohort at 831 per device

---

## Project Documentation

| Document | Description |
|---|---|
| Mobile\_Sales\_SCAN\_Framework.docx | Full SCAN Framework applied to this project |
| Mobile\_Sales\_Project\_Phases\_SCAN.docx | All 6 project phases with detailed decisions and outcomes |
| Mobile\_Sales\_Semantic\_Model.md | Complete data model documentation with DAX code and business logic |
| Mobile\_Sales\_Business\_Report\_v2.docx | 2-page business report with recommendations and business impact |
| Mobile\_Sales\_QA\_Report.docx | 9 business questions answered with data-driven responses |
| Mobile\_Sales\_Prompts.docx | 125 prompts used throughout the project organised by phase |

---

## Dataset

| Property | Detail |
|---|---|
| Source | Mobile\_Sales.xlsx |
| Period | January 1 to December 31, 2024 |
| Transactions | 366 |
| Brands | 5 (Apple, Samsung, Google, OnePlus, Xiaomi) |
| Models | 19 |
| Countries | 4 (India, Turkey, Bangladesh, Pakistan) |
| Cities | 25 |
| Currency | Native dataset unit — denomination unspecified |

---

## Author

**Kirby**
Data Analyst | Power BI Developer
[LinkedIn](https://www.linkedin.com/in/) | [Website](https://)

