---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- array-manipulation
- array-reshaping
- dynamic-arrays
- data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Wrap Columns
- Column Wrap
- Reshape to Columns
- Vertical to Horizontal
tags:
- array
- wrap
- reshape
- dynamic-arrays
- transformation
---

# WRAPCOLS

## Description

**WRAPCOLS** reshapes a single row or column of data into a two-dimensional array by wrapping values into columns of a specified length. Think of it as taking a long list and filling it into a grid column by column—values fill down the first column, then continue at the top of the second column, and so on. If you have 12 months of data in a single column, WRAPCOLS can transform it into a 3-row by 4-column grid (filling each column with 3 values before moving to the next).

The function reads values from the source vector (row or column) sequentially and places them into the result array column by column, from top to bottom within each column, then left to right across columns. The wrap_count parameter determines how many values go in each column before wrapping to the next. This is the opposite flow of WRAPROWS, which fills row by row instead of column by column.

**Key behaviors to understand:** If the source doesn't divide evenly by wrap_count, the last column will have fewer values, and those empty positions are filled with pad_with (default #N/A). For example, wrapping 10 values with wrap_count=3 creates a 3x4 array: three full columns of 3 values, plus a fourth column with 1 value and 2 #N/A padding cells. WRAPCOLS only accepts one-dimensional input (either a single row or single column)—not two-dimensional arrays.

**Platform availability:** WRAPCOLS is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically. Before these functions existed, reshaping data required complex combinations of INDEX, MOD, INT, and array formulas—WRAPCOLS makes this transformation trivially simple.

## Syntax

> [!f(x)] WRAPCOLS Syntax
>
> ```
> =WRAPCOLS(vector, wrap_count, [pad_with])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `vector` | Yes | The source row or column of values to wrap. Must be one-dimensional (either a single row or single column, not a 2D array). |
| `wrap_count` | Yes | The number of values per column in the result. Each column will contain up to this many values before wrapping to the next column. Must be a positive integer. |
| `pad_with` | No | The value to use for padding when the source doesn't divide evenly. Defaults to #N/A if omitted. Can be any value: number, text, blank (""), etc. |

### Return Value

Returns a two-dimensional array with wrap_count rows and enough columns to contain all source values. Values are filled column by column (down then across). If the source count isn't a multiple of wrap_count, the last column is padded with pad_with values.

## Examples

> [!f(x)] WRAPCOLS Examples

### Example 1: Basic Column Wrapping
```
=WRAPCOLS(A1:A12, 3)
```
**Result:** Returns a 3-row by 4-column array

**Explanation:** A list of 12 values becomes a 3x4 grid. Values fill down: A1, A2, A3 in column 1, then A4, A5, A6 in column 2, etc. Each column has exactly 3 values because 12 divides evenly by 3.

---

### Example 2: Wrapping with Padding
```
=WRAPCOLS(A1:A10, 4)
```
**Result:** Returns a 4-row by 3-column array; last column has 2 values and 2 #N/A

**Explanation:** 10 values with wrap_count=4 means 3 columns needed (ceiling of 10/4). First two columns have 4 values each (8 total), the third column has 2 values and 2 #N/A padding cells.

---

### Example 3: Custom Pad Value
```
=WRAPCOLS(A1:A10, 4, 0)
```
**Result:** Same shape as above, but padding is 0 instead of #N/A

**Explanation:** Specifying pad_with=0 fills incomplete positions with zeros instead of errors. Useful when the result will be used in calculations where #N/A would cause problems.

---

### Example 4: Pad with Empty Strings
```
=WRAPCOLS(A1:A10, 4, "")
```
**Result:** Padding cells appear blank

**Explanation:** Empty string padding looks cleaner for display purposes. The cells are technically not empty (they contain ""), but they appear blank.

---

### Example 5: Wrap a Row into Columns
```
=WRAPCOLS(A1:L1, 3)
```
**Result:** A 3-row by 4-column array from a horizontal source

**Explanation:** WRAPCOLS works with either rows or columns as input. This horizontal row of 12 values becomes a 3x4 grid using the same column-first fill logic.

---

### Example 6: Create Monthly Calendar Layout
```
=WRAPCOLS(SEQUENCE(12), 3)
```
**Result:** Returns {1,4,7,10; 2,5,8,11; 3,6,9,12}

**Explanation:** Numbers 1-12 arranged into a 3x4 grid. Values 1,2,3 fill the first column; 4,5,6 fill the second; etc. This creates a grid where reading down gives sequential values.

---

### Example 7: Transform List to Multi-Column Display
```
=WRAPCOLS(ProductList, 10)
```
**Result:** Product list displayed in multiple columns, 10 items per column

**Explanation:** A long list of products becomes a multi-column layout for space-efficient display. Instead of one long column of 100 items, you get a 10-row by 10-column grid (assuming 100 products).

---

### Example 8: Wrap with Calculated Count
```
=WRAPCOLS(Data, CEILING(COUNT(Data)/4, 1))
```
**Result:** Data wrapped into exactly 4 columns

**Explanation:** To get a specific number of columns rather than rows per column, calculate wrap_count dynamically. CEILING(n/4, 1) gives the rows needed for 4 columns.

---

### Example 9: Combine with FLATTEN
```
=WRAPCOLS(FLATTEN(A1:D10), 5)
```
**Result:** Flattened 2D array re-wrapped into 5-row columns

**Explanation:** FLATTEN converts a 2D array to a single column, then WRAPCOLS reshapes it with a new structure. This enables complete array restructuring.

---

### Example 10: Wrap FILTER Results
```
=WRAPCOLS(FILTER(Names, Status="Active"), 10, "")
```
**Result:** Active names displayed in columns of 10

**Explanation:** FILTER returns matching names as a column. WRAPCOLS reshapes this for multi-column display. If 35 names match, you get a 10x4 grid with 5 empty-padded cells.

---

### Example 11: Create Fixed-Width Display
```
=WRAPCOLS(A:A, 20, "-")
```
**Result:** Column A data in columns of 20, padded with dashes

**Explanation:** For display purposes, wrapping with a character like "-" clearly shows which cells are padding versus data. Useful for visual layouts.

---

### Example 12: Wrap for Side-by-Side Comparison
```
=WRAPCOLS(VSTACK(Method1Results, Method2Results), 2)
```
**Result:** Results from two methods shown side by side

**Explanation:** Stack two result sets vertically, then wrap with count=2. Each column pair represents one result set. This creates a comparison layout.

---

### Example 13: Transform Time Series for Display
```
=WRAPCOLS(DailyValues, 7)
```
**Result:** Daily values arranged with 7 per column (one week)

**Explanation:** If DailyValues is 28 days of data, this creates a 7x4 grid where each column is a week. Day 1-7 in column 1, day 8-14 in column 2, etc.

---

### Example 14: Dynamic Columns Based on Count
```
=LET(
  data, A1:A100,
  cols, 5,
  rows, CEILING(ROWS(data)/cols, 1),
  WRAPCOLS(data, rows, "")
)
```
**Result:** Data distributed into exactly 5 columns

**Explanation:** To control the number of OUTPUT columns (rather than rows per column), calculate the required rows. LET makes this calculation clean and reusable.

---

### Example 15: Wrap with Headers
```
=VSTACK(
  {"Q1", "Q2", "Q3", "Q4"},
  WRAPCOLS(MonthlyData, 3)
)
```
**Result:** Quarterly headers above monthly data wrapped into 3-month columns

**Explanation:** Wrap monthly data so each column represents a quarter (3 months), then add quarter headers on top with VSTACK.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Vector is 2D (multiple rows AND multiple columns) | WRAPCOLS only accepts 1D input. Use FLATTEN first for 2D arrays, or select just one row/column. |
| `#VALUE!` | wrap_count is zero or negative | wrap_count must be a positive integer (1 or greater). |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula. |
| `#NAME?` | Function not available in your Excel version | WRAPCOLS requires Excel 365/2021 or Google Sheets. |
| `#N/A in results` | Source count not divisible by wrap_count | This is expected—use pad_with parameter to specify different padding. |

