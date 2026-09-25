# 📊 SQL Learning Project — From Basics to Advanced
### Using a Real-World Dirty Sales Dataset (50,000 Rows)

---

## 📌 Project Overview

This project is a **complete, hands-on SQL learning journey** built on top of a purposefully dirty, real-world style sales dataset. It covers everything from the very first `SELECT` statement all the way to advanced topics like **Window Functions**, **Recursive CTEs**, **Materialized Views**, and **Query Optimization**.

Every concept is demonstrated with **multiple practical queries** executed against the same dataset, so you can see not just *what* the syntax is — but *why* it matters, *what goes wrong* with dirty data, and *how to fix it*.

> **Database used:** SQLite (via DBeaver). All queries are SQLite-compatible unless explicitly noted for PostgreSQL / SQL Server.

---

## 📁 Repository Structure

```
📂 sql-learning-project/
│
├── 📄 README.md                          ← You are here
├── 📄 Dirty_Sales_Dataset_50000_Rows.csv ← Dataset (50,043 rows, 30 columns)
└── 📄 SQL_Queries.sql                    ← All queries, organized by topic
```

---

## 🗄️ Dataset Overview

**File:** `Dirty_Sales_Dataset_50000_Rows.csv`

| Property | Details |
|---|---|
| **Total Rows** | 50,043 |
| **Total Columns** | 30 |
| **Date Range** | January 2023 – September 2024 |
| **Regions** | North, South, East, West |
| **Countries** | India, UAE, USA *(with casing issues: `india`, `INDIA`)* |
| **Salespersons** | Karan, Meera, Anjali, Rohit |

### 📋 Column Reference

| Column | Type | Description |
|---|---|---|
| `Order_ID` | TEXT | Unique order identifier (has duplicates — intentional!) |
| `Order_Date` | TEXT | Date order was placed (mixed formats) |
| `Ship_Date` | TEXT | Date order was shipped |
| `Customer_ID` | TEXT | Unique customer identifier |
| `Customer_Name` | TEXT | Full name of the customer |
| `Segment` | TEXT | Business segment: Corporate, Consumer, Home Office |
| `Country` | TEXT | Country of the order (has casing issues) |
| `State` | TEXT | State |
| `City` | TEXT | City (has casing issues: `MUMBAI`, `Mumbai`, `mumbai`) |
| `Region` | TEXT | Sales region: North / South / East / West |
| `Postal_Code` | FLOAT | Postal code (has NULLs) |
| `Product_ID` | TEXT | Unique product identifier |
| `Product_Name` | TEXT | Name of the product |
| `Category` | TEXT | Product category (dirty: `Technology` vs `Tech`) |
| `Sub_Category` | TEXT | Sub-category of the product |
| `Brand` | TEXT | Brand name |
| `Quantity` | INTEGER | Units sold (has negative values — dirty!) |
| `Unit_Price` | FLOAT | Price per unit (stored as string with `$` in some rows) |
| `Discount` | FLOAT | Discount applied — valid range: 0 to 1 (has negatives & >1 values!) |
| `Sales` | FLOAT | Total sales amount (has negative values — dirty!) |
| `Cost` | FLOAT | Cost of goods |
| `Profit` | FLOAT | Profit = Sales − Cost |
| `Profit_Margin` | FLOAT | Profit as % of sales |
| `Ship_Mode` | TEXT | Shipping method (casing inconsistency: `standard ` vs `Standard`) |
| `Shipping_Cost` | FLOAT | Cost of shipping |
| `Payment_Method` | TEXT | Payment type: UPI, Cash, COD, Card, Net Banking |
| `Order_Status` | TEXT | Shipped / Pending / Delivered / Cancelled |
| `Salesperson` | TEXT | Assigned salesperson |
| `Return_Status` | TEXT | Yes / No |
| `Customer_Rating` | FLOAT | Rating 1–5 (has ~7,363 NULLs) |

---

### 🦠 Intentional Data Quality Issues

This dataset was designed to be **dirty on purpose** — so you can practice real-world data cleaning in SQL:

