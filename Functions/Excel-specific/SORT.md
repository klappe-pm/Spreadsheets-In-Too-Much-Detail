---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- array-functions
- data-manipulation
subTopics:
- sorting
- data-ordering
- dynamic-arrays
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sort Array
- Dynamic Sort
- Array Sort
tags:
- excel-only
- sorting
- arrays
- dynamic-arrays
---

# SORT

## Description

**SORT** returns a sorted array from a range or array. Unlike the manual Sort feature in Excel's ribbon which modifies data in place, the SORT function creates a new sorted array that spills into adjacent cells while leaving the original data unchanged. This enables dynamic, formula-based sorting that updates automatically when source data changes.

SORT revolutionized data manipulation in Excel by bringing sorting into the formula world. Before SORT, sorted views required VBA, helper columns, or manual re-sorting. Now you can create live sorted lists that automatically adjust when new data is added, values change, or sort criteria are modified. Combined with FILTER, UNIQUE, and other dynamic array functions, SORT enables sophisticated data transformations entirely through formulas.

**Key capabilities:** SORT can sort by any column (not just the first), sort in ascending or descending order, and sort by multiple columns with different sort orders for each. It handles text, numbers, and dates, sorting according to Excel's standard collation rules (numbers before text, case-insensitive for text).

**Platform note:** SORT is available in Microsoft 365 Excel, Excel 2021+, and Google Sheets. However, the syntax differs slightly between platforms. This documentation focuses on the Excel implementation. Google Sheets users should note parameter order differences.

## Syntax

> [!f(x)] SORT Syntax
>
> ```
> =SORT(array, [sort_index], [sort_order], [by_col])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array to sort. |
| `sort_index` | No | The column (or row) number to sort by. Default is 1 (first column). Can be an array for multi-level sorting. |
| `sort_order` | No | 1 for ascending (A-Z, small to large), -1 for descending (Z-A, large to small). Default is 1. Can be an array for multi-level sorting. |
| `by_col` | No | TRUE to sort by columns (horizontal sort), FALSE to sort by rows (vertical sort). Default is FALSE. |

### Return Value

Returns a sorted array with the same dimensions as the input array.

## Examples

> [!f(x)] SORT Examples

### Example 1: Basic Ascending Sort
```
=SORT(A2:C10)
```
**Result:** Data sorted by first column, ascending (A-Z or small-to-large)

**Explanation:** The simplest SORT - sorts by the first column in ascending order. All columns move together to maintain row integrity.

---

### Example 2: Descending Sort
```
=SORT(A2:C10, 1, -1)
```
**Result:** Data sorted by first column, descending (Z-A or large-to-small)

**Explanation:** The third parameter (-1) specifies descending order. Use for "top N" lists like highest sales.

---

### Example 3: Sort by Different Column
```
=SORT(A2:D10, 3)
```
**Result:** Data sorted by third column (ascending)

**Explanation:** The second parameter specifies which column to sort by. Here, column 3 determines the sort order while all columns remain linked.

---

### Example 4: Sort by Multiple Columns
```
=SORT(A2:D10, {1,3}, {1,-1})
```
**Result:** Primary sort by column 1 ascending, secondary sort by column 3 descending

**Explanation:** Arrays in sort_index and sort_order enable multi-level sorting. First sorts by column 1; ties are broken by column 3 (descending).

---

### Example 5: Sort Single Column
```
=SORT(B2:B100)
```
**Result:** Single column sorted ascending

**Explanation:** SORT works on single columns too. Creates a sorted list of values without affecting other columns.

---

### Example 6: Sort Dates
```
=SORT(A2:B50, 1, 1)
```
Where column A contains dates

**Result:** Data sorted chronologically by date (oldest first)

**Explanation:** Dates are stored as numbers, so numeric sorting produces chronological order. Use -1 for most recent first.

---

### Example 7: Horizontal Sort (by Column)
```
=SORT(A1:F1, 1, 1, TRUE)
```
**Result:** Single row sorted horizontally

**Explanation:** The by_col parameter (TRUE) sorts columns instead of rows. Useful for horizontal data layouts.

---

### Example 8: Combined with FILTER
```
=SORT(FILTER(A2:D100, C2:C100="Active"), 2, -1)
```
**Result:** Only "Active" rows, sorted by column 2 descending

**Explanation:** First FILTER extracts matching rows, then SORT orders them. Classic pattern for dynamic filtered and sorted views.

---

### Example 9: Sort UNIQUE Values
```
=SORT(UNIQUE(B2:B500))
```
**Result:** Sorted list of unique values from column B

**Explanation:** Extract distinct values with UNIQUE, then alphabetize with SORT. Perfect for dropdown lists or reference data.

---

### Example 10: Top 5 by Value
```
=TAKE(SORT(A2:C100, 3, -1), 5)
```
**Result:** Top 5 rows based on column 3

**Explanation:** SORT descending puts highest values first; TAKE gets the top 5. This pattern creates dynamic "top N" lists.

---

### Example 11: Sort with Named Range
```
=SORT(SalesData, 4, -1)
```
**Result:** SalesData table sorted by fourth column descending

**Explanation:** Using named ranges makes formulas more readable and maintainable. Named tables also work.

---

### Example 12: Three-Level Sort
```
=SORT(A2:E100, {2,3,1}, {1,1,-1})
```
**Result:** Sort by column 2 (asc), then column 3 (asc), then column 1 (desc)

**Explanation:** Complex sorting with three levels. Array parameters must have matching element counts.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#SPILL!` | Cells in spill range not empty | Clear cells where sorted array needs to appear |
| `#VALUE!` | Sort_index references non-existent column | Ensure sort_index is <= number of columns in array |
| `#VALUE!` | Sort_order array size doesn't match sort_index | Arrays must have same number of elements |
| `#REF!` | Array reference invalid | Check source range exists and is valid |
| No result visible | Single cell selected shows formula | Select cell with SORT formula to see spill range |

