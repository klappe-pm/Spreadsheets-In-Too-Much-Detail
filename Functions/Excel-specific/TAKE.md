---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- dynamic-arrays
- array-manipulation
subTopics:
- array-extraction
- first-last-elements
- data-trimming
- row-column-selection
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Take Array
- First N Rows
- Last N Rows
- Array Subset
tags:
- take
- dynamic-arrays
- extraction
- array-manipulation
- excel-365
---

# TAKE

## Description

**TAKE** returns a specified number of contiguous rows or columns from the start or end of an array. Need the first 5 rows? `=TAKE(Data, 5)`. Need the last 3 columns? `=TAKE(Data, , -3)`. This function provides the simplest way to extract leading or trailing portions of arrays without complex INDEX formulas or CHOOSEROWS/CHOOSECOLS with SEQUENCE.

The function elegantly handles both row and column extraction in a single call. Positive numbers take from the start (first N rows/columns), while negative numbers take from the end (last N rows/columns). You can specify rows only, columns only, or both simultaneously. This dual capability makes TAKE incredibly versatile for data preview, top/bottom analysis, and array sizing.

**TAKE in Google Sheets:** Google Sheets added TAKE in 2022, providing identical functionality to Excel. The syntax, behavior, and positive/negative number interpretation are the same. Formulas using TAKE are fully portable between Excel 365/2021 and Google Sheets.

**Complement to DROP:** TAKE and DROP are inverse operations. TAKE keeps the first/last N elements; DROP removes them. Together they provide complete control over array boundaries. Use TAKE when you want to keep a specific count from start/end; use DROP when you want to remove a specific count from start/end.

## Syntax

> [!f(x)] TAKE Syntax
>
> ```
> =TAKE(array, [rows], [columns])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to extract rows/columns. Can be a cell range, named range, or array returned by another function. |
| `rows` | No | Number of rows to take. Positive = from start, negative = from end. Omit or use empty to take all rows. |
| `columns` | No | Number of columns to take. Positive = from start, negative = from end. Omit or use empty to take all columns. |

### Return Value

Returns an array containing the specified rows and/or columns from the source array. If both rows and columns are specified, returns the intersection.

## Examples

> [!f(x)] TAKE Examples

### Example 1: First N Rows
```
=TAKE(A1:E100, 5)
```
**Result:** First 5 rows with all columns

**Explanation:** Most common use - preview top of data or get first N records. All columns included.

---

### Example 2: Last N Rows
```
=TAKE(A1:E100, -5)
```
**Result:** Last 5 rows with all columns

**Explanation:** Negative number takes from end. Perfect for "most recent" data or bottom of sorted lists.

---

### Example 3: First N Columns
```
=TAKE(A1:E100, , 3)
```
**Result:** All rows but only first 3 columns

**Explanation:** Empty rows parameter keeps all rows. Take only leftmost columns.

---

### Example 4: Last N Columns
```
=TAKE(A1:E100, , -2)
```
**Result:** All rows but only last 2 columns

**Explanation:** Get rightmost columns regardless of total column count.

---

### Example 5: Rows and Columns Together
```
=TAKE(A1:E100, 10, 3)
```
**Result:** 10x3 subset from top-left corner

**Explanation:** First 10 rows AND first 3 columns simultaneously.

---

### Example 6: Top Rows, Last Columns
```
=TAKE(A1:E100, 5, -2)
```
**Result:** First 5 rows, but only last 2 columns

**Explanation:** Mix positive/negative for different directions per dimension.

---

### Example 7: From Sorted Data (Top N)
```
=TAKE(SORT(Sales, 2, -1), 10)
```
**Result:** Top 10 highest values after sorting

**Explanation:** Sort descending by column 2, then take first 10. Classic top-N pattern.

---

### Example 8: From Filtered Data
```
=TAKE(FILTER(Data, Criteria), 5)
```
**Result:** First 5 matching rows

**Explanation:** Filter first, then take top 5 from results. Limits output size.

---

### Example 9: With UNIQUE for Top Distinct Values
```
=TAKE(UNIQUE(A1:A1000), 10)
```
**Result:** First 10 unique values

**Explanation:** Get unique values, then take first 10. Useful for category lists.

---

### Example 10: Bounded Output for Dashboard
```
=TAKE(SORT(Metrics, -1), 5, 3)
```
**Result:** Exactly 5x3 array of top sorted values

**Explanation:** Dashboard cells need fixed dimensions. TAKE guarantees maximum size.

---

### Example 11: Single Row/Column
```
=TAKE(A1:E10, 1)
```
**Result:** First row only (header extraction)

**Explanation:** Take 1 row extracts just that row. Simple header extraction.

---

### Example 12: Combine with DROP
```
=TAKE(DROP(Data, 1), 5)
```
**Result:** Rows 2-6 (skip header, take next 5)

**Explanation:** DROP removes header, TAKE limits remaining data. Common pattern.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Rows/columns parameter is 0 | Zero is invalid; use positive or negative numbers only |
| `#VALUE!` | Requesting more rows/columns than exist | Request count cannot exceed array dimensions |
| `#SPILL!` | Not enough room for result array | Clear cells in the spill range |
| `#REF!` | Array reference is invalid | Verify the source array exists |

