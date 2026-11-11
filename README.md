# Tableau Notes: From Basics to Advanced Functions

My personal study notes from a YouTube tutorial on Tableau (beginner through advanced). The course used **Tableau Public** (2021.4.2 for the advanced section), with data in Excel, CSV and MySQL.

## Table of Contents

1. [What Tableau Is](#1-what-tableau-is)
2. [Product Suite](#2-product-suite)
3. [Business Intelligence in a Nutshell](#3-business-intelligence-in-a-nutshell)
4. [The Tableau Interface](#4-the-tableau-interface)
5. [Data Concepts](#5-data-concepts)
6. [Connecting to Data](#6-connecting-to-data)
7. [Data Source Page Features](#7-data-source-page-features)
8. [Workspace Tools](#8-workspace-tools)
9. [Building a View](#9-building-a-view)
10. [Combining Data: Relationships, Joins, Blends, Unions](#10-combining-data)
11. [Parameters](#11-parameters)
12. [Level of Detail (LOD) Expressions](#12-level-of-detail-lod-expressions)
13. [Groups and Sets](#13-groups-and-sets)
14. [Spatial Functions](#14-spatial-functions)
15. [Geospatial Chart Examples](#15-geospatial-chart-examples)
16. [Filters and Order of Operations](#16-filters-and-order-of-operations)
17. [Table Calculations](#17-table-calculations)
18. [Quick Reference Cheat Sheet](#18-quick-reference-cheat-sheet)

---

## 1. What Tableau Is

- A visual analytics platform founded in 2003, out of a Stanford computer science project.
- Drag-and-drop actions are translated into data queries.
- Connects to flat files (Excel, CSV), databases (live or extract) and web data.
- Separates the data layer from the presentation layer, so updating a source spreadsheet is simple.
- Easy enough for Excel users, powerful enough for complex analysis; dashboards are responsive across devices.

## 2. Product Suite

| Category | Product | Notes |
|---|---|---|
| Developer tools | **Tableau Desktop** | Build views/dashboards. *Professional* connects to everything and publishes to Server; *Personal* is flat-files only and can't publish to Server. |
| | **Tableau Public** | Free. Flat files, Google Sheets, OData and web data connectors only. Workbooks are published to the Tableau Public site (no local save), 1M row limit. |
| | **Tableau Prep** | Visual data cleaning/shaping. *Builder* for building flows, *Conductor* for scheduling and monitoring them. |
| Sharing tools | **Tableau Reader** | Free, read-only viewer for packaged workbooks with extracts. No security, so not for internal dashboards. |
| | **Tableau Server** | On-prem or cloud hosting inside an organization. Supports live connections, scheduled refreshes, AD/SSO/SAML/Kerberos, multiple sites. |
| | **Tableau Online** | Hosted SaaS version of Server. No guest access; all viewers must authenticate. |
| | **Tableau Mobile** | Free iOS/Android app for dashboards and KPIs, with offline previews. |

## 3. Business Intelligence in a Nutshell

BI is the set of processes and technologies used to collect, integrate, analyze and present business information. Its goal is to turn raw data into actionable insight through reports, dashboards, charts and maps. Typical uses: sales KPIs and revenue analysis, HR payroll and satisfaction tracking, finance expense management and planning, plus spotting risks, automating data processing and benchmarking against competitors.

## 4. The Tableau Interface

**Start page** has three panes: *Connect* (files and servers), *Open* (recent/pinned workbooks) and *Discover* (community content, training).

**Data Source page** has four areas:
- **Left pane**: connections and the list of sheets/tables.
- **Canvas**: two layers. The *logical layer* holds relationships; double-click a table to open the *physical layer* for joins and unions.
- **Data grid**: first 1,000 rows, for sorting, renaming, hiding and changing types.
- **Metadata grid**: fields with their table and remote field names.

**Workspace** terminology:
- **Workbook** > **Worksheet** (one view) / **Dashboard** (several views) / **Story** (sequence of views).
- **Shelves**: Columns, Rows, Filters, Pages, Marks.
- **Data pane** (fields, sets, parameters) and **Analytics pane** (constant lines, reference lines, forecasts).
- **Show Me**: suggests chart types for the selected fields.
- **Sheet sorter / filmstrip / tabs**: three ways to navigate sheets.
- **Dashboard pane**: size settings, sheets and objects (images, containers, text). **Layout tab** for padding/borders, **item hierarchy** for structure.
- **Story pane**: story points, captions, navigator style.

## 5. Data Concepts

### Data shape
- Tableau works best with **tall (long) data**: each **row is a record**, each **column is a field**, and headers hold field names.
- **Wide data** (e.g. one column per year) should be reshaped (pivoted) into tall format (`Country | Year | Count`).
- Prep checklist: no extra header rows above the field names, remove total rows/columns (Tableau computes totals), keep dates in a proper date format rather than pre-split into year/month.

### Data types (icons)
| Icon | Type |
|---|---|
| ABC | String |
| Calendar | Date |
| Calendar + clock | Date & time |
| # | Number |
| T/F | Boolean |
| Globe | Geographic |

### Dimensions vs. measures
- **Dimensions** (blue): qualitative, categorize and set the level of detail (city, category).
- **Measures** (green): quantitative and aggregatable (sales, profit). Default aggregation is `SUM`.

### Discrete vs. continuous
- **Discrete**: distinct values, drawn as headers/labels (no axis).
- **Continuous**: any value in a range, drawn as an axis (trends over time).
- Usually dimensions are discrete and measures continuous, but not always. Right-click a pill to switch. Dates can be either.

### Relationships vs. joins
| Relationships | Joins |
|---|---|
| Flexible, dynamic "contract" between tables | Static, defined up front in the physical layer |
| Join type chosen automatically per viz | You choose the join type and clauses |
| Keeps native level of detail | Merges everything into a single table |
| **Recommended first** | Use when you need a specific join type |

## 6. Connecting to Data

Things to consider before connecting: **access/security**, **shape and cleanliness**, **data model** (how tables combine), **metadata/customization**, **scalability and discoverability**, **performance and freshness** (live vs. extract).

- **Databases**: typed columns, so formats are already enforced.
- **Flat files**: flexible but fragile (inconsistent formats, renamed columns break things).
- **Web Data Connector**: HTML + JavaScript for sources without a built-in connector.
- Connect to files by dragging them onto the start page or via the Connect pane.
- Connect to databases by entering server, port, database, username and password; optionally an **initial SQL** command; then use **custom SQL** for specific queries.

## 7. Data Source Page Features

- Each connection gets a different color tag; adding a connection does **not** replace the existing one (use *Edit Connection* to replace).
- **Live** (default): real-time, but speed depends on the database and network.
- **Extract**: a fast, in-memory snapshot (offline capable) that must be refreshed. Can be filtered/aggregated when created.
- **Data source filters**: limit what users can see.
- **Data Interpreter**: detects titles, footnotes, empty cells and sub-tables in messy Excel/CSV/PDF/Google Sheets. Not available beyond 2,000 columns or 3,000 rows. *Review the results* exports a color-coded Excel summary.
- **Pivot**: select the wide columns, right-click, Pivot. Rename `Pivot Field Names` / `Pivot Field Values`.
- **Rename, sort, change data type, split / custom split** a field.
- **Aliases**: alternate display names for discrete dimension members only.
- All of these changes live inside Tableau; the source file is untouched.

## 8. Workspace Tools

- **View Data**: inspect underlying rows (limit up to 10,000), copy or export.
- **Folders** and **hierarchies** (e.g. Country > State > City) for organizing and drill-down. Drag one field onto another to create a hierarchy.
- **Calculated fields**: new columns defined by formulas, e.g. `Invoiced Amount = [Price] * [Quantity]`.
- Red `!` on a field means the source field changed: right-click > **Replace References**.
- Double-click the title to edit it (can insert dynamic fields/parameters); **Format** panes control fonts, shading, borders.
- Anatomy of a view: **headers**, **field labels**, **cells**, **panes**, **title**, **caption**, **axes**, **tooltips**, **legends**.

## 9. Building a View

Start with a **question**. Four ways to build:
1. Drag fields onto shelves/cards.
2. Double-click fields in the Data pane.
3. Select fields and choose a chart from **Show Me**.
4. Drop fields into the "Drop field here" grid (tabular views only).

**Marks card**: mark type (Automatic, Bar, Line, Area, Shape...), **Color**, **Size**, **Shape** (scatter/symbol views), **Detail** (adds granularity without changing the visual structure), **Label/Text**, **Tooltip**.

Rules of thumb:
- Discrete dimension: splits into categories.
- Continuous date: creates a trend line.
- Continuous measure: creates an axis.
- Discrete measure: shows plain text.

## 10. Combining Data

| Method | What it does |
|---|---|
| **Relationship** | Dynamic, context-aware combination of tables in one data source (recommended). |
| **Join** | Merges tables (physical layer). Types: Inner, Left, Right, Full Outer. |
| **Blend** | Combines separate data sources at the sheet level. Aggregates *first*, then combines. |
| **Union** | Stacks tables vertically; same structure needed. |

### Join types
- **Inner**: only rows that match in both tables.
- **Left**: all left rows plus matching right rows (nulls where no match).
- **Right**: all right rows plus matching left rows.
- **Full outer**: everything from both, nulls where no match.

### Blending
- First data source used in the view = **primary** (blue check); the others are **secondary** (orange check). This is set per sheet.
- Link fields show a red chain when active, a gray broken chain when available. Edit via *Data > Edit Blend Relationships*.
- Similar to a left join, but a join combines first then aggregates, while a blend aggregates first.
- Blends can't be published. Publish the data sources separately and blend them.
- Use only when relationships aren't possible (e.g. link fields vary per sheet, or combining published sources).

### Unions
- Manual: drag sheets into the union window. Automatic: wildcard search on sheet/workbook names (optionally subfolders/parent folders).
- Adds `Sheet` and `Table Name` (or `Path` and `Sheet`) fields showing where each row came from.

## 11. Parameters

A **parameter** is a workbook variable (number, date, boolean or string) that users can control. It holds a value but does nothing until used in a **calculated field, filter, or reference line**. Parameters don't change the source data.

| Parameters | Filters |
|---|---|
| Usable anywhere in the workbook | Tied to a sheet (or linked for dashboards) |
| Single value only | Multiple selections |
| Can be used in calculations | Cannot |

Allowable values: **All** (free input), **List**, **Range**. Show it with *right-click > Show Parameter*; change display (dropdown, radio buttons, slider, type-in).

### Example 1: category selector
```
[Category Parameter] = [Product Category]
```
Use this calculated field on the Filters shelf, filtered to `True`.

### Example 2: Top N
Filter a dimension > **Top** tab > *By field* > Top *parameter* by `SUM(Revenue)`. Create the parameter (integer, e.g. current value 15, min 1, max 50) right from the filter dialog. You can also insert the parameter into a title for a dynamic heading.

### Example 3: dynamic date level (string parameter: Year / Quarter / Month)
```
IF [Date Parameter] = "Year" THEN STR(YEAR([Date]))
ELSEIF [Date Parameter] = "Quarter" THEN STR(YEAR([Date])) + " Q" + STR(QUARTER([Date]))
ELSEIF [Date Parameter] = "Month" THEN STR(MONTH([Date])) + " " + STR(YEAR([Date]))
END
```
Replace the date pill on Columns with this field. The text must match the parameter's list values exactly (including case).

### Example 4: measure swapper
```
CASE [Measure Parameter]
WHEN "Total Sales"   THEN SUM([Sales])
WHEN "Average Sales" THEN AVG([Sales])
WHEN "Total Orders"  THEN COUNTD([Invoice Number])
END
```
Drop this field in place of the measure on color/label. Insert the parameter in the title to keep it in sync.

## 12. Level of Detail (LOD) Expressions

LODs compute aggregations at a different granularity than the view. Syntax:

```
{ FIXED | INCLUDE | EXCLUDE  [dimensions] : AGGREGATION([measure]) }
```

| Type | Behavior |
|---|---|
| **FIXED** | Uses only the listed dimensions, **ignores the view's dimensions**. |
| **INCLUDE** | Adds listed dimensions **in addition to** the view's. |
| **EXCLUDE** | **Removes** listed dimensions from the view's level of detail. |

```
{FIXED : SUM([Total Customers])}                  // grand total
{FIXED [County] : SUM([Total Customers])}         // total per county
AVG({INCLUDE [Region] : SUM([Sales])})            // average of per-region sums
{EXCLUDE [City] : SUM([Total Customers])}         // ignore city, keep other view dims
```

Notes:
- `EXCLUDE` can't be used in row-level expressions (nothing to exclude).
- `FIXED` is evaluated **after context filters, before dimension filters**; `INCLUDE`/`EXCLUDE` are evaluated **after dimension filters**.
- Limitations: floating-point measures can behave unreliably in comparisons; LODs aren't shown on the data source page; when referencing a parameter in the dimension list use the parameter *name*; with blends, the primary linking field must be in the view.

## 13. Groups and Sets

**Groups**: manual combinations of dimension members (select members > paperclip icon, or right-click field > Create Group). Can include an **Other** bucket. Great for correcting data issues; stored in the workbook, not the source.

**Sets**: custom subsets of data.
- **Fixed sets**: manually chosen members that don't change with the data.
- **Dynamic sets**: members update with the data (by condition, formula or top/bottom N). Based on a single dimension in a sheet.
  ```
  CONTAINS([Accessory Name], "Shades")
  ```
- Show as a filter (**In/Out** or members), or as a **set control** (dynamic sets only).
- **Combine sets** (same dimension only): union, intersection, or only-in-left / only-in-right. Example: top 5 + bottom 5 by sales.

| Groups | Sets |
|---|---|
| Manual/fixed only | Fixed or dynamic |
| Dimensions and measures | Dimensions only |
| Processed like dimension filters | Processed before dimension filters |
| Can't be combined | Can be combined |
| Can't be used in expressions | Can be used in calculations |

## 14. Spatial Functions

Geospatial data = anything with a location relative to Earth. Tableau (10.2+) reads shapefiles, MapInfo tables, KML, TopoJSON, GeoJSON and Esri file geodatabases.

- Numeric X/Y columns need a **geographic role** (Latitude/Longitude) assigned manually; fields named Country/State/City/ZIP get roles automatically.
- Uncheck *Analysis > Aggregate Measures* to see individual points.
- Spatial object types: **point**, **line**, **polygon**.

```
MAKEPOINT([Centroid Y], [Centroid X])           // latitude first, then longitude
MAKELINE([Mall Points], [Next Mall Point])      // line between two points
DISTANCE([Mall Points], [Next Mall Point], "m") // units: m, km, mi, ft
BUFFER([Mall Points], 5, "km")                  // circle around a point (points only)
AREA([Mall Radius], "ft")                       // polygons only
```

- Add extra layers by dragging a spatial field onto the map ("Add a Marks Layer"); each layer has its own Marks card.
- Spatial fields aggregate with **Collect** by default.
- Format a measure in a tooltip (custom number format, decimals, suffix such as "meters").

## 15. Geospatial Chart Examples

**Airline route map (SFO flights)**
1. `Origin Point = MAKEPOINT([Origin Y], [Origin X])` and `Destination Point = MAKEPOINT([Destination Y], [Destination X])`.
2. Use custom shapes (PNG with transparent background saved in `Documents/My Tableau Repository/Shapes/<folder>`), e.g. a plane for origin and an X for destination.
3. `Plane Route = MAKELINE([Origin Point], [Destination Point])` as a layer, thin line, low opacity.
4. Route filter:
   ```
   IF LEFT([Route Tag], 3) = "SFO" THEN "From San Francisco" ELSE "To San Francisco" END
   ```
5. Map styling via *Map > Map Layers* (background style, washout, optional layers like terrain or points of interest).

**Places of worship map (two layers)**
1. Filled map layer: State on Detail (fix unknown locations via *Edit Locations* and set Country from a field), attendance on Color, dark background map.
2. Point layer: `MAKEPOINT([Y], [X])` as small star shapes colored by subtype.
3. Optionally lock the view (*Map Options > uncheck Allow Pan and Zoom*).

## 16. Filters and Order of Operations

Filters run in this order (top to bottom):

1. **Extract filters** (extract connections only; not in Tableau Public)
2. **Data source filters**
3. **Context filters** (gray pills)
4. **Dimension filters**
5. **Measure filters**
6. **Table calculation filters**

Key points:
- Filters are independent by default. Selecting "Male" with a Top 10 filter can yield fewer than 10. Fix: **Add to Context** on the gender filter so it runs first.
- Dimension filter tabs: *General*, *Wildcard*, *Condition*, *Top*.
- Measure filters ask for an aggregation, then a type: *range*, *at least*, *at most*, *special* (null / non-null / all).
- Table calc filters: Ctrl-drag a copy of the calculated pill to Filters.
- **Cascading filters**: set filter cards to *Only Relevant Values* so dependent filters (region > sub-region > country) show only valid options.

## 17. Table Calculations

A **table calculation** transforms the values already in the view (a triangle icon on the pill marks one). Compared to calculated fields:

| Table calculation | Calculated field |
|---|---|
| Works on aggregated values in the view | Works on row-level source data |
| Lives on the shelf | Appears in the Data pane |
| Filtered-out values aren't considered | Independent of view filters |

### Addressing and partitioning
- **Addressing**: the direction the calculation moves (across, down, specific dimensions).
- **Partitioning**: how the view is split into sub-tables (table, pane, cell...).
- **Compute using** options: Table (across/down/...), Pane, Cell, or Specific Dimensions. With multiple dimensions, **At the level** picks the granularity (Deepest by default).
- The yellow **calculation assistance** highlight shows scope and direction.
- Quick table calcs only apply to continuous (green) measure pills, via *right-click > Quick Table Calculation*. Available: difference, percent difference, percent from, percent of total, rank, percentile, running total, moving average (YTD/YoY if the data spans enough time).

### Examples
- **Percent of total**: choosing "Table (across)" vs "Table (down)" changes which direction sums to 100%. Add Grand Totals from the Analytics pane to verify.
- **Running total / running average**: copy the measure pill (Ctrl-drag), add a table calculation (type *Running total*, method sum/average/min/max), compare with the original, optionally use a **dual axis** and **synchronize axis**.
- **Percent difference from previous**: compare years side by side (Year and Month as specific dimensions with Year ordered first, At the level = Year, Relative to = Previous). The first year has no previous value, so it shows no label.

## 18. Quick Reference Cheat Sheet

| Task | How |
|---|---|
| Create a calculated field | Data pane dropdown > Create Calculated Field |
| Create a parameter | Data pane dropdown > Create Parameter |
| Show a parameter control | Right-click the parameter > Show Parameter |
| Pivot columns | Select columns > right-click > Pivot |
| Switch discrete/continuous | Right-click pill > Discrete / Continuous |
| Remove aggregation (spatial) | Analysis > uncheck Aggregate Measures |
| Duplicate a pill | Hold Ctrl and drag |
| Clear a sheet | Toolbar clear button or `Alt+Shift+Backspace` |
| Context filter | Right-click filter pill > Add to Context |
| Combine sets | Ctrl-select two sets > right-click > Create Combined Set |

---

*Notes based on a Simon Says IT Tableau tutorial (beginner course plus the first section of the advanced course, "Advancing in Tableau"). The advanced course's second section (actions, animation, advanced charts such as Sankey/bump/funnel, and the full dashboards) isn't covered in this video, so it isn't covered here.*
