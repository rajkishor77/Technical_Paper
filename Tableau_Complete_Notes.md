# 📊 Tableau — Complete Notes (Beginner to Advanced)

> **Roadmap:** Basics → Charts → Calculations → Dashboards → Projects → SQL → Advanced → Portfolio

---

## Table of Contents

1. [Introduction to Tableau](#1-introduction-to-tableau)
2. [Installation & Setup](#2-installation--setup)
3. [Data Connection](#3-data-connection)
4. [Tableau Interface](#4-tableau-interface)
5. [Data Modeling](#5-data-modeling)
6. [Data Cleaning](#6-data-cleaning)
7. [Data Types](#7-tableau-data-types)
8. [Dimensions & Measures](#8-dimensions--measures)
9. [Basic Charts](#9-basic-charts)
10. [Advanced Charts](#10-advanced-charts)
11. [Geographic Visualization](#11-geographic-visualization)
12. [Sorting & Filtering](#12-sorting--filtering)
13. [Groups, Sets & Hierarchies](#13-groups-sets--hierarchies)
14. [Calculated Fields](#14-calculated-fields)
15. [Table Calculations](#15-table-calculations)
16. [Parameters](#16-parameters)
17. [Level of Detail (LOD)](#17-level-of-detail-lod-expressions)
18. [Dashboard Creation](#18-dashboard-creation)
19. [Dashboard Actions](#19-dashboard-actions)
20. [Formatting & UI Design](#20-formatting--ui-design)
21. [Storytelling](#21-storytelling)
22. [Publishing & Sharing](#22-publishing--sharing)
23. [SQL + Tableau Integration](#23-sql--tableau-integration)
24. [Advanced Tableau Topics](#24-advanced-tableau-topics)
25. [Portfolio & Career Tips](#25-portfolio--career-tips)
26. [30 Interview Questions & Answers](#26-30-interview-questions--answers)

---

## 1. Introduction to Tableau

### What is Tableau?
- Tableau is a **visual analytics platform** used for data analysis and interactive dashboard creation
- Founded in **2003**, acquired by **Salesforce in June 2019**
- No coding required — purely drag-and-drop based
- Used by data analysts, business analysts, and BI developers worldwide

### Why Use Tableau?
- Fastest visualization tool available in the market
- Can handle millions of rows of data without performance issues
- Connects to **50+ data sources** (Excel, SQL, Cloud, APIs, etc.)
- Creates interactive dashboards that non-technical people can also use
- Strong community support and documentation

### Tableau Products

| Product | Description | Cost |
|---|---|---|
| **Tableau Desktop** | Full-featured tool for building dashboards locally | Paid |
| **Tableau Public** | Free version — dashboards published publicly on web | Free |
| **Tableau Server** | Host and share dashboards within an organization | Paid |
| **Tableau Cloud** | Cloud-hosted version of Tableau Server (SaaS) | Paid |
| **Tableau Prep** | Data cleaning and preparation tool | Paid |

### Advantages of Tableau
- ✅ Very fast backend calculation engine
- ✅ Interactive and visually rich dashboards
- ✅ No manual calculation needed — Tableau handles aggregations
- ✅ Handles large datasets without impacting performance
- ✅ Wide variety of chart types available
- ✅ Easy sharing via Tableau Public or Tableau Server
- ✅ Strong integration with SQL, Python, R

### Disadvantages of Tableau
- ❌ High cost for Desktop and Server licenses (~$70–$75/month)
- ❌ Parameters are static — single value only, need manual update when data changes
- ❌ Limited data preprocessing — not a replacement for ETL tools
- ❌ Steep learning curve for LOD and advanced calculations
- ❌ Large workbooks can be slow on Tableau Server

### Tableau Architecture
- **Data Layer** — connects to data sources (files, databases, cloud)
- **Logic Layer** — relationships, joins, calculated fields
- **Presentation Layer** — dashboards, stories, sheets shown to end user
- **Sharing Layer** — Tableau Server / Cloud / Public for distribution

---

## 2. Installation & Setup

### Steps to Install Tableau Public (Free)
1. Go to `public.tableau.com` → click **Download Tableau Desktop**
2. Create a free Tableau Public account (email required)
3. Download and run the installer (Windows or macOS)
4. Launch Tableau → sign in with your Public account
5. Connect to a data source and start building!

### Tableau Desktop vs Tableau Public

| Feature | Tableau Desktop | Tableau Public |
|---|---|---|
| Price | Paid (~$70/month) | Free |
| Save Locally | ✅ Yes (.twb / .twbx) | ❌ No (cloud only) |
| Private Dashboards | ✅ Yes | ❌ No (all public) |
| Data Source Support | All 50+ sources | Limited |
| Row Limit (Extract) | Unlimited | 15 million rows |
| Best For | Professional/enterprise use | Learning, portfolio |

> 💡 **Tip:** For learning purposes, Tableau Public is more than enough. Use it to build your portfolio.

---

## 3. Data Connection

### File-Based Sources
- Microsoft Excel (`.xlsx`, `.xls`)
- CSV / Text files (`.csv`, `.tsv`, `.txt`)
- JSON files
- PDF files
- Spatial files (GeoJSON, Shapefile, KML)
- Google Sheets

### Database Sources
- MySQL, PostgreSQL, SQL Server, Oracle
- Google BigQuery, Amazon Redshift, Snowflake
- SAP HANA, Teradata, IBM DB2
- MongoDB (via connector)

### Live vs Extract Connection

| | Live Connection | Extract (.hyper) |
|---|---|---|
| Data | Real-time from source | Snapshot saved locally |
| Speed | Slower (DB dependent) | Faster (optimized columnar engine) |
| Best For | Real-time dashboards | Large datasets, offline work |
| Refresh | Auto (always current) | Manual or scheduled |
| File Type | N/A | `.hyper` file |

> 💡 **Tip:** Use **Extract** for better performance. Use **Live** only when real-time data is a strict requirement.

---

## 4. Tableau Interface

### Key Areas of the Interface

- **Start Page** — connect to data, open recent workbooks, sample data
- **Data Source Tab** — configure connections, joins, unions, data preview
- **Worksheet** — single canvas for creating one chart or view
- **Dashboard** — combine multiple worksheets into one unified view
- **Story** — a sequence of dashboards/sheets for presentation

### Important Panels

| Panel | Location | Purpose |
|---|---|---|
| **Data Pane** | Left | Lists all fields — Dimensions (blue) + Measures (green) |
| **Analytics Pane** | Left (tab beside Data) | Add trend lines, forecasts, reference lines, clusters |
| **Marks Card** | Center-left | Control Color, Size, Label, Detail, Tooltip, Shape |
| **Show Me** | Top-right | Suggests chart types based on selected fields |
| **Rows & Columns Shelf** | Top-center | Main area to drag fields and build the chart |
| **Filters Shelf** | Top-left of canvas | Add and manage filters |
| **Pages Shelf** | Left of canvas | Create animated/paginated views |

### Toolbar Icons (Key ones)
- **Undo / Redo** — Ctrl+Z / Ctrl+Y
- **Clear Sheet** — clears all fields from current view
- **Swap Rows & Columns** — swaps axis
- **Sort Ascending / Descending** — sort current view
- **Show Me** — open/close chart type panel

---

## 5. Data Modeling

### Joins
Joins combine rows from two or more tables based on a **common key field**.

| Join Type | Returns |
|---|---|
| **Inner Join** | Only matching rows from BOTH tables |
| **Left Join** | All rows from LEFT table + matching rows from right |
| **Right Join** | All rows from RIGHT table + matching rows from left |
| **Full Outer Join** | All rows from BOTH tables (nulls where no match) |

> ⚠️ Joins can cause **data duplication** when one-to-many relationships exist. Be careful.

### Relationships (Tableau 2020.2+)
- The **modern, recommended** way to combine tables in Tableau
- Unlike joins, relationships work at **query time** — they don't pre-join the data
- Avoids row duplication issues that Joins create
- Each table retains its own **level of detail (granularity)**
- Defined by linking fields (like foreign keys in SQL)
- **Best Practice:** Always prefer Relationships over Joins unless you have a specific reason

### Union
- Stacks rows from multiple tables **vertically** (like SQL UNION ALL)
- Tables must have the **same column structure**
- Common use: combining monthly files — Jan + Feb + Mar into one dataset
- **Wildcard Union** — auto-includes new files matching a pattern

### Data Blending
- Joins data from **two different data sources** at the visualization level
- One source = **Primary** (blue checkmark), other = **Secondary** (orange link icon)
- Blending happens at the **view level**, not at the database level
- Less efficient and flexible than Relationships/Joins
- **Use when:** you cannot combine data at the source level (different databases)

---

## 6. Data Cleaning

> Done in the **Data Source tab** before building visualizations.

### Common Cleaning Operations

- **Rename Columns** — double-click column header in Data Source view
- **Change Data Types** — click the icon beside field name (Abc = string, # = number, 📅 = date)
- **Hide Fields** — right-click → Hide (hides from Data Pane, data still in source)
- **Split** — splits one column into multiple (e.g., "John Smith" → "John" + "Smith")
  - Right-click field → Transform → Split
- **Custom Split** — define your own delimiter and number of splits
- **Pivot** — converts wide-format data to long-format (columns become rows)
  - Select columns → right-click → Pivot
- **Data Interpreter** — Tableau auto-detects and cleans messy Excel formatting (merged cells, extra headers)
- **Remove Nulls** — use Filters or calculated fields: `IFNULL([Field], 0)` or `ZN([Field])`

### Handling Null Values
```
IFNULL([Sales], 0)        → replaces null with 0
ZN([Sales])               → shortcut for IFNULL([Sales], 0)  
ISNULL([Sales])           → returns TRUE if null
```

---

## 7. Tableau Data Types

| Data Type | Icon | Examples |
|---|---|---|
| **String (Text)** | `Abc` | Product Name, City, Customer ID |
| **Number (Integer)** | `#` | Quantity, Year |
| **Number (Decimal)** | `#` | Sales, Profit, Discount |
| **Boolean** | `T/F` | Is Returned? (True/False) |
| **Date** | 📅 | Order Date, Ship Date |
| **Date & Time** | 🕐 | Login Timestamp, Transaction Time |
| **Geographic** | 🌐 | Country, State, ZIP Code, Latitude |

> 💡 To change a data type: click the icon beside the field name in the Data Source tab or Data Pane.

---

## 8. Dimensions & Measures

### Dimensions
- **Categorical / Qualitative** fields — used to slice and segment data
- Shown in **BLUE** in the Data Pane
- Usually **Discrete** — create headers and labels in the view
- Examples: `Category`, `Region`, `Product Name`, `Customer ID`, `Country`
- Typically placed on: **Rows, Columns, Color, Filters**

### Measures
- **Numeric / Quantitative** fields — used for calculations and aggregations
- Shown in **GREEN** in the Data Pane
- Usually **Continuous** — create axes with a range
- Examples: `Sales`, `Profit`, `Quantity`, `Discount`
- Tableau automatically aggregates measures (SUM by default)

### Continuous vs Discrete

| | Discrete (Blue Pill) | Continuous (Green Pill) |
|---|---|---|
| Creates | Headers / Labels | Axis with a numeric range |
| Example | Month names (Jan, Feb...) | Sales axis (0 to 1,000,000) |
| Visual | Separated columns/rows | Connected line or bar |
| Switch | Right-click field → Convert to Discrete/Continuous |

### Common Aggregations
Right-click a measure → **Measure** → choose:

| Aggregation | Description |
|---|---|
| `SUM` | Total of all values (default) |
| `AVG` | Average value |
| `MIN` / `MAX` | Minimum / Maximum value |
| `COUNT` | Count of rows |
| `COUNTD` | Count of distinct (unique) values |
| `MEDIAN` | Middle value |
| `STDEV` | Standard deviation |

---

## 9. Basic Charts

| Chart Type | Best Used For | Minimum Fields Required |
|---|---|---|
| **Bar Chart** | Comparing categories side-by-side | 1 Dimension + 1 Measure |
| **Line Chart** | Trends over time | 1 Date + 1 Measure |
| **Pie Chart** | Part-to-whole (keep ≤ 6 slices) | 1 Dimension + 1 Measure |
| **Area Chart** | Cumulative trends, stacked comparisons | 1 Date + 1 Measure |

### How to Build Any Chart — Quick Steps
1. Drag a **Dimension** to Columns shelf
2. Drag a **Measure** to Rows shelf
3. Tableau auto-creates a bar chart
4. Use **Show Me** panel to switch chart types
5. Drag fields to **Marks Card** (Color, Size, Label) to enhance

> 💡 **Show Me Tip:** Select your fields first (Ctrl+Click to multi-select), then click any chart in Show Me — Tableau auto-configures Rows/Columns for you.

---

## 10. Advanced Charts

| Chart | Use Case | Key Setup |
|---|---|---|
| **Scatter Plot** | Correlation between 2 measures | 1 Measure on Rows + 1 Measure on Columns |
| **Histogram** | Distribution of values using bins | 1 Measure converted to Bins |
| **Heat Map** | Magnitude across 2 dimensions using color | 2 Dimensions + 1 Measure → Color |
| **Tree Map** | Hierarchical part-to-whole | 1+ Dimensions + 1-2 Measures (Size + Color) |
| **Bubble Chart** | 3 measures: X, Y position, bubble size | 2 Measures + 1 Measure for Size |
| **Box Plot** | Distribution, quartiles, outliers | 1 Dimension + 1 Measure |
| **Gantt Chart** | Project timelines, task durations | Date on Columns + Dimension on Rows + Duration as Size |
| **Waterfall Chart** | Running total with pos/neg contributions | Measure + Running Total table calc |
| **Funnel Chart** | Sales/marketing pipeline stages | 1 Dimension + 1 Measure → sorted descending |
| **Dual Axis Chart** | Two different measures on same chart | 2 Measures on Rows → right-click → Dual Axis |
| **Word Cloud** | Text frequency visualization | 1 Text Dimension + 1 Measure for size |

### Creating a Dual Axis Chart (Step-by-Step)
1. Drag **first measure** to Rows shelf
2. Drag **second measure** to Rows shelf (placed next to first)
3. Right-click the **second measure pill** → **Dual Axis**
4. Right-click the **right-side axis** → **Synchronize Axis** (if scales need to match)
5. Click each **Marks card** (Measure 1 / Measure 2) to set different chart types (e.g., Bar + Line)

### Creating a Histogram (Step-by-Step)
1. Right-click a Measure (e.g., `Sales`) → **Create** → **Bins**
2. Set the bin size (e.g., 100 = groups of 100)
3. Drag the **[Sales (bin)]** field to Columns
4. Drag `Sales` or `CNT(Sales)` to Rows
5. Change mark type to **Bar**

---

## 11. Geographic Visualization

### How Maps Work in Tableau
- Tableau has **built-in geographic recognition** for Countries, States, Cities, ZIP codes
- When Tableau detects a geographic field, it auto-assigns a **Geographic Role**
- Always assign the correct role: right-click field → **Geographic Role** → choose type

### Map Types

| Map Type | Description | Best For |
|---|---|---|
| **Filled Map (Choropleth)** | Shades entire regions with color | Country/State level metrics |
| **Symbol Map** | Circles/shapes plotted on map | City-level or point-level data |
| **Density Map** | Shows concentration of data points | Large volume point data |
| **Custom Map** | Import your own background map | Retail floor plans, country maps |

### Creating a Basic Map
1. Double-click a **geographic field** (Country, State, etc.) → Tableau auto-generates a map
2. Drag a **Measure** to Color on the Marks Card → fills map with color gradient
3. Change mark type to **Filled Map** or **Map** (Symbol) in Marks Card

### Geographic Roles Available
- Country/Region, State/Province, City, ZIP Code/Postal Code
- County, CBSA/MSA, Congressional District
- Latitude, Longitude (for custom point mapping)

> 💡 **Latitude/Longitude Tip:** If your data has lat/long columns, right-click each → Geographic Role → Latitude/Longitude → double-click both to create a symbol map instantly.

---

## 12. Sorting & Filtering

### Filter Order of Operations (Top → Bottom priority)

| Order | Filter Type | Notes |
|---|---|---|
| 1st | **Extract Filter** | Applied when creating the extract |
| 2nd | **Data Source Filter** | Applied to all sheets in the workbook |
| 3rd | **Context Filter** | Creates a temp subset; all other filters run within it |
| 4th | **Dimension Filters** | Filters by category/text values |
| 5th | **Measure Filters** | Filters by aggregated numeric values |
| 6th | **Table Calculation Filter** | Applied last — after all calculations are done |

> ⚠️ **Important:** LOD (FIXED) calculations ignore Dimension Filters but **respect Context Filters**.

### Types of Filters

| Filter | How to Create | Use Case |
|---|---|---|
| **Quick Filter** | Right-click field in view → Show Filter | Show interactive dropdown/slider on dashboard |
| **Context Filter** | Right-click filter → Add to Context | Make Top N filters work correctly; create subsets |
| **Top N Filter** | Filter pane → Top tab → By field | Show only top/bottom N items |
| **Conditional Filter** | Filter pane → Condition tab | Filter based on formula (e.g., SUM(Sales) > 10000) |
| **Wildcard Filter** | Filter pane → Wildcard tab | Filter text fields containing/starting with a string |

### Context Filter — Why It's Important
- A Context Filter creates a **temporary sub-table** from your data
- All other filters, LOD, and Top N calculations then run **within** this sub-table
- **Without Context Filter:** Top 5 Customers by Region might not work correctly (shows global Top 5, not per-region Top 5)
- **With Context Filter on Region:** Each region gets its own Top 5 customers

### Sorting Options
- **Toolbar sort button** — quick ascending/descending on any axis
- **Right-click field** → Sort → By Field (choose measure + aggregation)
- **Manual sort** — drag members in the sort dialog to custom order
- **Data Source order** — keeps original order from the data

---

## 13. Groups, Sets & Hierarchies

### Groups
- Manually combine dimension members into a new, single category
- **Example:** Group "North" + "Northeast" → "Northern Region"
- **Create:** Select multiple members in view → right-click → **Group**
- Groups are shown with a 📎 icon in the Data Pane
- Can be edited anytime: right-click group field → Edit Group
- Good for: fixing spelling errors, combining similar categories

### Sets
- A **dynamic subset** of data based on a condition or manual selection
- Creates an **In/Out** classification for each member
- **Create:** Right-click a Dimension → **Create Set** → define condition
- **Types:**
  - **Fixed Sets** — manually selected members
  - **Computed Sets** — based on a condition (e.g., Sales > 50000)
  - **Combined Sets** — union/intersection of two sets

| | Groups | Sets |
|---|---|---|
| Type | Static grouping | Dynamic In/Out membership |
| Based on | Manual selection | Condition or manual |
| Output | New combined category | True/False membership |
| Use in Calculations | Not directly | Yes — `IN [Set Name]` |

### Hierarchies
- Organize related fields in a **drill-down structure**
- **Example:** `[Country]` → `[State]` → `[City]` → `[ZIP Code]`
- **Create:** Drag one field **onto another** in Data Pane → give the hierarchy a name
- In the view, use **+** to drill down, **−** to drill back up

### Bins
- Groups a continuous measure into **equal-width buckets**
- **Example:** Sales values grouped into bins of $500 each
- **Create:** Right-click Measure → **Create Bins** → set bin size
- Commonly used for creating Histograms

---

## 14. Calculated Fields

> **Create:** Right-click anywhere in Data Pane → **Create Calculated Field**
> Or go to **Analysis menu → Create Calculated Field**

### IF / ELSEIF / ELSE
```
IF [Sales] > 50000 THEN "High"
ELSEIF [Sales] > 20000 THEN "Medium"
ELSE "Low"
END
```

### CASE Statement
```
CASE [Region]
  WHEN "East"  THEN "Eastern Zone"
  WHEN "West"  THEN "Western Zone"
  WHEN "South" THEN "Southern Zone"
  ELSE "Other"
END
```

### String Functions

| Function | Syntax | Description |
|---|---|---|
| Length | `LEN([Name])` | Number of characters |
| Left/Right | `LEFT([Name], 3)` | First 3 characters |
| Mid | `MID([Name], 2, 4)` | 4 chars starting from position 2 |
| Upper/Lower | `UPPER([Name])` | Convert to uppercase/lowercase |
| Trim | `TRIM([Name])` | Remove leading/trailing spaces |
| Replace | `REPLACE([Name], "old", "new")` | Replace text |
| Contains | `CONTAINS([Name], "abc")` | Returns TRUE if found |
| Find | `FIND([Name], "abc")` | Position of first occurrence |
| Split | `SPLIT([Name], "-", 1)` | Split by delimiter, return Nth part |

### Date Functions

| Function | Syntax | Description |
|---|---|---|
| Today | `TODAY()` | Current date |
| Now | `NOW()` | Current date and time |
| Date Add | `DATEADD('month', 3, [Date])` | Add 3 months to date |
| Date Diff | `DATEDIFF('day', [Start], [End])` | Days between two dates |
| Date Part | `DATEPART('year', [Date])` | Extract year as number |
| Date Name | `DATENAME('month', [Date])` | Month name as string (e.g., "January") |
| Date Trunc | `DATETRUNC('month', [Date])` | Truncate to first of the month |

### Aggregate Functions in Calculated Fields
```
SUM([Sales])                    → Total sales
AVG([Profit])                   → Average profit
MAX([Order Date])               → Latest order date
MIN([Sales])                    → Lowest sale value
COUNTD([Customer ID])           → Count of unique customers
```

### Logical / Null Functions
```
ISNULL([Field])                 → TRUE if field is null
IFNULL([Field], 0)              → Replace null with 0
ZN([Field])                     → Shortcut for IFNULL([Field], 0)
IIF([Sales] > 0, "Positive", "Negative")   → Inline IF
```

### Profit Ratio Example (Real-World)
```
SUM([Profit]) / SUM([Sales])
```

---

## 15. Table Calculations

- Calculations performed on the **result set already in the view** (not raw data)
- Run **after** the query returns results — so they see aggregated values
- **Add Quick Table Calc:** Right-click a measure in the view → **Quick Table Calculation**

### Types of Table Calculations

| Calculation | What it Does | Common Use |
|---|---|---|
| **Running Total** | Cumulative sum up to current row | Year-to-date sales |
| **Moving Average** | Average over N previous periods | Smooth out weekly spikes |
| **Percent of Total** | Each value as % of grand total | Market share by region |
| **Percent Difference** | % change from one period to next | Month-over-month growth |
| **Rank** | Rank each value (1 = highest) | Leaderboard, top performers |
| **Percentile** | Rank as percentile | Statistical distribution |
| **Index** | Sequential number for each row | Row numbering |

### Compute Using (Very Important!)
- After adding a table calc → right-click → **Edit Table Calculation** → set **Compute Using**
- Options: `Table (Across)`, `Table (Down)`, `Pane (Across)`, `Pane (Down)`, `Cell`, or a specific dimension
- **Most Common Mistake:** Wrong Compute Using setting = wrong results

### Custom Table Calculation Syntax
```
WINDOW_SUM(SUM([Sales]))          → Sum across entire window
WINDOW_AVG(SUM([Sales]))          → Average across window
WINDOW_MAX(SUM([Sales]))          → Max value in window
RUNNING_SUM(SUM([Sales]))         → Running/cumulative total
RANK(SUM([Sales]))                → Rank value in partition
INDEX()                           → Row index number
SIZE()                            → Total number of rows in partition
```

---

## 16. Parameters

- Parameters are **dynamic input controls** users can interact with on a dashboard
- They store a **single value** that can be used in calculated fields, filters, and reference lines
- **Create:** Right-click anywhere in Data Pane → **Create Parameter**

### Parameter Properties to Set
- **Name** — what it's called
- **Data Type** — Integer, Float, String, Boolean, Date, Date & Time
- **Current Value** — default starting value
- **Allowable Values** — All / List / Range

### How to Use a Parameter (Step-by-Step)
1. Create a Parameter (e.g., `Top N`, type = Integer, range = 1 to 20)
2. Right-click it → **Show Parameter** (shows slider/input on sheet)
3. Create a Calculated Field referencing it:
   ```
   RANK(SUM([Sales])) <= [Top N]
   ```
4. Drag this calculated field to **Filters** → set to **True**
5. Now user can change Top N slider → view updates dynamically!

### Common Parameter Use Cases

| Use Case | Setup |
|---|---|
| **Dynamic Top N Filter** | Integer param → Used in RANK() calc → Filter to TRUE |
| **Switch Between Measures** | String param with list (Sales/Profit/Qty) → CASE in calc |
| **What-If Analysis** | Float param for growth rate → Multiply with Sales |
| **Dynamic Reference Line** | Create param → Add Reference Line → set to param value |
| **Date Range Control** | Date param → Used in date filter calculated field |

---

## 17. Level of Detail (LOD) Expressions

> **Most important topic for interviews and senior analyst roles.**

LOD expressions let you control the **granularity of a calculation independently** of what dimensions are in the view.

### Syntax
```
{ TYPE [Dimension] : AGGREGATE(Measure) }
```

### Three Types of LOD

| LOD Type | Syntax | Behavior |
|---|---|---|
| **FIXED** | `{FIXED [Dim] : SUM([Sales])}` | Calculates at specified level — **ignores view dimensions** |
| **INCLUDE** | `{INCLUDE [Dim] : SUM([Sales])}` | Adds extra dimension **to** view's granularity |
| **EXCLUDE** | `{EXCLUDE [Dim] : SUM([Sales])}` | Removes a dimension **from** view's granularity |

### FIXED Examples
```
{FIXED [Customer ID] : SUM([Sales])}
→ Total sales per customer (even if view is at Order level)

{FIXED [Region] : AVG([Sales])}
→ Average sales per region (usable in any view)

{FIXED [Year] : MIN([Order Date])}
→ First order date of each year
```

### INCLUDE Example
```
{INCLUDE [Order ID] : SUM([Sales])}
→ View is at Customer level, but this calc goes down to Order level first, then aggregates up
→ Useful for: Average order size per customer
```

### EXCLUDE Example
```
{EXCLUDE [Month] : SUM([Sales])}
→ View has Month in it, but this calc ignores Month → shows annual total in every month row
→ Useful for: % of annual total for each month
```

### LOD vs Filters — Critical Rule
- **FIXED LOD** ignores all Dimension Filters in the view
- FIXED LOD **does respect** Context Filters
- **Solution:** If your LOD should respect a filter → right-click that filter → **Add to Context**

### Practical Use Cases
- Customer's first purchase date: `{FIXED [Customer ID] : MIN([Order Date])}`
- Days since first purchase: `DATEDIFF('day', {FIXED [Customer ID] : MIN([Order Date])}, TODAY())`
- Cohort analysis (new vs returning customers)
- % contribution of each product to its category total

---

## 18. Dashboard Creation

### Steps to Create a Dashboard
1. Click the **New Dashboard** tab (bottom of screen)
2. Set **Size** — Fixed (px), Automatic, or Range
3. Drag **sheets** from left panel onto the dashboard canvas
4. Add **objects** (Text, Image, Blank, Web Page) from bottom-left
5. Add **Actions** for interactivity (Filter, Highlight, URL)
6. Preview for different devices using **Device Preview**

### Layout Types

| Layout | Description | Use When |
|---|---|---|
| **Tiled** | Grid-based, no overlap | Structured professional dashboards |
| **Floating** | Free-position, can overlap | Creative/custom designs, overlays |

### Dashboard Objects

| Object | Icon | Use |
|---|---|---|
| Horizontal Container | ⬛ | Place sheets side-by-side |
| Vertical Container | ⬛ | Stack sheets top-to-bottom |
| Text Box | 📝 | Titles, descriptions, footnotes |
| Image | 🖼️ | Logos, background images |
| Blank | ⬜ | Add spacing / padding |
| Web Page | 🌐 | Embed external URLs |
| Navigation | 🔗 | Button to navigate between dashboards |
| Extension | 🔌 | Third-party add-on components |

### Dashboard Design Best Practices
- Use **max 2–3 colors** — one accent for KPIs, neutral for backgrounds
- Add **KPI cards** at the top — quick summary of key numbers
- Use **containers** for consistent alignment
- Add a **title** and data source note at the bottom
- Keep it **clutter-free** — every visual should answer a specific question
- Test on both desktop and mobile using Device Preview

---

## 19. Dashboard Actions

> Go to: **Dashboard menu → Actions → Add Action**

| Action Type | What It Does | Trigger Options |
|---|---|---|
| **Filter Action** | Click a mark → filters another sheet | Hover / Select / Menu |
| **Highlight Action** | Click a mark → highlights related marks | Hover / Select / Menu |
| **URL Action** | Click → opens a URL in browser | Hover / Select / Menu |
| **Parameter Action** | Click → changes a parameter value | Select / Menu |
| **Set Action** | Click → adds/removes items from a Set | Select / Deselect |

### Setting Up a Filter Action (Most Common)
1. Dashboard → Actions → Add Action → **Filter**
2. **Source Sheet:** which sheet triggers the filter
3. **Target Sheet:** which sheet(s) get filtered
4. **Run on:** Select / Hover / Menu (right-click)
5. **Clearing selection will:** Keep filtered values / Show all values / Exclude all values

> 💡 **Best Practice:** Use "Show all values" on clearing selection — so the dashboard resets when nothing is selected.

---

## 20. Formatting & UI Design

### Formatting Options

| What to Format | Where to Access |
|---|---|
| Fonts, Colors, Borders | Format menu → Font / Shading / Borders / Lines |
| Axis formatting | Right-click axis → Format Axis |
| Tooltip customization | Worksheet → Tooltip (click Tooltip in Marks Card) |
| Number formatting | Right-click a measure → Format → Number |
| Title formatting | Double-click any title → edit text, font, color |
| Null value display | Format → Special Values |

### Tooltip Tips
- Click **Tooltip** in Marks Card → edit with rich text + dynamic fields
- **Viz in Tooltip** — embed a mini-chart inside tooltip:
  - Create a separate detail sheet → In tooltip editor → click Insert → Sheet

### Key Formatting Best Practices
- Remove gridlines for cleaner look: Format → Lines → set to None
- Use **consistent fonts** — one font family for entire dashboard
- Number format: Use abbreviations (1.2M instead of 1,200,000) for KPI cards
- Remove axes titles when chart title already explains it
- Use **white or light gray background** for containers/dashboard

---

## 21. Storytelling

### What is a Tableau Story?
- A Story is a **sequence of Story Points** shown like a slide presentation
- Each Story Point = a snapshot of a dashboard or worksheet + a caption
- Used for: stakeholder presentations, monthly reporting, guided analytics

### Creating a Story
1. Click **New Story** tab at the bottom
2. Drag sheets or dashboards from the left panel into **Story Points**
3. Add a **caption** at the top of each Story Point
4. Use the **navigator** (dots or arrows) for presentation flow
5. File → Presentation Mode for fullscreen slideshow view

### Story Point Tips
- Each Story Point can have different **filters applied** (to highlight specific insights)
- You can **annotate** charts within a Story Point
- Use **blank Story Points** as transition slides with just a big text caption

---

## 22. Publishing & Sharing

### File Formats

| Format | Extension | Description |
|---|---|---|
| Tableau Workbook | `.twb` | Structure + formulas only — no data embedded |
| Packaged Workbook | `.twbx` | Everything packed together — share-ready file |
| Extract File | `.hyper` | Data snapshot only |
| Data Source | `.tds` | Connection info only (no data) |
| Packaged Data Source | `.tdsx` | Connection info + extract |

### Publishing to Tableau Public
1. File → **Save to Tableau Public**
2. Sign in with your Tableau Public account
3. Add title, description, and tags
4. Set visibility (public by default)
5. Click Save → dashboard goes live on your profile
6. Share via direct link or embed code

### Export Options
- **File → Export** → choose PDF, Image (.png), PowerPoint (.pptx), Crosstab (Excel)
- **Dashboard → Export Image** — exports just the current view
- For **scheduled refreshes**, use Tableau Server/Cloud (not available in Public)

---

## 23. SQL + Tableau Integration

### Why SQL Matters for Tableau Users
- Most companies store data in **relational databases** (MySQL, PostgreSQL, SQL Server)
- Tableau connects directly to these databases
- SQL lets you write **custom queries** when Tableau's GUI isn't flexible enough
- SQL + Tableau = essential combo for data analyst roles

### Connecting to a Database
1. Start Page → **To a Server** → choose DB type (MySQL, PostgreSQL, etc.)
2. Enter: **Server hostname**, **Port** (MySQL=3306, PG=5432), **Database name**, **Username**, **Password**
3. Click **Sign In** → Tableau shows all tables in that database
4. Drag tables to canvas to join them (visual join builder)

### Using Custom SQL in Tableau
- In Data Source tab → **New Custom SQL** → write your SELECT query
```sql
SELECT 
    o.order_id,
    c.customer_name,
    p.product_name,
    o.sales,
    o.order_date
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id
WHERE o.order_date >= '2023-01-01'
    AND o.sales > 0
ORDER BY o.order_date DESC
```

### When to Use Custom SQL
- Complex multi-table joins that visual interface doesn't support
- Pre-aggregated data (reduces data volume loaded into Tableau)
- Filtered data at source level (faster than filtering in Tableau)
- When you need window functions or CTEs from the database
- Stored procedure results

> ⚠️ **Caution:** Custom SQL disables some Tableau optimizations. Use only when necessary.

---

## 24. Advanced Tableau Topics

### Forecasting
- Works on **time-series data** (needs a date field)
- Analytics Pane → drag **Forecast** onto the view
- Tableau uses **Exponential Smoothing (ETS)** algorithm
- Right-click → **Forecast Options** → set: Forecast Length, Model Type (Automatic/Custom), Ignore last N periods
- Right-click → **Describe Forecast** → see model details (AIC, RMSE)

### Trend Lines
- Analytics Pane → drag **Trend Line** onto scatter plot or line chart
- **Types:** Linear, Logarithmic, Exponential, Power, Polynomial
- Right-click → **Describe Trend Line** → see equation, R² value, p-value
- Higher R² = stronger fit

### Clustering
- Analytics Pane → drag **Cluster** onto a scatter plot
- Tableau uses **K-Means** algorithm
- Right-click → **Edit Clusters** → change number of clusters
- Right-click → **Describe Clusters** → statistical summary
- Clusters appear as a new group in Data Pane

### Reference Lines, Bands, and Distributions
- **Reference Line** — horizontal/vertical line at a specific value (e.g., average, target)
- **Reference Band** — shaded area between two values
- **Reference Distribution** — shows percentiles across the view
- All added from Analytics Pane — drag and drop onto the chart

### Performance Optimization

| Tip | Detail |
|---|---|
| Use Extract | Faster than Live connection for large data |
| Reduce Marks | Fewer marks in view = faster rendering |
| Context Filters | Reduce dataset size before other calculations |
| Avoid nested LODs | Each LOD = separate query, nesting = multiple queries |
| Limit Custom SQL | Use native joins where possible |
| Performance Recording | Help → Settings → Start Performance Recording → analyze slow events |

### Row-Level Security
- Restrict which data rows each user can see
- Use **User Functions** in calculated fields:
  ```
  USERNAME()              → returns logged-in user's Tableau username
  ISMEMBEROF("GroupName") → TRUE if user belongs to a group
  FULLNAME()              → returns user's full display name
  ```
- Create a filter based on username:
  ```
  [Region] = USERNAME()
  ```
  (works when region values match usernames, or map via a lookup table)
- Implement via **User Filters** on Tableau Server/Cloud

### KPI Dashboard Design
- Use **BANs (Big Ass Numbers)** — large font KPI tiles at top
- Add **sparklines** next to each KPI (small line trend chart)
- Use color: Green = on target, Red = below target
- Add a **target line** as reference line using a parameter
- Calculate KPI status: `IIF(SUM([Sales]) >= [Target], "✓ On Track", "✗ Behind")`

### Data Blending — Advanced
- Blending limitation: Secondary source can only show **aggregated values** at the primary source's grain
- Use `*` (asterisk) on linked fields to define the join key
- Blending shows `*` mark on the secondary source icon in Data Pane

---

## 25. Portfolio & Career Tips

### What to Build for Portfolio (Freshers)

| Dashboard | Why | Dataset |
|---|---|---|
| Sales Performance | Most requested by interviewers | Superstore dataset (built into Tableau) |
| HR Analytics | Shows domain knowledge | Kaggle HR dataset |
| E-commerce Dashboard | Very common use case | Online retail dataset |
| COVID / World Health | Shows geographic + time-series skills | WHO/Johns Hopkins public data |
| IPL / Sports Analytics | Shows creative skills + filters | Kaggle IPL dataset |

### Where to Find Datasets
- Tableau built-in: **Superstore Sales** (already included in Tableau)
- Kaggle.com — thousands of free datasets
- data.gov — US government open data
- Google Dataset Search — `datasetsearch.research.google.com`
- World Bank Open Data — `data.worldbank.org`

### Portfolio Checklist
- ✅ Upload 3–5 dashboards on **Tableau Public** profile
- ✅ Each dashboard has a proper **title and description**
- ✅ Add **Tableau Public profile link** on resume and LinkedIn
- ✅ Create a **GitHub repo** with screenshots + README explanation
- ✅ Include at least one dashboard using **LOD expressions**
- ✅ Include at least one dashboard using **Dashboard Actions**
- ✅ Include at least one **geographic/map visualization**

### Advanced Learning Resources (After Basics)
- **Tableau Tim** (YouTube) — LOD and advanced calcs
- **Andy Kriebel** (Tableau Zen Master) — design and table calcs
- **Data with Baraa** (YouTube) — real business projects
- **Makeover Monday** — weekly data viz challenge (great for practice)
- **Viz for Social Good** — build dashboards for real causes

---

## 26. 30 Interview Questions & Answers

---

### 🔹 Basic Level (Q1–Q10)

---

**Q1. What is Tableau and what are its main uses?**

> **Answer:**
> Tableau is a visual analytics and business intelligence platform that enables users to connect to various data sources and create interactive dashboards and visualizations without writing code. Its main uses include:
> - Creating interactive dashboards for business reporting
> - Analyzing trends and patterns in large datasets
> - Building KPI tracking dashboards
> - Geographic data visualization
> - Ad-hoc analysis and data exploration
> It is widely used by data analysts, BI developers, and business stakeholders.

---

**Q2. What is the difference between Tableau Desktop and Tableau Public?**

> **Answer:**
>
> | Feature | Tableau Desktop | Tableau Public |
> |---|---|---|
> | Cost | Paid | Free |
> | Save Locally | Yes | No (cloud only) |
> | Data Privacy | Private | All dashboards are public |
> | Data Sources | All 50+ | Limited |
> | Best For | Enterprise/professional use | Learning and portfolio |
>
> For learning, Tableau Public is sufficient. Tableau Desktop is required for sensitive/private data.

---

**Q3. What are Dimensions and Measures in Tableau?**

> **Answer:**
> - **Dimensions** are categorical, qualitative fields used to segment data. They are shown in **blue** and are usually discrete. Examples: `Region`, `Product Name`, `Category`.
> - **Measures** are numeric, quantitative fields used for calculations. They are shown in **green** and are usually continuous. Examples: `Sales`, `Profit`, `Quantity`.
>
> Tableau automatically aggregates measures (default = SUM) and uses dimensions to define the level of detail (granularity).

---

**Q4. What is the difference between a .twb and a .twbx file?**

> **Answer:**
> - **`.twb` (Tableau Workbook):** Contains only the workbook structure — the layout, calculations, and formulas. It does NOT include the actual data. The data source must be accessible separately.
> - **`.twbx` (Tableau Packaged Workbook):** Contains the workbook structure PLUS the data (extract or local file). It is a zip-like package that is self-contained and can be shared without needing access to the original data source.
>
> **Use `.twbx` when sharing with others who don't have access to the original data source.**

---

**Q5. What are the different types of joins available in Tableau?**

> **Answer:**
> Tableau supports four types of joins:
> - **Inner Join:** Returns only rows that have matching values in BOTH tables
> - **Left Join:** Returns all rows from the left table + matching rows from the right table (nulls for non-matches)
> - **Right Join:** Returns all rows from the right table + matching rows from the left table (nulls for non-matches)
> - **Full Outer Join:** Returns all rows from both tables, with nulls where there is no match
>
> In modern Tableau, **Relationships** are preferred over Joins as they avoid data duplication and handle multiple granularities better.

---

**Q6. What is the difference between a Live Connection and an Extract in Tableau?**

> **Answer:**
> - **Live Connection:** Tableau queries the data source in real-time every time the view loads. Data is always current but performance depends on the database speed.
> - **Extract (`.hyper`):** Tableau takes a snapshot of the data and stores it locally in a highly optimized columnar format. Queries are much faster but data may be stale until you refresh.
>
> **When to use Extract:** Large datasets, slow databases, offline work, better dashboard performance.
>
> **When to use Live:** Real-time dashboards where data changes every few minutes (stock prices, live operations monitoring).

---

**Q7. What are Continuous and Discrete fields in Tableau?**

> **Answer:**
> - **Discrete fields (Blue pill):** Have distinct, separate values. They create **headers and labels** in the view. Example: Month names (January, February...) — each month gets its own column/row.
> - **Continuous fields (Green pill):** Form an unbroken range. They create a numeric **axis** in the view. Example: Sales as an axis from 0 to 1,000,000.
>
> Any field can be switched between Continuous and Discrete by right-clicking → Convert to Discrete/Continuous. Dates are commonly switched (e.g., Year as continuous = axis with years; Year as discrete = separate columns per year).

---

**Q8. What is the Show Me panel in Tableau?**

> **Answer:**
> The **Show Me panel** is located at the top-right corner of a worksheet. It suggests available chart types based on the fields currently selected. When you select dimensions and measures, Tableau highlights which chart types are appropriate (based on required field types and count). Clicking a chart type auto-arranges the selected fields into the correct shelves (Rows, Columns, Marks) to produce that chart. It is especially useful for beginners to explore different visualization options quickly.

---

**Q9. What is Data Blending in Tableau? When would you use it?**

> **Answer:**
> Data Blending combines data from **two different data sources** at the visualization level. One source acts as the **Primary** source and the other as the **Secondary** source. Tableau aggregates data from the secondary source at the level of detail of the primary source and links them on a common dimension.
>
> **When to use Data Blending:**
> - When data is in two different systems that can't be joined at the database level (e.g., sales data in Excel + targets data in Google Sheets)
> - When you don't have permission to write joins at the database level
>
> **Limitation:** The secondary source can only show aggregated values — you can't access row-level detail from the secondary source.

---

**Q10. What is the difference between Groups and Sets in Tableau?**

> **Answer:**
>
> | | Groups | Sets |
> |---|---|---|
> | Definition | Manually combine dimension members into one category | Dynamic subset with In/Out classification |
> | Type | Static | Can be dynamic (based on conditions) |
> | Output | New category name | TRUE (In) / FALSE (Out) |
> | Use in calc | Not directly | Yes — `IN [Set Name]` |
> | Example | Group East + West → "Coastal" | Customers with Sales > $50,000 = "Top Customers" |
>
> **Groups** are for relabeling/recombining. **Sets** are for defining segments used in calculations and comparisons.

---

### 🔹 Intermediate Level (Q11–Q20)

---

**Q11. Explain the Filter Order of Operations in Tableau.**

> **Answer:**
> Tableau applies filters in a specific order (top = applied first, bottom = applied last):
>
> 1. **Extract Filter** — applied when creating the extract
> 2. **Data Source Filter** — applied globally across all worksheets
> 3. **Context Filter** — creates a temporary sub-dataset
> 4. **Dimension Filters** — filters specific category values
> 5. **Measure Filters** — filters aggregated numeric values
> 6. **Table Calculation Filters** — applied last, after all calculations
>
> This order matters because LOD expressions (FIXED) ignore Dimension Filters but respect Context Filters. Top N filters also need Context Filters to work correctly per segment.

---

**Q12. What is a Context Filter and why is it important?**

> **Answer:**
> A Context Filter creates a **temporary sub-dataset** from your data. All subsequent filters, Top N filters, and LOD expressions then run within this sub-dataset rather than the full dataset.
>
> **Why it matters:**
> - **Top N per Category:** Without Context Filter, "Top 5 Products by Region" might show the global top 5, not top 5 per region. Adding Region as a Context Filter fixes this.
> - **LOD Respect:** FIXED LOD expressions ignore regular Dimension Filters but respect Context Filters. If you want a FIXED LOD to account for a user-selected filter, add that filter to Context.
>
> **How to create:** Right-click a filter in the Filters shelf → **Add to Context** (turns gray color).

---

**Q13. What are Calculated Fields and Table Calculations? What is the difference?**

> **Answer:**
> - **Calculated Fields:** Created from raw data — they compute values row by row or as aggregations on the underlying data. They appear in the Data Pane and can be used in any sheet. Example: `Profit Ratio = SUM([Profit]) / SUM([Sales])`
>
> - **Table Calculations:** Applied on the **result set already returned** from the database — they compute values based on what is already displayed in the view. They do NOT go back to the raw data. Example: Running Total, Rank, Percent of Total.
>
> **Key Difference:** Calculated fields run during the database query phase. Table calculations run after the query, on the rendered data in the view. Table calculations are affected by what dimensions are in the view — changing dimensions can change table calc results.

---

**Q14. What is a Parameter in Tableau? Give a real example.**

> **Answer:**
> A Parameter is a **dynamic, user-controlled input** that stores a single value and can be referenced in calculated fields, filters, and reference lines.
>
> **Real Example — Dynamic Top N Filter:**
> 1. Create a Parameter: Name = `Top N`, Type = Integer, Range = 1 to 20, Default = 5
> 2. Right-click → Show Parameter (shows a slider on the dashboard)
> 3. Create Calculated Field: `RANK(SUM([Sales])) <= [Top N]`
> 4. Drag calculated field to Filters shelf → keep TRUE
> 5. Now when user moves the slider from 5 to 10, the view shows top 10 instead of top 5 automatically.
>
> Other uses: Switch between measures (Sales/Profit/Qty), What-if analysis, reference line thresholds.

---

**Q15. What are LOD (Level of Detail) expressions? Explain FIXED, INCLUDE, EXCLUDE.**

> **Answer:**
> LOD expressions control the granularity of a calculation independently of what dimensions are in the current view.
>
> **FIXED:** Calculates at the specified dimension level, ignoring view dimensions.
> ```
> {FIXED [Customer ID] : SUM([Sales])}
> → Total sales per customer, regardless of what else is in the view
> ```
>
> **INCLUDE:** Adds an extra dimension to the calculation beyond what's in the view.
> ```
> {INCLUDE [Order ID] : AVG([Sales])}
> → Even if view is at Customer level, this goes down to Order level for the avg calc
> ```
>
> **EXCLUDE:** Removes a dimension from the calculation that IS in the view.
> ```
> {EXCLUDE [Month] : SUM([Sales])}
> → Even though Month is in the view, this calculation ignores it → shows annual total in each month row
> ```

---

**Q16. What is the difference between Relationships and Joins in Tableau?**

> **Answer:**
>
> | | Joins | Relationships |
> |---|---|---|
> | When combined | At data source load time | At query time (dynamically) |
> | Data duplication | Can cause row duplication | No duplication |
> | Granularity | Single, fixed grain | Each table keeps own grain |
> | Null handling | Depends on join type | Automatic |
> | Best for | Simple cases, full control | Most multi-table scenarios |
> | Available since | Always | Tableau 2020.2+ |
>
> **Recommendation:** Use Relationships as the default. Use Joins only when you need a specific row-level combination or when working with custom SQL.

---

**Q17. What are Dashboard Actions and what types are available?**

> **Answer:**
> Dashboard Actions make dashboards interactive — they define what happens when a user interacts with a visualization. Types:
>
> - **Filter Action:** Clicking a mark in one chart filters the data shown in another chart. Most commonly used action.
> - **Highlight Action:** Clicking a mark highlights related marks in other charts (doesn't filter, just highlights).
> - **URL Action:** Clicking a mark opens a URL in the browser (e.g., product page, Google Maps location).
> - **Parameter Action:** Clicking a mark changes the value of a Parameter dynamically.
> - **Set Action:** Clicking a mark adds or removes items from a Set.
>
> Actions can be triggered by: Hover, Select (click), or Menu (right-click).

---

**Q18. How do you create a Top N filter that the user can control?**

> **Answer:**
> Step-by-step:
> 1. Create a **Parameter**: Name = `Top N`, Type = Integer, Range = 1 to 20
> 2. Right-click Parameter → **Show Parameter** (visible slider on dashboard)
> 3. Create a **Calculated Field**:
>    ```
>    RANK(SUM([Sales])) <= [Top N]
>    ```
> 4. Drag this Calculated Field to **Filters** shelf → set to **True**
> 5. **Important:** If filtering within categories (e.g., top N per Region), right-click the Region filter → **Add to Context** first
> 6. Now user adjusts the slider → view updates to show that many top items

---

**Q19. Explain the different table calculation types with examples.**

> **Answer:**
> - **Running Total:** Adds up values cumulatively. Sales = [100, 200, 300] → Running Total = [100, 300, 600]. Use for YTD sales.
> - **Moving Average:** Average of current + N previous periods. Smooths out spikes in weekly/monthly data.
> - **Percent of Total:** Each bar's value divided by grand total × 100. Shows contribution of each region to total sales.
> - **Percent Difference:** (Current - Previous) / Previous × 100. Shows month-over-month or year-over-year growth %.
> - **Rank:** Assigns rank 1 to highest value. Used for leaderboards, top performer identification.
> - **Index:** Assigns a sequential row number (1, 2, 3...) to each row in the partition.
>
> All can be added via: Right-click measure in view → Quick Table Calculation → choose type.

---

**Q20. What is Union in Tableau? How is it different from Join?**

> **Answer:**
> - **Union** combines tables **vertically** — it stacks rows from multiple tables on top of each other. The tables must have the same column structure (like SQL UNION ALL). Example: January.csv + February.csv + March.csv → one combined table with all rows.
>
> - **Join** combines tables **horizontally** — it matches rows from different tables based on a common key and adds columns from both tables side-by-side.
>
> **Analogy:** Join = adding new columns (widening the table). Union = adding new rows (lengthening the table).
>
> Tableau also supports **Wildcard Union** — automatically includes new files that match a file naming pattern.

---

### 🔹 Advanced Level (Q21–Q30)

---

**Q21. How does FIXED LOD interact with filters? What is the solution when they conflict?**

> **Answer:**
> **FIXED LOD ignores all Dimension Filters** in the view. This means if a user selects a filter (e.g., filter by Year = 2023), a FIXED LOD expression calculates across ALL years, not just 2023.
>
> **Example:**
> ```
> {FIXED [Customer ID] : SUM([Sales])}
> ```
> Even with a Year filter on the sheet, this will sum sales across all years for each customer.
>
> **Solution:** Convert the conflicting filter to a **Context Filter**:
> - Right-click the Year filter in the Filters shelf → **Add to Context**
> - Now FIXED LOD respects this filter and only calculates within the context
>
> This is one of the most common bugs in Tableau dashboards that analysts encounter.

---

**Q22. What is the difference between COUNTD and COUNT in Tableau?**

> **Answer:**
> - **COUNT([Field]):** Counts the total number of rows (including duplicates). If a customer placed 5 orders, COUNT = 5.
> - **COUNTD([Field]):** Counts the number of **distinct (unique)** values only. For the same customer with 5 orders, COUNTD([Customer ID]) = 1.
>
> **When to use COUNTD:** When you need unique counts — number of unique customers, unique products sold, unique orders, etc.
>
> **Performance note:** COUNTD is slower than COUNT because it requires deduplication. On very large datasets, use with extracts for better performance.

---

**Q23. How would you calculate Year-over-Year (YoY) growth in Tableau?**

> **Answer:**
> **Method 1 — Table Calculation (simpler):**
> 1. Drag `Order Date` to Columns (set to YEAR granularity)
> 2. Drag `Sales` to Rows
> 3. Right-click the `SUM(Sales)` pill → Quick Table Calculation → **Percent Difference**
> 4. Edit Compute Using → **Table (Across)** or Year dimension
>
> **Method 2 — LOD + Calculated Field (more flexible):**
> ```
> (SUM([Sales]) - {FIXED YEAR([Order Date]) - 1 : SUM([Sales])}) 
> / {FIXED YEAR([Order Date]) - 1 : SUM([Sales])}
> ```
> *(This is a conceptual formula — actual implementation uses DATEADD and lookup functions)*
>
> **Method 3 — LOOKUP function (table calc):**
> ```
> (SUM([Sales]) - LOOKUP(SUM([Sales]), -1)) / ABS(LOOKUP(SUM([Sales]), -1))
> ```
> `LOOKUP(expr, -1)` returns the value from the previous row in the partition.

---

**Q24. What is a Viz in Tooltip and how do you create it?**

> **Answer:**
> **Viz in Tooltip** is a feature that displays a **mini-chart inside the hover tooltip** of a main chart. When a user hovers over a data point, a detailed chart appears in the tooltip popup.
>
> **Steps to create:**
> 1. Create a **separate, detailed worksheet** that you want to show in the tooltip (e.g., monthly sales trend)
> 2. Go to your **main worksheet** → click **Tooltip** in the Marks Card
> 3. In the Tooltip editor → click **Insert** → **Sheets** → select your detail sheet
> 4. Adjust the `maxwidth` and `maxheight` parameters in the inserted code
> 5. Optional: Add filter parameter so the tooltip sheet filters based on the hovered item
>
> Example use: Hover over a Region bar → see monthly trend for that specific region in the tooltip.

---

**Q25. How do you implement Row-Level Security in Tableau?**

> **Answer:**
> Row-Level Security (RLS) restricts which data rows each user can see after logging into Tableau Server/Cloud.
>
> **Implementation using User Filters:**
> 1. Create a **mapping table** — columns: `Username` (Tableau login), `Region` (the data they can see)
> 2. Join/relate this mapping table to your main data source
> 3. Create a **Calculated Field:**
>    ```
>    [Region] = USERNAME()
>    ```
>    Or for a lookup approach:
>    ```
>    CONTAINS([Allowed Regions], USERNAME())
>    ```
> 4. Add this calculated field to the **Data Source Filters** → set to TRUE
> 5. Publish to Tableau Server
>
> Now each user who logs in sees only the rows where their username matches the Region mapping.
>
> **Alternative:** Use Tableau Server's built-in User Filters (available in the Data Source tab → Server menu → Create User Filter).

---

**Q26. What is the difference between SUM and TOTAL in Tableau?**

> **Answer:**
> - **SUM([Sales]):** Aggregates the raw Sales values from the underlying data. The result depends on the dimensions in the view (granularity). If Region is in the view, SUM gives sales per region.
>
> - **TOTAL(SUM([Sales])):** A Table Calculation that returns the **grand total** across all rows in the current partition of the view. It gives you the total regardless of how the view is segmented.
>
> **Use case:** To calculate what percentage each region contributes to total:
> ```
> SUM([Sales]) / TOTAL(SUM([Sales]))
> ```
> This divides each region's sales by the grand total shown in the view (not the database total).
>
> **TOTAL vs WINDOW_SUM:** Both are similar. `TOTAL()` = sum across entire table by default. `WINDOW_SUM()` lets you define a specific window range (start, end).

---

**Q27. When should you use INCLUDE vs FIXED LOD?**

> **Answer:**
>
> | Scenario | Use |
> |---|---|
> | Calculate at a specific level regardless of the view | **FIXED** |
> | Add more granularity than what's in the view | **INCLUDE** |
>
> **FIXED Example:** You want total sales per customer always — even if your view shows by City:
> ```
> {FIXED [Customer ID] : SUM([Sales])}
> → Always gives customer-level total, no matter what's in the view
> ```
>
> **INCLUDE Example:** View is at Customer level. You want average order value per customer (need to go down to Order ID level first, then average up):
> ```
> {INCLUDE [Order ID] : SUM([Sales])}
> → Calculates SUM at Customer+OrderID level, then AVG of those sums at Customer level
> ```
>
> **Rule of thumb:** FIXED = lock to a dimension. INCLUDE = temporarily add a finer grain dimension.

---

**Q28. How do you optimize a slow Tableau dashboard?**

> **Answer:**
> **Data Source level:**
> - Switch from Live Connection to **Extract** (`.hyper`) for better performance
> - Apply **Extract Filters** to bring in only necessary rows
> - Use **Data Source Filters** to reduce data volume
> - Pre-aggregate data in SQL before connecting (Custom SQL or database views)
>
> **Workbook level:**
> - Reduce the number of **marks** in the view (limit data points shown)
> - Avoid using too many **quick filters** — each one runs a query
> - Use **Context Filters** to reduce dataset for subsequent operations
> - Avoid **nested LOD expressions** — each LOD = separate sub-query
> - Replace complex calculated fields with SQL-level calculations where possible
>
> **Dashboard level:**
> - Reduce number of **worksheets per dashboard**
> - Use **Layout Containers** efficiently (avoid too many floating objects)
> - Minimize **Viz in Tooltip** — they load additional queries on hover
>
> **Diagnosis tool:** Help → Settings → Start **Performance Recording** → interact with dashboard → Stop Recording → Tableau shows a waterfall chart of time spent per event.

---

**Q29. What is the difference between Measure Names and Measure Values in Tableau?**

> **Answer:**
> These are two special auto-generated fields that Tableau creates:
>
> - **Measure Names:** A dimension that contains the **names** of all measures in your data (Sales, Profit, Quantity, etc.). It is a list of strings.
>
> - **Measure Values:** A measure that contains the **actual values** of all measures, tied to what's selected in Measure Names.
>
> **When are they used?**
> When you want to show **multiple measures on the same axis** — for example, a bar chart comparing Sales, Profit, and Quantity side-by-side:
> 1. Drag `Measure Names` to Columns
> 2. Drag `Measure Values` to Rows
> 3. In the Measure Values shelf (appears at the bottom of the Marks area), keep only the measures you want
>
> This creates a chart with one bar per measure, all sharing the same axis.

---

**Q30. Explain a complex real-world scenario where you used LOD expressions to solve a business problem.**

> **Answer (Sample scenario — customize with your own experience):**
>
> **Business Problem:** The marketing team wanted to identify **which customers made their first purchase in 2023 (new customers)** versus those who purchased before (returning customers), and compare their average order value.
>
> **Solution using LOD:**
>
> **Step 1 — Find each customer's first purchase date:**
> ```
> First Purchase Date = {FIXED [Customer ID] : MIN([Order Date])}
> ```
>
> **Step 2 — Classify as New or Returning:**
> ```
> Customer Type =
> IF YEAR([First Purchase Date]) = 2023 THEN "New Customer"
> ELSE "Returning Customer"
> END
> ```
>
> **Step 3 — Average Order Value (needs Order ID level, but view is at Customer level):**
> ```
> Avg Order Value = {INCLUDE [Order ID] : SUM([Sales])}
> → Then use AVG() of this in the view
> ```
>
> **Step 4:** Built a dashboard showing:
> - Count of New vs Returning customers by month
> - Average Order Value comparison between the two segments
> - Regional breakdown of new customer acquisition
>
> This analysis helped the team identify that new customers had 23% lower avg order value — leading to a targeted upsell email campaign for first-time buyers.

---

## 📌 Quick Reference Cheat Sheet

### Most Important Functions

```
Null Handling:    IFNULL([F], 0)  |  ZN([F])  |  ISNULL([F])
Date:             DATEDIFF('day', [Start], [End])  |  DATEADD('month', 3, [D])
String:           CONTAINS([F], "text")  |  LEN([F])  |  TRIM([F])
LOD FIXED:        {FIXED [Dim] : SUM([Measure])}
LOD INCLUDE:      {INCLUDE [Dim] : AVG([Measure])}
LOD EXCLUDE:      {EXCLUDE [Dim] : SUM([Measure])}
Table Calc:       RUNNING_SUM()  |  RANK()  |  LOOKUP()  |  WINDOW_SUM()
User Security:    USERNAME()  |  ISMEMBEROF("Group")
```

### Filter Order (Memorize This!)
```
Extract → Data Source → Context → Dimension → Measure → Table Calc
```

### Key Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + Z` | Undo |
| `Ctrl + Shift + Z` | Redo |
| `Ctrl + D` | Duplicate sheet |
| `Ctrl + W` | Close sheet |
| `F7` | Start presentation mode |
| `Alt + Shift + Backspace` | Clear sheet |

---

## 🎯 Most Important Topics for Jobs

> Prepare these topics thoroughly before any data analyst interview:

1. ✅ **Calculated Fields** — IF/CASE, String, Date, Null functions
2. ✅ **LOD Expressions** — FIXED, INCLUDE, EXCLUDE with real examples
3. ✅ **Filter Order of Operations** — especially Context Filter + LOD interaction
4. ✅ **Joins vs Relationships** — when to use which
5. ✅ **Parameters** — dynamic Top N, measure switching
6. ✅ **Table Calculations** — Running Total, Rank, Percent of Total
7. ✅ **Dashboard Actions** — Filter, Highlight, Parameter, URL Actions
8. ✅ **SQL Integration** — connect to DB, Custom SQL
9. ✅ **Real Project Dashboards** — at least 3 on Tableau Public

---

*Tableau Complete Notes — Beginner to Advanced | Good luck! 🚀*