## Use Cases

### [[Top N Analysis]]

**Scenario:** Display top 10 salespeople by revenue without showing entire company list.

**Implementation:**
```
=TAKE(SORT(SalesData, RevenueColumn, -1), 10)
```

**Business Application:** Executive dashboards, leaderboards, performance summaries. Focus on top performers without data overload.

**Technical Details:** SORT descending first, then TAKE top 10. If fewer than 10 records exist, TAKE returns all available.

---

### [[Data Preview]]

**Scenario:** Show sample of large dataset for validation before full processing.

**Implementation:**
```
=TAKE(ImportedData, 20, 5)
```

**Business Application:** Quick data validation, sample review, column structure verification.

**Technical Details:** Shows top-left 20x5 portion. Adjust counts based on display area.

---

### [[Recent Records]]

**Scenario:** Display most recent 10 transactions from chronologically ordered data.

**Implementation:**
```
=TAKE(TransactionLog, -10)
```

**Business Application:** Activity feeds, recent orders, latest entries in time-series data.

**Technical Details:** Negative row count takes from bottom. Data should be chronologically ordered (oldest first).

---

### [[Column Subset for Export]]

**Scenario:** Export only first 5 columns (key fields) from a wide dataset.

**Implementation:**
```
=TAKE(FullData, , 5)
```

**Business Application:** Data exports with column limits, simplified views, essential fields only.

**Technical Details:** Empty rows parameter preserves all rows while limiting columns.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Negative numbers:** Fully supported for end-relative extraction
- **Array behavior:** Automatic spilling

### Google Sheets
- **Availability:** Added in 2022
- **Negative numbers:** Fully supported
- **Array behavior:** Identical spill behavior

### Key Differences
TAKE works identically across both platforms. Formulas are fully portable.

## Tips and Best Practices

1. **Use negative for "last N":** Negative numbers take from end. More reliable than calculating offsets.

2. **Empty parameter keeps all:** Use `=TAKE(Data,,3)` for all rows, first 3 columns.

3. **Combine with SORT for Top N:** Sort descending, then TAKE first N for top performers.

4. **Pair with DROP for slicing:** DROP removes from start/end; TAKE keeps. Use together for middle extraction.

5. **Bounds checking:** If you request more rows than exist, you get an error. Consider wrapping in IFERROR or using MIN.

6. **Consider CHOOSEROWS for non-contiguous:** TAKE only gets contiguous start/end. Use CHOOSEROWS for specific rows.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DROP]] | Removes first/last rows or columns | When removing is easier than keeping |
| [[CHOOSEROWS]] | Extracts specific rows by number | When you need non-contiguous rows |
| [[CHOOSECOLS]] | Extracts specific columns by number | When you need non-contiguous columns |
| [[INDEX]] | Returns value at position | When you need a single cell, not array |

### Commonly Used Together

**[[SORT]]** - Sort before taking top/bottom
```
=TAKE(SORT(Data, 2, -1), 5)
```

**[[DROP]]** - Skip then take
```
=TAKE(DROP(Data, 1), 10)
```

**[[FILTER]]** - Filter then limit
```
=TAKE(FILTER(Data, Criteria), 5)
```

## Official Documentation

- **Microsoft Excel:** [TAKE function](https://support.microsoft.com/en-us/office/take-function-25382ff1-5da1-4f78-ab43-f33bd2e4e003)
- **Google Sheets:** [TAKE function](https://support.google.com/docs/answer/13190081)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions |
| Excel 2021 | October 2021 | Included in perpetual license |
| Google Sheets | 2022 | Added to match Excel |
| Excel 2019 | Not available | Use INDEX or CHOOSEROWS workarounds |

---

*Last updated: 2026-01-10*