| Issue | Count / Details |
|---|---|
| **Negative Sales values** | 12,609 rows |
| **Duplicate Order_IDs** | 19,847 duplicates |
| **Invalid Discount** (outside 0–1 range) | 24,943 rows |
| **NULL Discount values** | 12,577 rows |
| **NULL Customer_Rating** | 7,363 rows |
| **Negative / Zero Quantity** | 1,781 rows |
| **Inconsistent casing in City** | `MUMBAI`, `Mumbai`, `mumbai` all present |
| **Inconsistent casing in Country** | `India`, `india`, `INDIA` all present |
| **Duplicate category names** | `Technology` and `Tech` mean the same thing |
| **Trailing spaces in Segment** | `' consumer '` vs `'Consumer'` |
| **Mixed date formats** | `2024-01-30`, `01/01/2023`, `Sep 30, 2024` |
| **NULL Postal_Code values** | Present throughout |

---

## 📚 SQL Topics Covered

The query file is organized into **5 major sections (A–D5)** covering 80+ individual topics with 700+ queries total.

---

### 🔷 Section A2 — SELECT Fundamentals

**A2.1 — Basic SELECT**
- `SELECT *` vs selecting specific columns — and why `SELECT *` is bad in production
- Clause order: `SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT`

**A2.2 — Derived Columns & Expressions**
- Column aliases with `AS` keyword (with and without quotes)
- Calculated columns: `Unit_Price * Quantity AS Revenue`, `(Sales - Cost) AS Profit`
- `CONCAT()` — combining string columns: city, state, country into one field
- `COALESCE()` — replacing NULLs with default values (e.g., `'Not Rated'` for missing ratings)

**A2.3 — DISTINCT**
- `SELECT DISTINCT` on single column
- `SELECT DISTINCT` on multiple columns
- `COUNT(DISTINCT col)` for unique value counting
- Performance difference: `DISTINCT` vs `GROUP BY`

**A2.4 — Filtering with WHERE**
- Comparison operators: `=`, `!=`, `<>`, `>`, `<`, `>=`, `<=`
- Boolean logic: `AND`, `OR`, `NOT` with parentheses for precedence
- `IN` — matching a list of values; `NOT IN`
- `BETWEEN` — inclusive range filtering on numbers and dates
- `LIKE` — pattern matching with `%` (any characters) and `_` (single character)
- `IS NULL` / `IS NOT NULL` — why `= NULL` doesn't work
- **The NOT IN + NULL trap** — why `NOT IN (subquery)` returns zero rows when the subquery has NULLs

---

### 🔷 Section B1 — Aggregate Functions

**B1.1 — COUNT Variants**
- `COUNT(*)` — counts all rows including NULLs
- `COUNT(column)` — counts only non-NULL values in that column
- `COUNT(DISTINCT column)` — counts unique non-NULL values
- `COUNT(*)` vs `COUNT(1)` — are they actually different?

**B1.2 — SUM, AVG, MIN, MAX**
- `SUM()` — total of a column; handling negative values in dirty data
- `AVG()` — automatic NULL-ignoring behavior; `AVG` vs `SUM/COUNT` difference
- `MIN()` and `MAX()` — on numbers, dates, and even strings
- **`SUM(CASE WHEN ...)` pattern** — conditional aggregation (the most powerful pattern in SQL)
- **Why `AVG` ≠ `SUM/COUNT(*)`** — demonstrated with NULL-heavy columns

---

### 🔷 Section B2 — GROUP BY & HAVING

**B2.1 — GROUP BY**
- How GROUP BY works internally — the "bucket" analogy
- `GROUP BY` with `COUNT`, `SUM`, `AVG`
- Grouping by multiple columns
- Common error: non-aggregated columns in SELECT without GROUP BY

**B2.2 — HAVING**
- `HAVING` vs `WHERE` — the key difference (filter before vs. after grouping)
- `HAVING COUNT(*) > N` — finding frequent groups
- `HAVING SUM() > value` and `HAVING AVG() > value`
- Using `WHERE` and `HAVING` together in a single query

**B2.3 — Advanced Grouping (OLAP)**
- `GROUPING SETS` — multiple GROUP BY levels in one query
- `ROLLUP` — automatic subtotals and grand total
- `CUBE` — all possible combination subtotals
- `GROUPING()` function — how to distinguish ROLLUP-generated NULLs from real NULLs

---

### 🔷 Section B3 — CASE WHEN

**B3.1 — CASE Syntax**
- Simple CASE (value-based branching)
- Searched CASE (condition-based branching)
- CASE inside SELECT for custom category labels
- `ELSE` clause — why you should always include it and how to use it as a data quality flag

