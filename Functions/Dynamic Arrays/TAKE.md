---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - dynamic-arrays
  - data-extraction
subTopics:
  - array-slicing
  - row-extraction
  - column-extraction
  - data-subsetting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - extract-rows
  - extract-columns
  - first-n-rows
  - last-n-rows
  - array-head
  - array-tail
tags:
  - dynamic-arrays
  - data-extraction
  - array-manipulation
  - excel
  - google-sheets
  - subsetting
---

# TAKE

## Description

TAKE is a dynamic array function that extracts a specified number of rows and/or columns from the beginning or end of an array or range. Given an array of data, TAKE returns a subset containing the first N rows (if positive) or the last N rows (if negative), and similarly for columns. This makes it incredibly easy to grab "the first 5 items," "the last 10 records," or "the top-left 3x3 corner" of any dataset without complex INDEX formulas or manual range references. The function returns a proper array that can spill into adjacent cells or be used within other array-aware functions.

TAKE is particularly valuable in data analysis workflows where you need to work with subsets of larger datasets. Common use cases include displaying the most recent N transactions, showing the top or bottom performers after sorting, extracting header rows from data ranges, creating preview summaries of large datasets, and building dashboards that display limited data for readability. TAKE pairs exceptionally well with SORT and SORTBY to create "top N" and "bottom N" reports: sort your data by a criteria column, then TAKE the first or last few rows. It also works beautifully with FILTER to take a limited number of results from filtered data.

There are several important gotchas when using TAKE. First, if you request more rows or columns than exist in the array, TAKE returns a #VALUE! error rather than returning what's available. Second, TAKE cannot take zero rows or columns; specifying 0 for either parameter causes an error. Third, when using negative numbers to take from the end, -1 means the last row/column, -2 means the last two, and so on, which can be confusing at first. Fourth, TAKE works on the array dimensions, not data content, so if your source has blank rows, they count toward the take count. Fifth, when combining TAKE with other functions, be aware that the order of operations matters: TAKE(SORT(...)) and SORT(TAKE(...)) produce different results.

Regarding platform availability, TAKE is available in Microsoft Excel 365 (introduced in 2022) and Excel 2024, but it is NOT available in Google Sheets as of 2026. Google Sheets users must use alternative approaches such as ARRAY_CONSTRAIN, INDEX with SEQUENCE, or QUERY with LIMIT. This is a significant consideration for workbook compatibility when sharing between platforms. In Excel, TAKE is part of the modern dynamic array function family and returns spill arrays that automatically expand. The function is not available in Excel 2019 or earlier versions. When building workbooks for mixed environments, consider providing Google Sheets alternatives or avoiding TAKE for cross-platform files.

## Syntax

