# 📊 Tableau Interview Questions & Answers (Top 50)
### JSW Data Engineer Interview Preparation

---

## 🔵 SECTION 1: BASICS (Q1–Q10)

---

**Q1. What is Tableau?**

> Tableau is a data visualization and Business Intelligence tool that converts raw data into interactive charts, dashboards and reports — without any coding.

---

**Q2. What are the different Tableau products?**

> - **Tableau Desktop** — create and design reports
> - **Tableau Server** — share reports within organization
> - **Tableau Online** — cloud-based version
> - **Tableau Mobile** — view dashboards on mobile
> - **Tableau Public** — free version for public sharing

---

**Q3. What is the difference between Dimensions and Measures?**

**Dimensions** are categorical fields that describe your data — they are used for grouping, segmenting and labeling. They are shown as **Blue pills** in Tableau and are discrete in nature.

**Measures** are numerical fields that can be calculated and aggregated — like sum, average, min, max. They are shown as **Green pills** in Tableau and are continuous in nature.

| | Dimensions | Measures |
|---|---|---|
| Nature | Categorical / Qualitative | Numerical / Quantitative |
| Pill Color | Blue (Discrete) | Green (Continuous) |
| Example | Country, Region, Product Name, Date | Sales, Profit, Quantity, Revenue |
| Use | Grouping, filtering, creating headers | Aggregation — SUM, AVG, MIN, MAX |
| Aggregation | Not aggregated | Always aggregated in charts |
| Role in Chart | Defines rows, columns, headers | Defines values, sizes, colors |
| Can be filtered by | Specific values or categories | Numeric ranges |

> **Simple Example:**
> If you are analyzing sales data —
> - **"Which Region?"** → Dimension (East, West, North, South)
> - **"How much Sales?"** → Measure (50000, 80000, 120000)
>
> In a bar chart → Region goes on X-axis (Dimension), Sales goes on Y-axis (Measure)

---

**Q4. What are the different file types in Tableau?**

> - **.twb** — Workbook (no data, only layout)
> - **.twbx** — Packaged Workbook (data included, easy to share)
> - **.tds** — Data Source connection file
> - **.tdsx** — Packaged Data Source
> - **.hyper** — Data Extract file (fast querying)

---

**Q5. What is the difference between Live Connection and Extract?**

| | Live Connection | Extract |
|---|---|---|
| Data | Real-time from database | Snapshot saved locally |
| Speed | Slower (depends on DB) | Faster |
| Use | When fresh data needed | When performance matters |

---

**Q6. What data sources can Tableau connect to?**

> - Relational databases: MySQL, PostgreSQL, SQL Server
> - Cloud databases: Amazon Redshift, Snowflake, Google BigQuery
> - Files: Excel, CSV, JSON
> - Cloud storage: Amazon S3, Google Cloud
> - Web APIs and custom connectors

---

**Q7. What is a Worksheet, Dashboard, Story and Workbook?**

| | Definition |
|---|---|
| Worksheet | Single view with one chart or visualization |
| Dashboard | Multiple worksheets combined in one view |
| Story | Sequence of dashboards to tell a narrative |
| Workbook | File that contains all sheets, dashboards and data |

---

**Q8. What are the two types of data values in Tableau?**

> - **Discrete** (Blue pill) — individual, separate values like names, categories
> - **Continuous** (Green pill) — range of values like sales amounts, dates

---

**Q9. What is the Marks card in Tableau?**

> Marks card controls how data is displayed visually. It includes options like Color, Size, Label, Detail, Tooltip and Shape to customize the appearance of marks in a chart.

---

**Q10. What is Show Me in Tableau?**

> Show Me is a panel that suggests the best chart type based on the fields you have selected. It automatically recommends bar charts, line charts, scatter plots, etc.

---

## 🔵 SECTION 1B: TABLEAU THEORY (Q10B–Q10E)

---

**Q10B. How does Tableau work internally? (Architecture)**

> Tableau has 3 main layers:
> - **Data Layer** — connects to data sources (live or extract)
> - **Logic Layer** — processes calculations, filters and aggregations
> - **Presentation Layer** — renders charts and dashboards on screen
>
> When you drag a field to the view, Tableau generates a query, sends it to the data source, gets the result and renders the visual — all automatically.

---