## Use Cases

### [[Multi-Column List Display]]

**Scenario:** A long list of items (products, names, codes) needs to be displayed in multiple columns to fit in a report area or to make scrolling unnecessary.

**Implementation:**
```
Single list to 4-column display:
=WRAPCOLS(ProductList, CEILING(ROWS(ProductList)/4, 1), "")

Or specify rows per column directly:
=WRAPCOLS(ProductList, 25, "")
```

**Business Application:** Catalogs, directories, reference sheets. Displaying long lists in newspaper-style columns improves readability and saves vertical space.

**Technical Details:** Calculate wrap_count to achieve desired column count: `rows_per_col = CEILING(total_items / desired_cols, 1)`. Pad with "" for clean appearance.

---

### [[Calendar and Schedule Generation]]

**Scenario:** Creating week-based views from daily data, or month-based views from weekly data. Transform sequential time data into grid layouts.

**Implementation:**
```
Daily data into weekly columns (7 days per column):
=WRAPCOLS(DailyData, 7, "")

Hours into daily columns (24 hours per column):
=WRAPCOLS(HourlyData, 24, "-")

Weeks into monthly columns (4-5 weeks per column):
=WRAPCOLS(WeeklyData, 5, "")
```

**Business Application:** Scheduling, time tracking, resource planning. Calendar-like layouts help visualize time-based patterns.

**Technical Details:** For true calendar layouts, you may need to add day/date headers. The wrap creates the grid; additional formatting creates the calendar context.

---

### [[Data Restructuring for Analysis]]

**Scenario:** Data arrives as a single long column but needs restructuring for pivot tables, comparisons, or specific analysis formats.

**Implementation:**
```
Transform single column to record structure (3 fields per record):
=WRAPCOLS(RawImport, 3, "")

Reshape for year-over-year comparison (12 months per year):
=WRAPCOLS(MonthlyData, 12, 0)
```

**Business Application:** Data transformation, ETL processes, format conversion. Raw data often needs restructuring before analysis.