**B3.2 — CASE in Advanced Patterns**
- **Creating bins/buckets** — grouping Sales into ranges like `0-999`, `1000-4999`, etc.
- **`SUM(CASE WHEN ...)` for conditional aggregation** — sum only specific rows
- **`COUNT(CASE WHEN ...)` for conditional counting**
- **CASE inside `ORDER BY`** — custom sort logic (e.g., `Pending → Delivered → Returned → Cancelled`)
- **Manual PIVOT with CASE + GROUP BY** — turning rows into columns

---

### 🔷 Section B4 — String Functions

**B4.1 — Case & Cleaning**
- `UPPER()`, `LOWER()` — standardizing city/country names
- `TRIM()`, `LTRIM()`, `RTRIM()` — removing whitespace (the silent data killer)
- `LENGTH()` / `LEN()` — validating field lengths (e.g., Order_ID should always be 9 chars)
- `REPLACE()` — removing `$` signs and commas from Unit_Price stored as text

**B4.2 — Extraction & Splitting**
- `SUBSTR()` / `SUBSTRING()` — extract part of a string by position
- `LEFT()`, `RIGHT()` — first/last N characters (SQLite alternative using `SUBSTR`)
- `INSTR()` / `POSITION()` / `CHARINDEX()` — find the position of a character
- Splitting Customer_Name into First_Name and Last_Name using `INSTR` + `SUBSTR`
- `SPLIT_PART()` — delimiter-based splitting (PostgreSQL; SQLite equivalent shown)

**B4.3 — Combining & Regex**
- `CONCAT()` and `CONCAT_WS()` — combining columns with a separator
- `||` operator — SQLite string concatenation
- `REGEXP_REPLACE()` — cleaning data using regex patterns (PostgreSQL)
- `REGEXP_LIKE()` / `~` operator — filtering with regex
- SQLite alternatives: `GLOB`, `LIKE`, `LENGTH` for pattern validation
- `CAST()` — converting `Unit_Price` stored as TEXT into a usable REAL number

---

### 🔷 Section B5 — Date Functions

**B5.1 — Current Date & Extraction**
- `CURRENT_DATE`, `CURRENT_TIMESTAMP`, `NOW()`, `datetime('now')` — SQLite equivalents
- `EXTRACT()` — getting year, month, day from a date (PostgreSQL); `strftime()` for SQLite
- `strftime('%Y', Order_Date)` for year-based grouping
- `strftime('%Y-%m', Order_Date)` for monthly trend analysis
- `TO_DATE()` — converting text to date; SQLite-compatible approach using `date()`
- Handling **mixed date formats** in this dataset (`YYYY-MM-DD` vs `DD/MM/YYYY` vs `Mon DD, YYYY`)

**B5.2 — Date Arithmetic**
- `DATE + INTERVAL` — adding/subtracting time (PostgreSQL); `date(col, '+7 days')` for SQLite
- `DATEDIFF()` / `julianday()` — calculating days between two dates
- Shipping delay analysis: `julianday(Ship_Date) - julianday(Order_Date)`
- `AGE()` function — PostgreSQL equivalent; SQLite workaround using `julianday`
- `DATEADD()` — SQL Server/MySQL syntax vs SQLite equivalent

**B5.3 — Advanced Date Operations**
- `DATE_TRUNC()` — truncating to month/week/year for period analysis
- Monthly sales trends with `strftime('%Y-%m', Order_Date)`
- **Generating a date series** for gap analysis using Recursive CTE
- Timezone handling with `AT TIME ZONE` (PostgreSQL)
- **Fiscal year calculations** using `CASE + EXTRACT` (April start, Indian FY)

---

### 🔷 Section C1 — Database Design Concepts

**C1.1 — Keys & Relationships**
- **Primary Key** — what it is, and how to detect violations (duplicate Order_IDs in this dataset!)
- **Foreign Key** — linking tables; how it prevents orphan records
- **Relationship types** — One-to-One, One-to-Many, Many-to-Many — demonstrated with customer/order/product data

**C1.2 — Design Principles**
- **Normalization** — why data is split across tables; detecting denormalization issues (same Customer_ID with multiple Customer_Names)
- **JOIN vs Subquery** — when to use which approach
- **Cartesian product** — what happens without a JOIN condition and why it's dangerous

---

### 🔷 Section C2 — JOINs (All Types)

**C2.1 — INNER JOIN**
- Basic INNER JOIN syntax with alias
- JOIN with WHERE filters
- JOIN with GROUP BY and aggregation
- Joining on multiple columns simultaneously