> [!f(x)] TAKE Syntax
>
> ```excel
> =TAKE(array, rows, [columns])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `array` | The source array or range from which to extract data. Can be a cell range, named range, table reference, or an array returned by another function. | Yes |
> | `rows` | The number of rows to extract. Positive values take from the beginning (top), negative values take from the end (bottom). Cannot be 0. If omitted or empty string, all rows are returned. | Yes |
> | `columns` | The number of columns to extract. Positive values take from the left, negative values take from the right. Cannot be 0. If omitted, all columns are returned. | No |

## Examples

### Example 1: Take First 5 Rows
```excel
=TAKE(A2:D100, 5)
```
**Result:** Returns rows 2-6 (first 5 data rows) with all 4 columns

**Explanation:** The simplest TAKE usage extracts the first N rows from a range. Since no column parameter is specified, all columns (A through D) are included. This is equivalent to manually selecting A2:D6 but dynamically sized.

---

### Example 2: Take Last 3 Rows
```excel
=TAKE(A2:D100, -3)
```
**Result:** Returns the last 3 rows of data with all columns

**Explanation:** Negative row values take from the end. -3 returns the bottom three rows of the array. This is perfect for showing "most recent" records when data is in chronological order with newest at bottom.

---

### Example 3: Take First 5 Rows and First 2 Columns
```excel
=TAKE(A1:E100, 5, 2)
```
**Result:** Returns a 5-row by 2-column subset from the top-left

**Explanation:** Both parameters limit the extraction. This takes rows 1-5 and columns A-B only, creating a rectangular subset from the top-left corner of the source range.

---

### Example 4: Take Last 10 Rows, Last 3 Columns
```excel
=TAKE(A1:F50, -10, -3)
```
**Result:** Returns a 10-row by 3-column subset from the bottom-right

**Explanation:** Negative values for both parameters extract from the bottom-right corner. This takes the last 10 rows and the rightmost 3 columns, useful for recent data in the last few columns.

---

### Example 5: Top 5 After Sorting (Top N Pattern)
```excel
=TAKE(SORT(A2:C100, 3, -1), 5)
```
**Result:** Returns the 5 rows with highest values in column 3

**Explanation:** This is the classic "Top N" pattern. SORT arranges data by column 3 in descending order (-1), then TAKE extracts the first 5 rows, which are now the top 5 performers. Essential for leaderboards and rankings.

---

### Example 6: Bottom 5 After Sorting (Bottom N Pattern)
```excel
=TAKE(SORT(A2:C100, 3, 1), 5)
```
**Result:** Returns the 5 rows with lowest values in column 3

**Explanation:** Sort in ascending order (1) puts smallest values first, then TAKE grabs those first 5 rows. Alternatively, you could sort descending and use TAKE with -5.

---

### Example 7: Take Header Row Only
```excel
=TAKE(A1:E100, 1)
```
**Result:** Returns just the first row (headers) of the range

**Explanation:** Taking 1 row is a clean way to extract headers from a dataset. Combine with DROP(range, 1) to separate headers from data in different cells.

---

### Example 8: Take All Rows, First Column Only
```excel
=TAKE(A1:E100, ROWS(A1:E100), 1)
```
**Result:** Returns all rows but only the first column

**Explanation:** Using ROWS() as the row parameter effectively takes "all rows." Combined with column parameter of 1, this extracts just column A. This is similar to A1:A100 but works dynamically with array results.

---

### Example 9: First N from Filtered Results
```excel
=TAKE(FILTER(A2:D100, B2:B100="Active"), 10)
```
**Result:** Returns first 10 rows where column B equals "Active"

**Explanation:** FILTER first extracts all matching rows, then TAKE limits to the first 10. Note: if fewer than 10 rows match the filter, this returns #VALUE! error. Use IFERROR or check counts first.

---

### Example 10: Dynamic Top N with Cell Reference
```excel
=TAKE(SORT(A2:C100, 3, -1), E1)
```
Where E1 contains the number 10

**Result:** Returns top N rows based on the value in E1

**Explanation:** The rows parameter can reference a cell, enabling dynamic "show top N" controls. Users can change E1 to see top 5, top 10, top 20, etc., and the output adjusts automatically.

---

### Example 11: Extract Specific Corner
```excel
=TAKE(TAKE(A1:J20, 5), , -2)
```
**Result:** First 5 rows, last 2 columns

**Explanation:** Nested TAKE functions can combine operations. The inner TAKE gets first 5 rows (all columns), then the outer TAKE gets the last 2 columns of that result. The empty second parameter in the outer TAKE means "all rows."

---

### Example 12: Recent Transactions Summary
```excel
=TAKE(SORT(Transactions, 1, -1), 5, 3)
```
Where Transactions is a named range or table

**Result:** 5 most recent transactions with first 3 columns

**Explanation:** Sort by date column (column 1, descending), take first 5 (most recent), limit to first 3 columns (maybe Date, Description, Amount). Perfect for dashboard widgets showing recent activity.

---

### Example 13: Combining with HSTACK/VSTACK
```excel
=HSTACK(TAKE(A:A, 10), TAKE(C:C, 10))
```
**Result:** First 10 values from column A next to first 10 values from column C

**Explanation:** TAKE each column separately, then HSTACK combines them horizontally. This creates a custom subset taking specific columns while TAKE handles the row limiting.

---

### Example 14: Safe TAKE with Error Handling
```excel
=IFERROR(TAKE(FILTER(A2:D100, B2:B100>1000), 5), "Fewer than 5 results")
```
**Result:** First 5 filtered rows, or error message if fewer than 5 exist

**Explanation:** Since TAKE errors when requesting more than available, wrap in IFERROR for graceful handling. Alternatively, use MIN with ROWS to ensure the take count doesn't exceed available rows.

---

### Example 15: Last Row Only (Single Record)
```excel
=TAKE(A2:D100, -1)
```
**Result:** Returns only the last row of data

**Explanation:** Taking -1 rows extracts just the final row. Useful for "current status" displays, latest entry tracking, or extracting the end of a growing dataset.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Requested more rows/columns than exist in the array | Check array dimensions first with ROWS()/COLUMNS(), use MIN to limit take count |
| `#VALUE!` | Rows or columns parameter is 0 | Use positive or negative non-zero integers only |
| `#VALUE!` | Array is empty (e.g., from FILTER with no matches) | Handle empty filter results with IFERROR or IF(ROWS(filtered)=0,...) |
| `#NAME?` | TAKE function not recognized (older Excel or Google Sheets) | Use Excel 365/2024; for Sheets use ARRAY_CONSTRAIN or INDEX alternatives |
| `#SPILL!` | Destination cells for spill array are not empty | Clear cells where the result needs to spill |
| `#REF!` | Source range references deleted cells | Update the array reference to valid cells |
| `#CALC!` | Calculation error in source array | Debug the source array formula first |