**Technical Details:** If data follows a repeating pattern (like Name, Address, Phone repeating), wrap_count should match that pattern length to create proper records.

---

### [[Creating Grids for Lookup or Reference]]

**Scenario:** Building lookup grids where sequential values need to be arranged spatially—like seat maps, grid references, or matrix layouts.

**Implementation:**
```
Seat numbers in theater layout (20 seats per row):
=WRAPCOLS(SEQUENCE(200), 20, "-")

Grid reference codes:
=WRAPCOLS(GridCodes, RowsPerSection, "N/A")
```

**Business Application:** Venue management, warehouse locations, any spatial reference system. Convert sequential identifiers to grid positions.

**Technical Details:** The wrap_count determines how many items per column (which appears as rows in the output). For horizontal reading order, consider WRAPROWS instead.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill
- **Input requirement:** Must be 1D (single row or column)
- **Default padding:** #N/A

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand
- **Input requirement:** Same as Excel—must be 1D
- **Default padding:** #N/A (same as Excel)
- **Syntax:** Identical to Excel

### Both Platforms

- Fills column by column (down, then right)
- wrap_count determines rows in output
- Incomplete final column is padded
- Works with formula results (FILTER, SORT, SEQUENCE, etc.)

## Tips and Best Practices

1. **Remember the fill direction:** WRAPCOLS fills DOWN each column before moving RIGHT. Values read vertically in sequence. For horizontal-first filling, use WRAPROWS.

2. **Use pad_with for clean output:** Default #N/A padding can be distracting. Use "" for blank appearance, 0 for numeric contexts, or "-" for explicit placeholders.

3. **Calculate wrap_count for target columns:** To get N columns, use `CEILING(COUNT(data)/N, 1)` as wrap_count. This ensures exactly N columns (or fewer if data is small).

4. **Only 1D input allowed:** WRAPCOLS rejects 2D arrays. Use FLATTEN first to convert 2D to 1D, then WRAPCOLS to reshape: `=WRAPCOLS(FLATTEN(2DArray), rows)`.

5. **Combine with VSTACK for headers:** After wrapping, use VSTACK to add header rows that label each column.

6. **Use SEQUENCE to understand the pattern:** `=WRAPCOLS(SEQUENCE(12), 3)` shows clearly how values map: {1,4,7,10; 2,5,8,11; 3,6,9,12}.

7. **Consider WRAPROWS for different reading order:** If you want to read left-to-right across rows (like text), WRAPROWS is more intuitive. WRAPCOLS reads top-to-bottom in columns.

8. **Handle dynamic data sizes:** Use ROWS() or COUNT() to calculate wrap_count dynamically based on actual data size.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WRAPROWS]] | Wraps vector into rows (fills row by row) | When you want horizontal-first filling (left to right, then down) |
| [[TOCOL]] | Converts array to single column | When going the opposite direction—2D to 1D |
| [[TOROW]] | Converts array to single row | When converting 2D to 1D horizontally |
| [[FLATTEN]] | Flattens array to single column | Similar to TOCOL; use before WRAPCOLS to reshape any array |

### Commonly Used Together

**[[SEQUENCE]]** - Generate sequential numbers

*Combined with WRAPCOLS for numbered grids:*
```
=WRAPCOLS(SEQUENCE(20), 5)
```
Creates a 5x4 grid numbered 1-20 (reading down columns).

---

**[[VSTACK]]** - Add headers to wrapped result

*Combined with WRAPCOLS for labeled display:*
```
=VSTACK({"Col1", "Col2", "Col3", "Col4"}, WRAPCOLS(Data, 5, ""))
```
Add column headers above the wrapped data.

---

**[[FLATTEN]]** - Convert 2D to 1D

*Combined with WRAPCOLS for complete restructuring:*
```
=WRAPCOLS(FLATTEN(2DArray), NewRowCount)
```
Flatten any array, then wrap to new dimensions.

---

**[[FILTER]]** - Filter before wrapping

*Combined with WRAPCOLS for multi-column filtered lists:*
```
=WRAPCOLS(FILTER(List, Criteria), 10, "")
```
Display filtered results in space-efficient columns.

---

**[[HSTACK]]** - Combine wrapped sections

*Combined with WRAPCOLS for categorized layouts:*
```
=HSTACK(WRAPCOLS(Category1, 5, ""), "", WRAPCOLS(Category2, 5, ""))
```
Wrap each category separately, then stack with separator columns.

---

**[[ROWS]]/[[COLUMNS]]** - Calculate dimensions

*Combined with WRAPCOLS for dynamic wrap counts:*
```
=WRAPCOLS(Data, CEILING(ROWS(Data)/4, 1), "")
```
Calculate wrap_count to achieve target column count.

## Official Documentation

- **Microsoft Excel:** [WRAPCOLS function](https://support.microsoft.com/en-us/office/wrapcols-function-d038b05a-57b7-4ee0-be94-ead0ea69c0ab)
- **Google Sheets:** [WRAPCOLS function](https://support.google.com/docs/answer/13190696)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added to match Excel's dynamic array capabilities |

---

*Last updated: 2026-01-10*