**C2.2 — LEFT JOIN**
- All rows from left table, matching from right — what NULLs mean in the result
- **Finding missing records** using `LEFT JOIN + IS NULL` (anti-join pattern)
- Difference between LEFT JOIN and INNER JOIN output
- **LEFT JOIN with COUNT** — why `COUNT(*)` vs `COUNT(column)` gives different results

**C2.3 — RIGHT JOIN & FULL OUTER JOIN**
- RIGHT JOIN — and why you can always rewrite it as a LEFT JOIN
- **FULL OUTER JOIN** — all rows from both tables; SQLite workaround using `UNION` of two LEFT JOINs
- Finding unmatched records in both directions simultaneously

**C2.4 — Self JOIN**
- Joining a table to itself — finding customers with multiple orders on the same date
- Employee-Manager hierarchy using Self JOIN
- **Finding duplicate rows** using Self JOIN on matching columns

**C2.5 — CROSS JOIN**
- What Cartesian product means and when it's actually useful
- Generating all Region × Category combinations to find missing coverage gaps
- Using CROSS JOIN to generate test data (with `Digits` table trick)

---

### 🔷 Section C3 — Subqueries

**C3.1 — Subquery Placement**
- **Subquery in WHERE** — filter rows using `> (SELECT AVG(...))` or `IN (SELECT ...)`
- **Subquery in FROM** (inline view / derived table) — treat a query result as a table
- **Subquery in SELECT** (scalar subquery) — show overall average next to each row
- **Correlated vs Non-Correlated** subqueries — the performance difference and when each is appropriate

**C3.2 — Advanced Subquery Operators**
- **`EXISTS` and `NOT EXISTS`** — the faster alternative to `IN` / `NOT IN`
- **`IN` vs `EXISTS`** — performance difference explained with examples
- **`ANY` and `ALL` operators** — `> ANY` is equivalent to `> MIN`, `> ALL` equivalent to `> MAX`
- **Nested subqueries (3 levels deep)** — finding customers who spend above average, using CTEs as the cleaner alternative

---

### 🔷 Section C4 — Set Operations

**C4.1 — UNION & UNION ALL**
- `UNION` — combining result sets, automatically removes duplicates
- `UNION ALL` — keeps duplicates, significantly faster
- `UNION` vs `JOIN` — they do fundamentally different things (rows vs columns)
- Stacking monthly/yearly reports using `UNION ALL`
- Column count and data type matching requirements

**C4.2 — INTERSECT & EXCEPT**
- `INTERSECT` — rows common to both queries (e.g., customers who bought in both 2023 and 2024)
- `EXCEPT` / `MINUS` — rows in first query but not second
- **Real use case: Customer Churn Analysis** — finding customers who bought in 2023 but not in 2024, calculating churn rate percentage

---

### 🔷 Section D1 — Window Functions

**D1.1 — The OVER() Clause**
- What a window function is — how it differs from GROUP BY (keeps all rows!)
- `PARTITION BY` — creating groups without collapsing rows
- `ORDER BY` inside `OVER()` — controls row ordering within the window
- **`ROWS` vs `RANGE` framing** — the most misunderstood concept in window functions, demonstrated with duplicate dates

**D1.2 — Ranking Functions**
- `ROW_NUMBER()` — unique sequential number per row (no ties)
- `RANK()` — same rank for ties, leaves gaps after ties (1, 1, 3...)
- `DENSE_RANK()` — same rank for ties, no gaps (1, 1, 2...)
- `NTILE(n)` — divide rows into N equal buckets (quartiles, deciles)
- **Top N per Group** using `ROW_NUMBER()` inside a subquery — most commonly asked interview pattern

**D1.3 — Aggregate Window Functions**
- `SUM() OVER()` — running total / cumulative sum
- `AVG() OVER()` — running average and moving average
- `COUNT() OVER()` — running count and order sequence number per customer
- `MIN() / MAX() OVER()` — running min/max to detect new records
- **7-Day Rolling Average** using `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW`

**D1.4 — Lag/Lead & Value Functions**
- `LAG()` — access the previous row's value; day-over-day change calculation
- `LEAD()` — access the next row's value; days until next order
- `FIRST_VALUE()` — get the first value in the window (always needs explicit frame!)
- `LAST_VALUE()` — get the last value in the window (the `UNBOUNDED FOLLOWING` trap!)
- `NTH_VALUE()` — get the nth row's value in the window