## Use Cases

### [[Top N / Bottom N Reports]]
- **Implementation**: Combine SORT or SORTBY with TAKE to create ranked lists. SORT by the ranking column in descending order for top N, ascending for bottom N, then TAKE the desired count from the sorted results.
- **Business Application**: Sales leaderboards showing top performers, product rankings by revenue, customer lists by purchase frequency, expense reports highlighting largest costs, performance dashboards with worst-performing metrics requiring attention.
- **Technical Details**: Use SORTBY when you need to sort by one column but display others. Remember that SORT operates on all columns together. For ties, add secondary sort columns. Consider using TAKE with negative values on ascending-sorted data as alternative to descending sort with positive TAKE.

### [[Recent Records Display]]
- **Implementation**: For chronologically ordered data (newest at bottom), use TAKE with negative row count to extract the most recent entries. Combine with SORT by date descending if data isn't pre-ordered.
- **Business Application**: Dashboard widgets showing latest transactions, recent customer interactions, new order notifications, activity logs with most recent events, audit trails displaying current activity.
- **Technical Details**: If source data has newest first, use positive TAKE. If newest is last, use negative. For truly dynamic "latest N," ensure the source range extends far enough to capture future growth or use table references that auto-expand.

### [[Data Preview and Sampling]]
- **Implementation**: Extract a small sample from large datasets for preview purposes, documentation, or testing. Use TAKE to grab representative rows without loading entire datasets into views.
- **Business Application**: Report previews before final generation, data validation checks on import files, sample displays in documentation, quick data health checks showing first/last records, proof-of-concept demonstrations.
- **Technical Details**: Combine with UNIQUE to preview distinct values, or with FILTER to preview matching records. For random samples, consider SORTBY with RANDARRAY before TAKE. Remember samples may not be statistically representative.

### [[Array Subsetting for Calculations]]
- **Implementation**: Extract portions of arrays for focused calculations. Take the last 12 months for year-over-year comparisons, first quarter for projections, or specific row/column ranges for matrix operations.
- **Business Application**: Financial modeling comparing periods, trend analysis on recent data, rolling calculations on trailing periods, extracting training data subsets, isolating data windows for statistical tests.
- **Technical Details**: Chain TAKE with aggregation functions: AVERAGE(TAKE(data, -30)) for average of last 30 values. Combine with LET for clarity: LET(recent, TAKE(data, -12), AVERAGE(recent)). Use negative values for trailing periods, positive for leading.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | Excel 365, Excel 2024 | NOT AVAILABLE |
| **Alternative in Sheets** | N/A | ARRAY_CONSTRAIN, INDEX with SEQUENCE, QUERY with LIMIT |
| **Dynamic Arrays** | Full spill behavior | N/A |
| **Row Parameter** | Required | N/A |
| **Negative Values** | Fully supported (take from end) | N/A |
| **Empty Parameter** | Returns all rows/columns when omitted | N/A |
| **Integration with SORT** | Seamless | N/A |
| **Integration with FILTER** | Seamless | N/A |

**Google Sheets Alternatives:**

Since TAKE is not available in Google Sheets, use these alternatives:

1. **ARRAY_CONSTRAIN** - Limits array dimensions (similar to positive TAKE):
```
=ARRAY_CONSTRAIN(A2:D100, 5, 4)
```
Returns first 5 rows with 4 columns. Cannot take from end like negative TAKE.

2. **INDEX with SEQUENCE** - More flexible but complex:
```
=INDEX(A2:D100, SEQUENCE(5), SEQUENCE(1, 4))
```
Returns first 5 rows with 4 columns. Can be adapted for bottom rows with ROWS()-based calculations.

3. **QUERY with LIMIT** - For simple cases:
```
=QUERY(A2:D100, "SELECT * LIMIT 5")
```
Returns first 5 rows. Use "ORDER BY" before LIMIT for top/bottom N patterns.

**Key Recommendations by Platform:**

For Microsoft Excel:
- Use TAKE freely in Excel 365/2024 environments
- Combine with SORT, FILTER, and other dynamic array functions
- Consider backward compatibility if sharing with Excel 2019 or earlier users

For Google Sheets:
- Use ARRAY_CONSTRAIN for simple "first N" extractions
- Use QUERY with LIMIT and ORDER BY for top/bottom N patterns
- Use INDEX/SEQUENCE combinations for complex subsetting
- Be aware that none perfectly replicate TAKE's negative row capability