## Use Cases

### [[Dynamic Top Performers List]]

**Scenario:** Maintain a live "Top 10" list that automatically updates as data changes.

**Implementation:**
```
=TAKE(SORT(SalesData, 3, -1), 10)
```

**Business Application:** Sales leaderboards, top customers, best-selling products - any ranking that needs to stay current without manual re-sorting.

**Technical Details:** SORT descending puts highest values first. TAKE extracts the top N. When underlying data changes, the list updates automatically.

---

### [[Alphabetized Reference Lists]]

**Scenario:** Create sorted dropdown source lists from dynamic data.

**Implementation:**
```
=SORT(UNIQUE(FILTER(Names, Names<>"")))
```

**Business Application:** Data validation dropdowns showing sorted options, reference tables for lookups, or clean lists for reporting.

**Technical Details:** FILTER removes blanks, UNIQUE removes duplicates, SORT alphabetizes. Chain as needed for your specific requirements.

---

### [[Chronological Event Log]]

**Scenario:** Display events sorted by date with most recent first.

**Implementation:**
```
=SORT(EventLog, 1, -1)
```
Where column 1 contains dates.

**Business Application:** Activity logs, transaction histories, and timeline views all benefit from chronological sorting. Descending puts recent items at top.

**Technical Details:** Dates sort numerically (serial numbers). Format the result columns as dates for proper display.

## Platform Differences

### Microsoft Excel (365/2021+)

| Feature | Support |
|---------|---------|
| Basic sorting | Full support |
| Multi-column sorting | Full support (array parameters) |
| by_col parameter | Full support |
| Dynamic array spilling | Full support |

### Google Sheets

| Feature | Support |
|---------|---------|
| SORT | Available (different syntax) |
| Syntax | `SORT(range, column, is_ascending, [column2, is_ascending2, ...])` |
| Multi-column | Inline parameters instead of arrays |
| by_col equivalent | Not directly available |

**Google Sheets syntax difference:**
```
Excel: =SORT(A2:C10, {1,2}, {1,-1})
Sheets: =SORT(A2:C10, 1, TRUE, 2, FALSE)
```
Google Sheets uses alternating column/order parameters instead of arrays.

### Other Platforms

SORT availability:
- Microsoft Excel 365: Full support
- Microsoft Excel 2021: Full support
- Microsoft Excel 2019: Not available
- Google Sheets: Available (different syntax)
- LibreOffice Calc: Not available (use Data > Sort)
- Apple Numbers: Not available

## Tips and Best Practices

1. **Use with structured tables:** `=SORT(Table1, 3, -1)` is cleaner than cell references and adjusts automatically as the table grows.

2. **Chain with FILTER for filtered sorted views:** `=SORT(FILTER(data, criteria), column, order)` is the most common pattern.

3. **Remember spill behavior:** SORT results spill into adjacent cells. Ensure the target area is clear.

4. **For "Bottom N", sort ascending:** `=TAKE(SORT(data, col, 1), 5)` gets the 5 lowest values.

5. **Multi-column arrays must match:** `{1,2,3}` for columns needs `{1,1,-1}` for orders - same element count.

6. **Use negative numbers in TAKE for bottom results:** `=TAKE(SORT(data, col, 1), -5)` gets last 5 rows.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SORTBY]] | Sort by separate array | When sort column isn't in result array |
| [[FILTER]] | Filter rows by criteria | When you need to filter before or without sorting |
| [[UNIQUE]] | Remove duplicates | Often combined with SORT for clean lists |

### Commonly Used Together

**[[FILTER]]** - Extract rows before sorting

*Filtered and sorted view:*
```
=SORT(FILTER(Data, Status="Active"), DateCol, -1)
```
Filter first, then sort the filtered results.

---

**[[TAKE]]** - Get top/bottom N results

*Top 10 list:*
```
=TAKE(SORT(Data, ValueCol, -1), 10)
```
Sort descending, take first 10 for "Top 10".

---

**[[UNIQUE]]** - Remove duplicates before sorting

*Sorted unique list:*
```
=SORT(UNIQUE(Column))
```
Classic pattern for clean, sorted reference lists.

---

**[[XLOOKUP]] / [[INDEX/MATCH]]** - Lookup in sorted results

*Find value in sorted data:*
```
=XLOOKUP(target, SORT(Data, 1), SORT(Data, 1))
```
Note: SORT doesn't create a lookup table; use XLOOKUP on sorted output if needed.

---

**[[DROP]]** - Remove header rows

*Sort data excluding headers:*
```
=VSTACK(Headers, SORT(DROP(DataWithHeaders, 1), SortCol, SortOrder))
```
DROP removes headers, SORT sorts data, VSTACK reunites them.

## Official Documentation

- **Microsoft Excel:** [SORT function](https://support.microsoft.com/en-us/office/sort-function-22f63bd0-ccc8-492f-953d-c20e8e44b86c)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2018 (dynamic arrays release) | Initial release |
| Excel 2021 | 2021 | Included in perpetual license |
| Excel Online | 2018 | Full support |
| Google Sheets | Original | Different syntax but similar functionality |
| Excel 2019 | Not available | No dynamic array support |

---

*Last updated: 2026-01-10*