**D1.5 — Statistical Window Functions**
- `PERCENT_RANK()` — relative rank as a percentage (0 to 1)
- `CUME_DIST()` — cumulative distribution value
- **Percentile calculation** using `ROW_NUMBER()` for SQLite (no `PERCENTILE_CONT` support)
- **Outlier detection** using `PERCENT_RANK()` — identifying top 1% and bottom 1% of Sales

---

### 🔷 Section D2 — Common Table Expressions (CTEs)

**D2.1 — Standard CTEs**
- `WITH` clause syntax — step-by-step explanation
- **CTE vs Subquery** — when CTEs are more readable and when to prefer subqueries
- **Multiple CTEs chained together** — building data pipelines step by step
- **CTE vs Temp Table vs View** — a practical comparison of when to use each

**D2.2 — Recursive CTEs**
- What recursion means in SQL — anchor member + recursive member
- `WITH RECURSIVE` — generating number sequences 1 to N
- **Generating a complete date series** — for gap analysis and zero-filled reports
- **Traversing an organizational hierarchy** (Manager → Employee chain) — downward and upward traversal
- `MAXRECURSION` — preventing infinite loops; SQLite `LIMIT` as safety net

---

### 🔷 Section D3 — PIVOT & UNPIVOT

**D3.1 — PIVOT (Rows to Columns)**
- What pivoting means — turning category values into column headers
- **Manual PIVOT using `CASE WHEN + GROUP BY`** — the universal approach that works in all databases
- Native `PIVOT` syntax in SQL Server — with `FOR ... IN (...)` clause
- **Dynamic PIVOT** — when column names are not known in advance (SQL Server with `sp_executesql`)
- Percentage-based pivot tables

**D3.2 — UNPIVOT (Columns to Rows)**
- What unpivoting means — converting wide format to long format
- **Manual UNPIVOT using `UNION ALL`** — the universal approach
- Native `UNPIVOT` syntax in SQL Server
- **Real use case: Employee Survey Data** — reshaping wide survey data for analysis with `AVG()` by category

---

### 🔷 Section D4 — Views, Temp Tables & Materialized Views

**D4.1 — Views**
- What a view is — a stored query as a virtual table
- Creating, querying, dropping, and recreating views
- Six view patterns: filtering, column restriction, JOIN, window function, CTE-based, aggregate
- **Updatable vs Read-Only views** — which views can be updated and why aggregate views cannot
- **When to use views** in a data analyst workflow — layered view architecture

**D4.2 — Temporary Tables**
- Creating temp tables in SQLite, PostgreSQL, SQL Server, MySQL
- Creating indexes on temp tables for performance
- **Temp Table vs CTE** — when performance matters
- **LOCAL vs GLOBAL temp tables** (SQL Server `#` vs `##` convention)
- **Multi-step data transformation pipelines** — building a full VIP Customer Report and a Data Quality Report using chained temp tables

**D4.3 — Materialized Views**
- What materialized views are — stored, pre-computed query results
- PostgreSQL: `CREATE MATERIALIZED VIEW`, `REFRESH MATERIALIZED VIEW`, concurrent refresh
- **SQLite simulation** — using a regular table as a materialized view with DROP + CREATE refresh cycle
- Refresh strategies: complete refresh, concurrent refresh, staging table pattern
- **When to use materialized views in Data Science** — feature stores, A/B test snapshots, dashboard aggregations
- Materialized View vs Regular View — performance comparison

---

### 🔷 Section D5 — Query Optimization

**D5.1 — Understanding Query Execution**
- **Logical order of SQL execution** vs written order — `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`
- Why you can't use SELECT aliases in WHERE (alias doesn't exist yet when WHERE runs)
- Why aggregate functions can't go in WHERE (they run after WHERE)
- **`EXPLAIN QUERY PLAN`** (SQLite) and `EXPLAIN ANALYZE` (PostgreSQL) — reading execution plans
- **Sequential Scan vs Index Scan** — what they mean, when the planner chooses each
- **Index selectivity** — why an index on `Customer_ID` (many unique values) helps more than an index on `Region` (only 4 values)
- **Cost estimation** — how the query planner makes decisions; using `ANALYZE` to update statistics
- Creating and dropping indexes; measuring their impact on query plans

---