**Q10C. What is the difference between Aggregated and Non-Aggregated data in Tableau?**

| | Aggregated | Non-Aggregated |
|---|---|---|
| What | Data is summarized (SUM, AVG) | Every individual row is shown |
| Example | Total Sales per Region | Each individual sale transaction |
| Default in Tableau | ✅ Yes — Tableau aggregates measures by default | Disaggregate via Analysis menu |
| Use | Dashboards, KPIs | Detailed row-level analysis |

---

**Q10D. What is a Mark in Tableau?**

> A Mark is every single data point displayed in a visualization — a bar, a circle, a line point, a square on a map.
>
> The **Marks card** controls how marks look:
> - **Color** — color marks by a dimension or measure
> - **Size** — size of each mark
> - **Label** — show value on top of mark
> - **Tooltip** — text shown on hover
> - **Shape** — circle, square, triangle etc.
> - **Detail** — adds more detail without changing the visual type

---

**Q10E. What is the difference between Row-level and Aggregate Calculations?**

> - **Row-level calculation** — calculated for each individual row before aggregation
>   Example: `[Sales] * 1.18` (adding GST to each row's sales)
>
> - **Aggregate calculation** — calculated after rows are grouped/aggregated
>   Example: `SUM([Sales]) / SUM([Orders])` (average order value at summary level)
>
> Row-level is slower on large data. Prefer aggregate calculations where possible.

---

## 🟡 SECTION 2: JOINS, FILTERS & CALCULATIONS (Q11–Q25)

---

**Q11. What are the different types of Joins in Tableau?**

> - **Inner Join** — only matching rows from both tables
> - **Left Join** — all rows from left + matching from right
> - **Right Join** — all rows from right + matching from left
> - **Full Outer Join** — all rows from both tables

---

**Q12. What is the difference between Join and Blending?**

| | Join | Blending |
|---|---|---|
| Data Source | Same data source | Different data sources |
| Performance | Fast | Slower |
| Result | Single merged table | Combined at query time |

---

**Q13. What is the Order of Filters in Tableau?**

> Extract Filter → Data Source Filter → Context Filter → Dimension Filter → Measure Filter → Table Calculation Filter

---

**Q14. What is a Context Filter?**

> A Context Filter is applied first before all other filters. It creates a temporary subset of data and other filters work on that subset only. It improves performance on large datasets.

---

**Q15. What are the different types of Filters in Tableau?**

> 1. **Extract Filter** — filters data during extract creation
> 2. **Data Source Filter** — applies to entire workbook
> 3. **Context Filter** — primary filter, applied first
> 4. **Dimension Filter** — filters categorical data
> 5. **Measure Filter** — filters numerical data
> 6. **Quick Filter** — interactive filter for end users
> 7. **Top N Filter** — shows top or bottom N records
> 8. **Relative Date Filter** — filters based on today's date dynamically
> 9. **Table Calculation Filter** — filters after table calculations

---

**Q16. What is a Calculated Field?**

> A Calculated Field is a new field created using formulas on existing fields.
> Example: `[Profit] / [Sales]` gives Profit Margin.
> It can use math, string, date and logical functions.

---

**Q17. What is LOD (Level of Detail) Expression?**

> LOD expressions let you calculate at a specific level of granularity, independent of the current view.
> - **FIXED** — calculates based on specified dimensions only
> - **INCLUDE** — adds extra dimensions to the calculation
> - **EXCLUDE** — removes specific dimensions from calculation

---

**Q18. What is the difference between COUNT and COUNTD?**

| | COUNT | COUNTD |
|---|---|---|
| What it does | Counts all rows including duplicates | Counts only unique/distinct values |
| Example | A, A, B, B, C → 5 | A, A, B, B, C → 3 |

---

**Q19. What are Aggregation Functions in Tableau?**

> SUM, AVG, MIN, MAX, COUNT, COUNTD, MEDIAN, STDEV, VAR

---

**Q20. What is a Quick Table Calculation?**

> Pre-built calculations applied directly on a measure without writing formulas manually.
> Examples: Running Total, Percent of Total, Moving Average, Rank, Year-over-Year Growth

---

**Q21. How do you calculate Profit Margin?**

```
[Profit] / [Sales]
```
> Format the field as percentage in Tableau.

---

**Q22. How do you classify Sales as High, Medium or Low?**

```
IF [Sales] > 100000 THEN "High"
ELSEIF [Sales] > 50000 THEN "Medium"
ELSE "Low"
END
```

---

**Q23. How do you calculate Year-over-Year (YoY) Growth?**

```
([Sales] - LOOKUP([Sales], -1)) / LOOKUP([Sales], -1)
```
> This compares current year sales with previous year.

---

**Q24. What is the LOOKUP function in Tableau?**

> LOOKUP returns the value of a field from a previous or next row.
> Syntax: `LOOKUP(expression, offset)`
> Example: `LOOKUP(SUM([Sales]), -1)` gives previous row's sales.

---

**Q25. How do you handle Null values in Tableau?**

> - `ZN([Field])` — replaces null with 0
> - `ISNULL([Field])` — checks if value is null, returns TRUE/FALSE
> - Filter shelf → uncheck Null option to remove null rows

---

## 🟠 SECTION 3: CHARTS & VISUALIZATION (Q26–Q36)

---

**Q26. Which chart to use for sales trend over time?**

> **Line Chart** — best for showing trends and changes over time.

---

**Q27. Which chart to use for comparing categories?**

> **Bar Chart** — best for comparing values across different categories.

---

**Q28. Which chart to use for market share comparison?**

> **Pie Chart** or **Stacked Bar Chart** — shows percentage contribution of each part.

---

**Q29. Which chart to use for data distribution of one variable?**

> **Histogram** — divides data into bins and shows frequency of each bin.

---

**Q30. Which chart to use for hierarchical data?**

> **TreeMap** — uses nested rectangles to show parent-child hierarchy like product categories and subcategories.

---

**Q31. Which chart to use for quartile distribution and outliers?**

> **Box Plot (Box and Whisker)** — shows median, Q1, Q3, min, max and outliers.

---

**Q32. What is a Dual Axis chart and how to create it?**

> A chart with two different measures on two separate Y-axes for comparison.
> Steps:
> 1. Drag first measure to Rows
> 2. Drag second measure also to Rows
> 3. Right click second measure → Dual Axis
> 4. Right click axis → Synchronize Axis

---

**Q33. What is a Scatter Plot used for?**

> To show the relationship or correlation between two continuous variables.
> Example: Sales vs Profit to find correlation.

---

**Q34. What is a Heat Map in Tableau?**

> A chart that uses color to represent values on a grid. Darker color = higher value.
> Useful for identifying patterns in large datasets.

---

**Q35. What is a Bullet Graph?**

> A chart used to track progress toward a target or goal. It shows actual value vs target value with performance ranges.

---

**Q36. What is a Gantt Chart used for?**

> To visualize tasks, their durations and dependencies over time. Commonly used in project management.

---

## 🟠 SECTION 2B: IMPORTANT THEORY (Q36B–Q36D)

---

**Q36B. What is the difference between Tableau's Row Shelf and Column Shelf?**

> - **Columns shelf** — controls what goes on the **X-axis** (horizontal)
> - **Rows shelf** — controls what goes on the **Y-axis** (vertical)
>
> Dragging a Dimension to Columns and a Measure to Rows creates a basic bar chart automatically.
> Swapping them rotates the chart from vertical to horizontal.

---

**Q36C. What is a Tooltip in Tableau and how to customize it?**

> A Tooltip is the small popup box that appears when you hover over a mark in a chart. It shows details about that data point.
>
> You can customize it by:
> - Click on Tooltip in the Marks card
> - Add or remove fields
> - Change the text and formatting
> - Even embed another worksheet inside a tooltip (Viz in Tooltip)

---

**Q36D. What is the difference between Hiding a field and Excluding it from a Filter?**

> - **Hiding** a field — removes it from the view visually but data is still used in background calculations
> - **Excluding in filter** — actually removes those rows from the data shown in the view
>
> Example: Hiding a null bar from a chart vs filtering out nulls from the data entirely — the totals and calculations will differ.

---

## 🔴 SECTION 4: DASHBOARD & PERFORMANCE (Q37–Q45)

---

**Q37. What is a Dashboard in Tableau?**

> A Dashboard is a collection of multiple worksheets, filters, images and interactive elements combined into a single view for monitoring and decision making.

---

**Q38. How do you create a Dashboard in Tableau?**

> 1. Click Dashboard tab at the bottom
> 2. Drag worksheets from the left panel onto the canvas
> 3. Add filters, titles and formatting
> 4. Add Actions for interactivity (filter, highlight, URL)

---

**Q39. What are Dashboard Actions?**

> Actions make dashboards interactive.
> - **Filter Action** — clicking one chart filters another
> - **Highlight Action** — highlights related data across charts
> - **URL Action** — opens a web page based on selection

---

**Q40. How do you optimize Dashboard performance?**

> - Use Extract instead of Live connection
> - Apply Data Source filters to reduce data
> - Use Context Filters for large datasets
> - Reduce number of sheets and marks on dashboard
> - Avoid complex row-level calculations
> - Hide unused worksheets from workbook

---

**Q41. What is Parameters in Tableau?**

> Parameters are dynamic input values that users can change. They work with filters, calculated fields and reference lines.
> Example: User selects Top 5 or Top 10 using a parameter.

---

**Q42. What is the difference between Sets and Groups?**

| | Sets | Groups |
|---|---|---|
| Definition | Subset based on condition or selection | Combines dimension members into one |
| Type | Dynamic or Fixed | Fixed |
| Example | Top 10 customers by sales | Combine "USA" and "Canada" as "North America" |

---

**Q43. What is a Bin in Tableau?**

> Bins divide continuous numeric data into equal intervals (buckets).
> Example: Age 0-10, 11-20, 21-30. Used for creating histograms.

---

**Q44. How do you add a Web Page to a Tableau Dashboard?**

> 1. Go to Objects panel on the left
> 2. Drag Web Page object onto the dashboard
> 3. Enter the URL in the dialog box
> 4. To make it dynamic, use Dashboard → Actions → Go to URL

---

**Q45. What is Performance Recording in Tableau?**

> A built-in feature that records and shows how long each part of a workbook takes to load. Helps identify performance bottlenecks.

---

## 🟢 SECTION 5: ADVANCED TOPICS (Q46–Q50)

---

**Q46. What is Data Blending in Tableau?**

> Data Blending combines data from two different data sources in a single visualization at query time. The primary source is queried first, then the secondary source data is brought in based on a common linking field.

---

**Q47. What is the WINDOW_AVG function?**

> Calculates the average of a measure within a defined window of rows.
> Syntax: `WINDOW_AVG([Sales], -2, 0)` — average of current and 2 previous rows.
> Used for moving averages.

---

**Q48. What is RANK in Tableau and its types?**

| Function | Description | Example (100, 90, 90, 80) |
|---|---|---|
| RANK | Same rank for ties, leaves gaps | 1, 2, 2, 4 |
| RANK_DENSE | Same rank for ties, no gaps | 1, 2, 2, 3 |
| RANK_MODIFIED | Averages ranks for tied values | 1, 2.5, 2.5, 4 |
| RANK_UNIQUE | Unique rank for every row | 1, 2, 3, 4 |

---

**Q49. What is the difference between Tableau Desktop and Tableau Server?**

| | Tableau Desktop | Tableau Server |
|---|---|---|
| Use | Create and design workbooks | Share and collaborate on workbooks |
| Access | Individual user on local machine | Multiple users via browser |
| Publishing | Publish to server | View and interact with published content |

---

**Q50. What is the difference between Reference Band and Bollinger Band?**

| | Reference Band | Bollinger Band |
|---|---|---|
| Definition | Highlights a fixed range on a chart | Shows volatility around a moving average |
| Use | Threshold or performance range | Financial/stock data analysis |
| Nature | Static or semi-dynamic | Fully dynamic |

---

## 💡 QUICK REVISION TIPS

> ✅ Always remember Filter Order: Extract → Data Source → Context → Dimension → Measure → Table Calc
>
> ✅ LOD types: FIXED, INCLUDE, EXCLUDE
>
> ✅ ZN() replaces null with 0
>
> ✅ Dual Axis = two measures, two Y-axes
>
> ✅ Extract is faster than Live connection
>
> ✅ Context Filter improves performance

---

---

## 🟣 SECTION 6: EXTRA IMPORTANT QUESTIONS (Q51–Q60)

---

**Q51. What is the difference between Discrete and Continuous in Tableau?**

| | Discrete (Blue) | Continuous (Green) |
|---|---|---|
| Nature | Individual separate values | Range of values |
| Example | Month names, Category | Sales amount, Temperature |
| In chart | Creates headers/labels | Creates an axis |

---

**Q52. What is Data Granularity in Tableau?**

> Granularity means the level of detail in your data.
> - **High granularity** = more detailed (each transaction separately)
> - **Low granularity** = summarized (monthly total)
>
> LOD expressions are used to control granularity in calculations.

---

**Q53. What is the difference between SUM and TOTAL in Tableau?**

> - **SUM([Sales])** — adds up Sales for each row in current view
> - **TOTAL(SUM([Sales]))** — gives the grand total of all Sales in the partition
>
> TOTAL is a table calculation while SUM is a basic aggregation.

---

**Q54. What is a Union in Tableau?**

> Union combines two or more tables with same structure by stacking rows on top of each other — like SQL UNION.
> Example: Combining Sales data of 2023 and 2024 from two separate sheets.

---

**Q55. What is Cross-Database Join in Tableau?**

> It allows joining tables from two different databases in a single data source.
> Example: Joining a PostgreSQL table with an Excel file in one connection.

---

**Q56. What is the difference between Sets and Filters?**

| | Sets | Filters |
|---|---|---|
| Purpose | Create In/Out groups for analysis | Show or hide data from view |
| Reusability | Can be used in calculated fields | Cannot be used in calculations |
| Dynamic | Can be dynamic (condition-based) | Applied only at view level |

---

**Q57. What is a Crosstab (Text Table) in Tableau?**

> A Crosstab is a table view showing exact data values in rows and columns — similar to Excel pivot table.
> Use it when exact numbers are more important than visual charts.

---

**Q58. What is the difference between Trend Line and Reference Line?**

| | Trend Line | Reference Line |
|---|---|---|
| Purpose | Shows direction/pattern of data over time | Marks a fixed or calculated benchmark value |
| Type | Linear, Exponential, Polynomial, etc. | Constant, Average, Median, Min, Max |
| Example | Sales growing upward over months | Average sales line across all bars |

---

**Q59. What is Forecasting in Tableau?**

> Tableau's built-in feature that predicts future values using Exponential Smoothing based on historical data.
>
> Steps: Analytics pane → Forecast → Show Forecast
>
> Shows predicted values with confidence intervals on a line chart.

---

**Q60. What are Table Calculations in Tableau?**

> Table Calculations are computations applied on the result of a query — they work on the data already shown in the view, not on the original database.
>
> Common examples:
> - Running Total
> - Percent of Total
> - Moving Average
> - Rank
> - Year-over-Year Growth

---

## 💡 FINAL QUICK REVISION

**Basics:**
> ✅ Dimensions = Blue pills = Categorical | Measures = Green pills = Numerical
> ✅ Discrete = individual values (headers) | Continuous = range of values (axis)
> ✅ Extract is faster than Live connection
> ✅ Marks card = Color, Size, Label, Tooltip, Shape, Detail

**Filters:**
> ✅ Filter Order: Extract → Data Source → Context → Dimension → Measure → Table Calc
> ✅ Context Filter = applied first, improves performance on large data
> ✅ ZN() replaces null with 0 | ISNULL() checks for null

**Calculations:**
> ✅ LOD types: FIXED, INCLUDE, EXCLUDE
> ✅ Row-level calc = per row before grouping | Aggregate calc = after grouping
> ✅ SUM([Sales]) = per view | TOTAL(SUM([Sales])) = grand total
> ✅ Calculated Field = custom formula | Quick Table Calc = pre-built (running total, rank, etc.)

**Charts:**
> ✅ Trend Line = pattern over time | Reference Line = fixed benchmark
> ✅ RANK (gaps) | RANK_DENSE (no gaps) | RANK_UNIQUE (all different)
> ✅ Dual Axis = two measures on two Y-axes → right click → Dual Axis

**Data Combining:**
> ✅ Union = stack rows (same structure) | Join = merge columns | Blend = different sources
> ✅ Cross-Database Join = join tables from two different databases

**Dashboard:**
> ✅ Dashboard Action = Filter / Highlight / URL
> ✅ Performance: Use Extract, Context Filter, hide unused sheets

---

*All the best Rajkishor! Crack the JSW interview! 🎯*