## Tips and Best Practices

1. **Combine with SORT for Top/Bottom N**: The most powerful TAKE pattern is `=TAKE(SORT(data, column, -1), N)` for top N. Make this your go-to for leaderboards and rankings.

2. **Handle Insufficient Rows**: TAKE errors if you request more than available. Use `=TAKE(data, MIN(N, ROWS(data)))` or wrap in IFERROR to handle edge cases gracefully.

3. **Use Negative Values for Recent Data**: When your data grows downward over time, `-N` gives you the most recent entries without knowing the exact row count. This is more robust than calculating `ROWS()-N`.

4. **Pair TAKE with DROP**: DROP is TAKE's complement (removes instead of returns). Use DROP(data, 1) to skip headers, TAKE(data, -10) for last 10, or combine them for middle sections.

5. **Remember Order Matters**: TAKE(SORT(data)) and SORT(TAKE(data)) are different. Usually you want to sort first (to rank), then take (to limit). Taking then sorting just sorts the subset.

6. **Use Cell References for Dynamic Limits**: Put your "Top N" count in a cell and reference it: `=TAKE(sorted, E1)`. Users can then control the output size interactively.

7. **Extract Columns with Empty Row Parameter**: `=TAKE(data, , 3)` takes first 3 columns only (all rows). The empty parameter means "no limit" for that dimension.

8. **Validate Source Before Taking**: If TAKE's source is FILTER or another function that might return nothing, the TAKE will error. Check `IF(ROWS(source)>=N, TAKE(source, N), "Not enough data")`.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from TAKE |
|----------|-------------|-------------------------|
| [[DROP]] | Removes rows/columns from beginning or end | DROP removes what TAKE would return; they're complementary |
| [[CHOOSEROWS]] | Selects specific rows by number | CHOOSEROWS picks arbitrary rows; TAKE gets contiguous runs from start/end |
| [[CHOOSECOLS]] | Selects specific columns by number | CHOOSECOLS picks arbitrary columns; TAKE gets contiguous runs from left/right |
| [[INDEX]] | Returns value at specific row/column intersection | INDEX gets single values or one dimension; TAKE gets rectangular blocks |
| [[ARRAY_CONSTRAIN]] | Limits array size (Google Sheets) | Sheets alternative; only takes from beginning, not end |

### Commonly Used Together

**[[SORT]]** - Sort arrays by column values

*Use SORT with TAKE for top/bottom N patterns:*
```excel
=TAKE(SORT(A2:C100, 3, -1), 5)
```
Sort by column 3 descending, then take first 5 (highest values). The classic leaderboard formula.

**[[SORTBY]]** - Sort arrays by external criteria

*Use SORTBY with TAKE when sort column isn't in the result:*
```excel
=TAKE(SORTBY(A2:C100, D2:D100, -1), 10)
```
Sort by column D (descending) but only return columns A-C for the top 10.

**[[FILTER]]** - Filter arrays by criteria

*Use FILTER with TAKE to limit filtered results:*
```excel
=TAKE(FILTER(A2:D100, B2:B100>100), 5)
```
Filter to rows where column B exceeds 100, then take only first 5 matches.

**[[DROP]]** - Remove rows/columns from arrays

*Use DROP and TAKE together for middle sections:*
```excel
=TAKE(DROP(A1:D100, 5), 10)
```
Drop first 5 rows, then take next 10. Returns rows 6-15 of original data.

**[[HSTACK]]/[[VSTACK]]** - Combine arrays

*Use TAKE with HSTACK to combine subsets:*
```excel
=HSTACK(TAKE(A2:A50, 10), TAKE(D2:D50, 10))
```
Take first 10 from two different columns and combine them side by side.

**[[LET]]** - Define named variables

*Use LET with TAKE for readable complex formulas:*
```excel
=LET(sorted, SORT(A2:C100, 3, -1), top5, TAKE(sorted, 5), top5)
```
Name intermediate results for clarity when building multi-step extractions.

## Official Documentation

- **Microsoft Excel**: [TAKE function](https://support.microsoft.com/en-us/office/take-function-25382ff1-5da1-4f78-ab43-f33bd2e4e003)
- **Google Sheets**: Not available - Use ARRAY_CONSTRAIN, INDEX, or QUERY as alternatives

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 365 | August 2022 | Initial release as part of text and array function update |
| Excel 2024 | 2024 | Included in perpetual license version |
| Excel for Web | 2022 | Full support added to browser version |
| Google Sheets | - | Not available; use ARRAY_CONSTRAIN or QUERY as alternatives |

---

*Last updated: 2026-01-10*