## 🛠️ Setup & Usage

### Prerequisites
- **DBeaver** (recommended) or any SQL client that supports SQLite
- **SQLite** — no installation needed; DBeaver bundles it

### Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/sql-learning-project.git
   cd sql-learning-project
   ```

2. **Open DBeaver** → Create a new SQLite connection → Choose a new file (e.g., `sales_project.db`)

3. **Import the CSV** → Right-click the database → Import Data → Select `Dirty_Sales_Dataset_50000_Rows.csv` → Name the table `Dirty_Sales_Dataset`

4. **Open `SQL_Queries.sql`** in DBeaver's SQL editor and run any section you want to explore

---

## 💡 Key Learning Takeaways

| What I Practiced | Why It Matters in the Real World |
|---|---|
| Cleaning dirty data with SQL | Every real dataset has issues — negative sales, wrong formats, NULLs |
| `COALESCE`, `NULLIF`, `TRIM` | The bread-and-butter of data cleaning pipelines |
| `CASE WHEN` as a pivot | Manual pivot works in every database without special syntax |
| Window functions with `PARTITION BY` | The most powerful analysis tool for rankings, running totals, and trend detection |
| `LAG` / `LEAD` for time-series | Day-over-day change, churn detection, next purchase prediction |
| Recursive CTEs for date series | Filling gaps in time-series data — critical for dashboards |
| `EXCEPT` for churn analysis | Finding customers who stopped buying — a core business question |
| `EXPLAIN QUERY PLAN` | Understanding *why* a query is slow and what indexes help |
| Layered view architecture | How production BI systems are built with clean → aggregate → report layers |
| Multi-step temp table pipelines | Breaking complex transformations into debuggable steps |

---

## 📝 Notes on Database Compatibility

| Feature | SQLite | PostgreSQL | SQL Server | MySQL |
|---|---|---|---|---|
| Window Functions | ✅ (v3.25+) | ✅ | ✅ | ✅ (v8.0+) |
| Recursive CTE | ✅ | ✅ | ✅ | ✅ (v8.0+) |
| `FULL OUTER JOIN` | ❌ (workaround shown) | ✅ | ✅ | ❌ |
| `GROUPING SETS` / `ROLLUP` / `CUBE` | ❌ (workaround shown) | ✅ | ✅ | ✅ |
| Native `PIVOT` / `UNPIVOT` | ❌ (manual CASE shown) | ❌ | ✅ | ❌ |
| `MATERIALIZED VIEW` | ❌ (table simulation shown) | ✅ | ❌ | ❌ |
| `REGEXP_REPLACE` | ❌ (REPLACE shown) | ✅ | ❌ | ✅ |
| `strftime()` | ✅ | ❌ (use `EXTRACT`) | ❌ (use `DATEPART`) | ❌ (use `DATE_FORMAT`) |

> Wherever a feature is not available in SQLite, a fully working alternative is demonstrated in the query file.

---

## 🙋 Who Is This For?

- **Beginners** starting their SQL journey who want a structured path from basics to advanced
- **Intermediate SQL users** who know SELECT and GROUP BY but haven't explored window functions or CTEs
- **Data Analysts** who want to master analytical SQL patterns used in real BI/analytics work
- **Anyone preparing for SQL interviews** — all the classic patterns are covered (Top-N per group, churn analysis, running totals, duplicate detection, etc.)

---

## 📈 Concept Progression Map

```
Basic SELECT & Filtering
        ↓
Aggregate Functions (COUNT, SUM, AVG, MIN, MAX)
        ↓
GROUP BY → HAVING → ROLLUP / CUBE
        ↓
String & Date Functions (cleaning dirty data)
        ↓
CASE WHEN (binning, pivoting, conditional sums)
        ↓
JOINs (INNER → LEFT → SELF → CROSS → FULL OUTER)
        ↓
Subqueries (WHERE → FROM → SELECT → Correlated → EXISTS)
        ↓
Set Operations (UNION, INTERSECT, EXCEPT)
        ↓
Window Functions (ROW_NUMBER → RANK → SUM OVER → LAG/LEAD)
        ↓
CTEs (Standard → Multiple → Recursive)
        ↓
PIVOT / UNPIVOT
        ↓
Views → Temp Tables → Materialized Views
        ↓
Query Optimization (EXPLAIN, Indexes, Execution Plans)
```

---

*Built with ❤️ using SQLite + DBeaver*
